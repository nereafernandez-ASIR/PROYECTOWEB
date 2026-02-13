# Análisis Funcional: Módulos de Cliente

Este documento detalla la implementación técnica de las funcionalidades accesibles para el usuario final (rol `user`).

## 1. Carrito de Compras (`public/carrito.php`)

El carrito no utiliza cookies para almacenar productos, sino que persiste los datos en la base de datos (`table: cart`) para permitir el acceso multidispositivo.

### Lógica de Recuperación de Items
El siguiente bloque SQL realiza un `JOIN` entre la tabla `cart` y `products` para obtener toda la información necesaria en una sola consulta eficiente:

```php
// config.php / carrito.php
$stmt = $db->prepare("
    SELECT c.*, p.title, p.price, p.image, p.stock 
    FROM cart c 
    JOIN products p ON c.product_id = p.id 
    WHERE c.user_id = ?
    ORDER BY c.added_at DESC
");
$stmt->execute([$user['id']]);
```

**Explicación Técnica:**
1.  Se prepara la consulta para prevenir Inyección SQL.
2.  Se vinculan los datos del usuario actual (`$user['id']`).
3.  Se recuperan dinámicamente el precio y stock actual (evitando guardar precios desactualizados en el carrito).

## 2. Gestión de Archivos STL (`php/community/upload_stl.php`)

Este controlador maneja la subida de archivos binarios grandes. Implementa una estrategia de validación de seguridad en profundidad.

### Código de Validación de Seguridad
El sistema rechaza cualquier archivo que no cumpla estrictamente con los criterios de seguridad MIME y extensión.

```php
// Verificación de Tipo MIME Real
$finfo = finfo_open(FILEINFO_MIME_TYPE);
$mime = finfo_file($finfo, $file['tmp_name']);
finfo_close($finfo);

// Lista blanca de tipos permitidos
$allowedMime = [
    'application/sla', 
    'application/octet-stream', 
    'application/zip', 
    'text/plain'
];

if (!in_array($mime, $allowedMime)) {
    // Manejo de error y redirección
    setFlash('Archivo no válido', 'error');
    redirect('/public/comunidad.php');
}
```

**Relación con Base de Datos:**
Una vez validado el archivo físico, se registra una entrada en la tabla `stl_files` con el estado `pending`. Esto asegura que el archivo existe en el servidor pero **no es visible** públicamente hasta la aprobación administrativa.

## 3. Sistema de Tutoriales (`php/community/add_tutorial.php`)

Este módulo demuestra la versatilidad del sistema al manejar dos tipos de fuentes de datos para una misma entidad.

### Lógica de Selección de Fuente
El controlador decide automáticamente si procesar un archivo subido o incrustar una URL externa.

```php
if (isset($_FILES['video_file']) && validFile($_FILES['video_file'])) {
    // Caso A: Almacenamiento Local
    $fileName = uniqid('video_') . '.mp4';
    move_uploaded_file($tmpName, $destPath);
    $finalSource = $destPath;
} else if (!empty($videoUrl)) {
    // Caso B: Referencia Externa (YouTube)
    $finalSource = $videoUrl;
}
```

Los datos se almacenan en la tabla `tutorials`, normalizando ambos casos en el campo `video_url`.
