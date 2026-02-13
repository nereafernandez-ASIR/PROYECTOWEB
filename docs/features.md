# Funcionalidades del Sistema

LayerHub se divide en tres grandes áreas funcionales interconectadas.

## 🛍️ Tienda (E-Commerce)
La tienda permite la adquisición de hardware y materiales de impresión 3D.

- **Catálogo Dinámico**: Filtrado por categorías y búsqueda en tiempo real.
- **Gestión de Stock**: El sistema verifica la disponibilidad antes de permitir añadir al carrito.
- **Carrito de Compras**: Persistente en base de datos para el usuario identificado.
- **Workflow de Pedido**:
    1. Selección de producto.
    2. Validación de stock.
    3. Simulación de pago seguro.
    4. Generación de pedido y descuento de inventario.

## 👥 Comunidad Maker
Inspirada en plataformas como Cults3D, permite la interacción social.

- **Modelos STL**: Los usuarios suben archivos que pasan por un proceso de moderación.
- **Tutoriales**: Integración con YouTube para compartir guías técnicas.
- **Sistema de Valoración**: Ratings de 1 a 5 estrellas con comentarios para cada modelo.
- **Social**: Funcionalidad de seguir/dejar de seguir a otros creadores.

## 🛠️ Panel de Administración
El centro de control exclusivo para usuarios con rol `admin`.

### Gestión
- **Usuarios**: Posibilidad de bloquear, eliminar o cambiar roles.
- **Productos**: CRUD completo (Crear, Leer, Actualizar, Borrar) de artículos de la tienda.
- **Pedidos**: Seguimiento y cambio de estado de las ventas.

### Moderación
- **Aprobación de Contenido**: El administrador debe revisar y aprobar cada STL o tutorial antes de que aparezca en la parte pública.
- **Limpieza**: Capacidad de eliminar valoraciones ofensivas o contenido inapropiado.
