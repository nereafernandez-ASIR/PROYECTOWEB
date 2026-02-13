# Arquitectura del Sistema

LayerHub utiliza una estructura modular que separa la lógica del negocio de la presentación visual.

## 📂 Estructura de Directorios

```text
/
├── docs/               # Archivos fuente de esta documentación (Markdown)
├── php/                # Lógica de Negocio (Backend)
│   ├── auth/           # Login, Registro y Logout
│   ├── community/      # Procesadores de subidas, ratings y follows
│   └── includes/       # Configuración global y gestión de sesiones
├── public/             # Archivos Públicos (Frontend)
│   ├── admin/          # Panel de Gestión Administrativo
│   ├── css/            # Estilos globales y específicos
│   ├── images/         # Recursos gráficos y placeholders
│   ├── js/             # Scripts de interactividad
│   └── uploads/        # Archivos subidos por usuarios (Modelos/Imágenes)
├── db/                 # Almacenamiento de la Base de Datos SQLite
└── mkdocs.yml          # Configuración de este portal
```

## 🔄 Flujo de una Petición (Request)

Cuando un usuario interactúa con la página, el flujo sigue este patrón:

1. **Interfaz**: El usuario hace clic en una acción (ej: Comprar) en un archivo en `public/`.
2. **Controlador**: Se envía una petición (GET/POST) a un archivo en `php/`.
3. **Seguridad**: Se invoca `session.php` para verificar permisos.
4. **Base de Datos**: El archivo PHP se comunica con `config.php` para realizar operaciones SQL.
5. **Respuesta**: El servidor responde con una redirección o un JSON (AJAX).

## 🧩 Integración PHP
Todos los archivos del sistema incluyen dos pilares fundamentales al inicio:

```php
require_once __DIR__ . '/../php/includes/session.php'; // Gestión de sesiones
require_once __DIR__ . '/../php/includes/config.php';  // Conexión y Helpers
```

- **session.php**: Define si hay un usuario activo (`isLoggedIn()`) y sus roles.
- **config.php**: Centraliza la conexión PDO y funciones comunes como `sanitize()` y `redirect()`.
