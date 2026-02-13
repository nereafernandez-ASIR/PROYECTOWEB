# Introducción a LayerHub

LayerHub es una plataforma integral diseñada para la gestión de recursos en el ámbito de la impresión 3D. El sistema integra un entorno de comercio electrónico para la adquisición de hardware y materiales con una sección comunitaria para el intercambio de modelos STL y contenido educativo.

## Objetivos del Proyecto

El desarrollo ha sido orientado hacia la creación de una infraestructura robusta y escalable que cumpla con los siguientes estándares:

*   **Arquitectura Modular**: Separación clara entre la lógica del backend (PHP) y la interfaz estética (CSS/JS).
*   **Gestión de Datos Centralizada**: Uso de SQLite para una persistencia eficiente y de baja latencia.
*   **Seguridad Multi-capa**: Implementación de controles de acceso basados en roles y validaciones estrictas de servidor.
*   **Interactividad Social**: Sistemas de seguimiento, valoraciones y feedback dinámico.

## Stack Tecnológico

El portal ha sido desarrollado utilizando las siguientes tecnologías de grado industrial:

| Capa | Tecnología | Descripción |
| :--- | :--- | :--- |
| **Backend** | PHP 8.1+ | Lógica de servidor y controladores de datos. |
| **Base de Datos** | SQLite 3 | Base de datos relacional integrada. |
| **Frontend** | Vanilla JS / CSS3 | Interfaz de usuario sin dependencias pesadas. |
| **Seguridad** | BCRYPT / PDO | Encriptación de credenciales y prevención de SQLi. |

## Estructura de la Documentación

Este portal técnico se divide en las siguientes secciones para facilitar su consulta:

1.  **Arquitectura del Sistema**: Descripción de la organización de archivos y flujo de datos.
2.  **Modelo de Datos**: Esquema relacional, tablas y lógica de base de datos.
3.  **Lógica Especializada**: Análisis de código de funcionalidades críticas (Carrito, Subidas).
4.  **Seguridad y Roles**: Protocolos de autenticación y niveles de acceso.
5.  **Instalación**: Procedimientos de despliegue y mantenimiento.
