# Manual de Despliegue y Evaluación

Esta guía está diseñada para facilitar la evaluación del proyecto en un entorno local estándar utilizando **XAMPP**.

## Requisitos del Entorno

*   **Software**: XAMPP (Apache + PHP 8.1 o superior).
*   **Base de Datos**: SQLite 3 (Nativa en PHP, no requiere servicio MySQL activo).
*   **Sistema Operativo**: Windows 10/11.

## Instrucciones de Instalación Paso a Paso

### 1. Ubicación de Archivos
Copie la carpeta del proyecto `PROYECTOWEB` (o el contenido del repositorio) dentro del directorio raíz del servidor web:
`C:\xampp\htdocs\PROYECTOWEB`

### 2. Verificación de Permisos
El sistema necesita crear y escribir en la base de datos y la carpeta de subidas. Asegúrese de que las siguientes carpetas tengan permisos de escritura:
*   `/db` (Aquí se generará automáticamente `layerhub.sqlite`)
*   `/public/uploads`

### 3. Configuración de Acceso
El sistema ya viene preconfigurado para ejecutarse en la carpeta `/PROYECTOWEB`.

Si abre el archivo `/php/includes/config.php`, verá la siguiente configuración automática:

```php
// Detección Automática de Dominio
$protocol = isset($_SERVER['HTTPS']) && $_SERVER['HTTPS'] === 'on' ? "https" : "http";
$host = $_SERVER['HTTP_HOST'];
define('BASE_URL', "$protocol://$host/PROYECTOWEB");
```

Esto permite que el proyecto funcione inmediatamente al descomprimirlo en `htdocs/PROYECTOWEB`.

## Ejecución y Pruebas

1.  Inicie el módulo **Apache** en el panel de control de XAMPP.
2.  Abra su navegador y acceda a: `http://localhost/PROYECTOWEB/public/index.php`.
3.  **Nota Importante**: Al cargar la página por primera vez, el sistema detectará que no existe la base de datos y ejecutará automáticamente el script de inicialización (`config.php -> initializeDatabase()`), creando todas las tablas e insertando los siguientes datos de prueba:

### Datos de Acceso (Seeds)

| Rol | Usuario/Email | Contraseña |
| :--- | :--- | :--- |
| **Administrador** | `admin` / `admin@layerhub.com` | `admin123` |
| **Cliente** | `cliente` / `cliente@layerhub.com` | `cliente123` |

## Visualización de Documentación Técnica

Para visualizar esta memoria técnica en formato web:
1.  Esta documentación se encuentra generada estáticamente en la URL del repositorio GitHub.
2.  Alternativamente, puede navegar por los archivos Markdown en la carpeta `/docs/` del proyecto entregado.
