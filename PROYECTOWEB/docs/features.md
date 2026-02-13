# Análisis de Código y Funcionalidades Críticas

En esta sección se detalla la implementación técnica de los procesos más complejos del backend de LayerHub.

## Sistema de Gestión de Carrito

La lógica del carrito se sincroniza directamente con la base de datos para garantizar la disponibilidad de los productos y la persistencia de la sesión.

### Recuperación de Productos
A continuación se muestra el código utilizado para obtener los elementos del carrito vinculando la información de stock y precio:

```php
$stmt = $db->prepare("
    SELECT c.*, p.title, p.price, p.image, p.stock 
    FROM cart c 
    JOIN products p ON c.product_id = p.id 
    WHERE c.user_id = ?
    ORDER BY c.added_at DESC
");
$stmt->execute([$user['id']]);
$cartItems = $stmt->fetchAll();
```

## Procesamiento de Subida de Archivos STL

La subida de archivos 3D requiere validaciones estrictas de tamaño, extensión y tipo MIME para prevenir la ejecución de código malicioso.

### Lógica de Validación de Seguridad
El controlador realiza una comprobación triple (Extensión, Tamaño y MIME real vía `finfo`):

```php
$finfo = finfo_open(FILEINFO_MIME_TYPE);
$mime = finfo_file($finfo, $file['tmp_name']);
finfo_close($finfo);

if (!in_array($mime, $allowedMime)) {
    if ($mime !== 'application/octet-stream' && $mime !== 'text/plain') {
        setFlash('Contenido no válido', 'error');
        redirect('/public/comunidad.php');
    }
}
```

## Gestión de Contenido Multimedia (Tutoriales)

El sistema de tutoriales admite un flujo híbrido: carga directa de video (.mp4) o vinculación de enlaces externos (YouTube).

### Control de Flujo de Video
El código prioriza la subida física del archivo si este existe y cumple los requisitos técnicos:

```php
if (isset($_FILES['video_file']) && $_FILES['video_file']['error'] === UPLOAD_ERR_OK) {
    // Proceso de validación de MP4 y almacenamiento físico
    $fileName = uniqid('video_') . '.mp4';
    $uploadPath = __DIR__ . '/../../uploads/users/videos/' . $fileName;
    move_uploaded_file($file['tmp_name'], $uploadPath);
} else if (!empty($videoUrl)) {
    // Almacenamiento de referencia externa a YouTube
    $finalVideoSource = $videoUrl;
}
```

## Moderación de Contenido

Toda contribución comunitaria queda marcada con el estado `status = 'pending'`. Este mecanismo asegura que ningún contenido sea público sin la revisión manual del administrador, cuya lógica se encuentra centralizada en los controladores de administración de la plataforma.
