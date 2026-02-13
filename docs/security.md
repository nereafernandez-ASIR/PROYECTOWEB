# Seguridad y Control de Roles

La seguridad es un pilar central en LayerHub. Se han implementado múltiples capas de protección para asegurar la integridad de los datos y la privacidad de los usuarios.

## 🛡️ Mecanismos de Protección

### 1. Autenticación de Sesión
El sistema utiliza sesiones nativas de PHP (`session_start()`) con configuraciones de seguridad adicionales en `php/includes/session.php`:
- **Regeneración de ID**: Se regenera el ID de sesión en cada login para prevenir ataques de *Session Fixation*.
- **Validación de User-Agent**: Se comprueba que el navegador no cambie durante la sesión para evitar el *Hijacking*.
- **Timeout Automático**: Las sesiones expiran tras 30 minutos de inactividad.

### 2. Control de Acceso basado en Roles (RBAC)
Existen dos niveles principales de acceso:

| Rol | Alcance | Verificación |
| :--- | :--- | :--- |
| **Público** | Solo login e index. | N/A |
| **Cliente** | Tienda, Comunidad y Perfil. | `requireLogin()` |
| **Admin** | Gestión total y Moderación. | `requireAdmin()` |

### 3. Blindaje de la URL
Para evitar que un usuario "adivine" una ruta y acceda sin permiso, cada controlador sensible incluye una guardia al inicio:

```php
// En cualquier página de administración
requireAdmin(); // Si no es admin, redirige al login o lanza 403.

// En la tienda o perfil
requireLogin(); // Si no hay sesión, envía al login.
```

## 🔒 Protección de Datos

- **Inyección SQL**: El uso sistemático de `PDO` y parámetros enlazados (`bindValue`/`execute`) neutraliza ataques de inyección.
- **XSS (Cross-Site Scripting)**: Todas las salidas de datos en el HTML pasan por la función `sanitize()` o `htmlspecialchars()`.
- **Contraseñas**: Nunca se guardan en texto plano. Se utiliza `password_hash()` con el algoritmo BCRYPT y se verifican con `password_verify()`.

## 🚫 Usuarios Bloqueados
El sistema consulta en cada carga de página crítica si el usuario ha sido marcado como `blocked` en la base de datos. Si lo está, la sesión se destruye instantáneamente y se le redirige al inicio.
