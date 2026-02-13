# Análisis Funcional: Módulos de Administración

Este documento describe las funcionalidades exclusivas del panel de administración, protegido por el middleware de seguridad `requireAdmin()`.

## 1. CRUD de Productos (`public/admin/productos.php`)

El núcleo de la gestión de la tienda permite crear, leer, actualizar y eliminar productos.

### Protección de Acceso
Antes de ejecutar cualquier lógica, el sistema verifica el rol:

```php
require_once __DIR__ . '/../../php/includes/session.php';
requireAdmin(); // Detiene la ejecución si el rol != 'admin'
```

### Lógica de Inserción/Actualización
El sistema utiliza sentencias preparadas para modificar el catálogo.

```php
if ($action === 'create') {
    $stmt = $db->prepare("INSERT INTO products (title, price, stock, category) VALUES (?, ?, ?, ?)");
    $stmt->execute([$title, $price, $stock, $category]);
}
```

## 2. Sistema de Moderación (`public/admin/moderar_stl.php`)

Esta interfaz actúa como un "semáforo" para el contenido generado por el usuario (UGC). Permite cambiar el estado de `pending` a `approved` o `rejected`.

### Flujo de Aprobación
El administrador visualiza una lista de pendientes y ejecuta una acción que actualiza la base de datos:

```php
// Al aprobar un archivo
$stmt = $db->prepare("UPDATE stl_files SET status = 'approved' WHERE id = ?");
$stmt->execute([$id]);

// Notificación (Opcional)
// El sistema podría disparar una notificación al usuario informando la aprobación.
```

**Impacto en el Sistema:**
Solo los registros con `status = 'approved'` son recuperados por las consultas SQL de la sección pública (`comunidad.php`), garantizando un entorno seguro y curado.

## 3. Gestión de Usuarios y Bloqueos (`public/admin/usuarios.php`)

Permite la administración de la base de usuarios, incluyendo la capacidad de revocar el acceso.

### Lógica de Bloqueo
El bloqueo es un "soft-delete" funcional. No borra al usuario, sino que establece un flag.

```php
// Bloquear usuario
$stmt = $db->prepare("UPDATE users SET blocked = 1 WHERE id = ?");
$stmt->execute([$userId]);
```

Este flag es verificado en cada inicio de sesión (`login.php`) y en cada carga de página (`session.php`), expulsando al usuario inmediatamente si está activo.
