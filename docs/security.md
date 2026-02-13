# Seguridad y Protocolos de Protección

La seguridad en LayerHub se aborda desde el diseño ("Security by Design"), implementando controles en cada capa de la aplicación.

## 1. Sanitización de Datos (Input Validation)

Para prevenir ataques de tipo XSS (Cross-Site Scripting), ninguna entrada de usuario se confía ciegamente. Se utiliza una función global `sanitize()`:

```php
function sanitize($data) {
    // 1. Eliminar espacios innecesarios
    // 2. Eliminar etiquetas HTML y PHP (strip_tags)
    // 3. Convertir caracteres especiales a entidades HTML
    return htmlspecialchars(strip_tags(trim($data)), ENT_QUOTES, 'UTF-8');
}
```

Esta función se aplica a todos los datos recibidos vía `$_POST` antes de ser procesados o almacenados.

## 2. Gestión de Contraseñas (BCRYPT)

LayerHub cumple con los estándares modernos de criptografía. Las contraseñas **nunca** se almacenan en texto plano.

### Registro
```php
$passwordHash = password_hash($password, PASSWORD_BCRYPT);
// Se almacena $passwordHash en la base de datos (60 caracteres)
```

### Login
```php
if (password_verify($inputPassword, $storedHash)) {
    // Contraseña correcta
    startUserSession($user);
}
```

## 3. Protección de Sesiones (Anti-Hijacking)

Para evitar que un atacante robe una sesión activa copiando la cookie `PHPSESSID`, el sistema vincula la sesión a la huella digital del navegador.

```php
// session.php
if ($_SESSION['user_agent'] !== $_SERVER['HTTP_USER_AGENT']) {
    // Si el navegador cambia repentinamente, destruimos la sesión por seguridad
    session_destroy();
    exit('Error de seguridad: Sesión inválida');
}
```

## 4. Control de Acceso (Middleware)

La función `requireAdmin()` actúa como barrera final. Ubicada al inicio de cada archivo sensible, garantiza que el código protegido ni siquiera llegue a ejecutarse si el usuario no tiene las credenciales adecuadas.
