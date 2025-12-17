# Sistema de Gestión de Prácticas Profesionales (v2.3)

Este proyecto es una plataforma web integral para la administración, seguimiento y control de prácticas profesionales. Utiliza una arquitectura desacoplada con un Frontend estático (SPA) y un Backend automatizado basado en n8n sobre Docker.

## 📋 Características Técnicas

* **Frontend:** Single Page Application (SPA) construida con HTML5, JavaScript (Vanilla) y Tailwind CSS.
* **Backend:** Orquestación de flujos de trabajo mediante n8n (Dockerizado).
* **Base de Datos:** Google Sheets (vía API).
* **Gestión Documental:** Google Drive y Google Docs (Generación de cartas de presentación en PDF).
* **Notificaciones:** Gmail (Envío de correos transaccionales).
* **Seguridad:** Panel de administración protegido por credenciales y manejo de CORS configurado.

## 🛠️ Requisitos del Servidor

* **Sistema Operativo:** Ubuntu Server 24.04 LTS (o superior).
* **Motor de Contenedores:** Docker Engine & Docker Compose.
* **Red:** Puertos 80 (Web) y 5678 (n8n) habilitados.
* **Dependencias Externas:** Proyecto en Google Cloud Platform con credenciales OAuth 2.0 habilitadas para Sheets, Drive, Docs y Gmail.

## 🚀 Instalación del Backend (n8n)

1.  **Crear directorio de trabajo:**
    ```bash
    mkdir ~/sistema-practicas && cd ~/sistema-practicas
    ```

2.  **Crear archivo `docker-compose.yml`:**
    Es crítico incluir las variables de entorno `N8N_HEADER_ACCESS_CONTROL_ALLOW_ORIGIN` para evitar errores de CORS con el frontend.

    ```yaml
    version: '3.8'
    services:
      n8n:
        image: n8nio/n8n:latest
        container_name: n8n_practicas
        restart: always
        ports:
          - "5678:5678"
        environment:
          - N8N_HEADER_ACCESS_CONTROL_ALLOW_ORIGIN=*
          - GENERIC_TIMEZONE=America/Santiago
          - TZ=America/Santiago
        volumes:
          - n8n_data:/home/node/.n8n

    volumes:
      n8n_data:
    ```

3.  **Iniciar el servicio:**
    ```bash
    docker-compose up -d
    ```

4.  **Configuración Inicial:**
    Accede a `http://10.40.5.13/:5678` y configura la cuenta de usuario administrador.

## ⚙️ Configuración del Flujo (Workflow)

1.  **Importar Workflow:**
    * En n8n, ir a `Workflows` > `Import from File`.
    * Cargar el archivo entregable: `Sistema Prácticas - PROD (Final Finalizada Fix) (1).json`.

2.  **Configurar Credenciales:**
    Es necesario configurar cuentas OAuth2 dentro de n8n para los siguientes servicios:
    * Google Sheets OAuth2 API
    * Google Drive OAuth2 API
    * Google Docs OAuth2 API
    * Gmail OAuth2 API

3.  **Vincular Recursos:**
    * **Google Sheets:** Verificar que los nodos apunten a la hoja "Estudiantes".
    * **Google Drive:** Verificar la carpeta de destino y el ID de la plantilla Docs.

4.  **Activar el Flujo:**
    * Cambiar el interruptor superior derecho de **Inactive** a **Active** (Verde).

## 🖥️ Despliegue del Frontend

1.  **Configurar Conexión:**
    Abrir el archivo `index.html` y localizar la constante de configuración (Línea ~650):
    ```javascript
    const N8N_BASE_URL = '[http://10.40.5.13:5678](http://10.40.5.13:5678)';
    ```
    Reemplazar la IP si el servidor de despliegue cambia.

2.  **Opción A (Servidor Web - Recomendado):**
    Copiar el archivo `index.html` al directorio público de su servidor web (ej. `/var/www/html/` en Apache/Nginx).

3.  **Opción B (Local):**
    Gracias a la configuración CORS del backend, el archivo puede abrirse directamente en el navegador con doble clic.

## 🔐 Credenciales de Acceso (Coordinador)

Para acceder al panel de administración (Buscar, Actualizar, Reportes):

* **Usuario:** `usuario`
* **Contraseña:** `Unab.2025`



## 📄 Licencia
Proyecto Académico - Grupo 7 Seguimiento de práctica profesional.