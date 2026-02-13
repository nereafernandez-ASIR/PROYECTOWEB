# Arquitectura del Sistema

## Estructura de Directorios

El proyecto sigue una arquitectura **MVC (Modelo-Vista-Controlador)** adaptada a PHP nativo, separando claramente la lógica de negocio, la capa de datos y la interfaz de usuario.

```text
/
├── db/                 # Capa de Datos (SQLite)
├── php/                # Lógica de Negocio (Backend)
│   ├── auth/           # Controladores de Sesión (Login, Registro)
│   ├── community/      # Controladores de Lógica (Subidas, Ratings)
│   └── includes/       # Núcleo del Sistema (Config, Session)
├── public/             # Capa de Presentación (Frontend)
│   ├── admin/          # Vistas Privadas (Administración)
│   ├── css/            # Hojas de Estilo
│   ├── js/             # Scripts de Cliente
│   ├── uploads/        # Almacenamiento de Archivos de Usuario
│   └── index.php       # Punto de Entrada Público
```

## Interoperabilidad PHP

El sistema funciona mediante un flujo de inclusión de dependencias jerárquico. Cada archivo accesible públicamente (en `public/`) inicia su ejecución cargando el núcleo del sistema.

### Diagrama de Flujo de Carga

```mermaid
graph TD
    A[Public Page (Vista)] -->|require_once| B[session.php]
    B -->|require_once| C[config.php]
    C -->|Database::getInstance| D[Conexión SQLite]
    A -->|Lógica Específica| E[Renderizado HTML]
```

### Componentes del Núcleo

1.  **session.php**: Actúa como un *Middleware* de autenticación. Se encarga de iniciar la sesión de PHP, verificar si el usuario está bloqueado y proporcionar funciones globales como `isLoggedIn()` e `isAdmin()`.
2.  **config.php**: Contiene la configuración global (`BASE_URL`, rutas), la conexión a base de datos (Clase `Database`) y funciones auxiliares de seguridad (`sanitize()`, `redirect()`).

## Ciclo de Vida de una Petición (Request-Response)

1.  **Petición**: El navegador solicita `public/tienda.php`.
2.  **Inicialización**: Se carga el núcleo (`session.php` + `config.php`).
3.  **Verificación**: Se comprueba si existe una sesión válida.
4.  **Consulta**: La vista solicita datos a la BD mediante `getDB()`.
5.  **Renderizado**: PHP genera el HTML dinámico inyectando los datos.
6.  **Respuesta**: El servidor envía el documento HTML final al cliente.
