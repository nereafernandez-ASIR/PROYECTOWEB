# Guía de Instalación y Despliegue

Sigue estos pasos para poner en marcha el portal de documentación y el proyecto LayerHub.

## 📥 Requisitos Previos
- **PHP 8.0** o superior.
- **Servidor Web** (Apache/Nginx) - Se recomienda **XAMPP**.
- **Python 3.x** (para ejecutar MkDocs).

## 📓 Instalación de la Documentación
Para visualizar este portal de forma interactiva y profesional:

1. **Instalar MkDocs y el tema Material**:
   ```bash
   pip install mkdocs-material
   ```

2. **Ejecutar el servidor local**:
   Desde la raíz del proyecto (`c:\xampp\htdocs`), ejecuta:
   ```bash
   mkdocs serve
   ```

3. **Ver en el navegador**:
   Abre [http://127.0.0.1:8000](http://127.0.0.1:8000).

## 🌐 Publicar en Internet (GitHub Pages)
MkDocs tiene una función integrada para crear un enlace web gratuito:

1. **Subir a GitHub**: Asegúrate de que tu proyecto está en un repositorio de GitHub.
2. **Ejecutar despliegue**:
   ```bash
   mkdocs gh-deploy
   ```
   Este comando compilará tu documentación y la subirá a una rama llamada `gh-pages`.

3. **Configuración en GitHub**:
   - Ve a tu repositorio en GitHub.com.
   - Entra en **Settings** > **Pages**.
   - Asegúrate de que la fuente es la rama `gh-pages`.
   - ¡GitHub te dará una URL (ej: `https://tu-usuario.github.io/layerhub`) para que cualquiera pueda entrar!

## 🏗️ Compilación Estática
Si quieres generar una versión HTML de esta documentación para subirla a un servidor web:
```bash
mkdocs build
```
Esto creará una carpeta `site/` con todo el contenido listo para ser servido.
