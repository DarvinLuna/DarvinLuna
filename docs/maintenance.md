# Mantenimiento del perfil

Este repositorio contiene el README del perfil de GitHub y sus recursos visuales. No contiene una aplicación web, API ni proyecto de React Native.

## Diseño

- `README.md`: presentación, tecnologías y estadísticas.
- `assets/profile-banner.svg`: cabecera editable, sin servicios externos.
- `profile-summary-card-output/`: tarjetas generadas de GitHub. El README usa `github` y `github_dark` según el tema del lector.
- Los bloques de imágenes se ajustan al ancho disponible y las tarjetas se distribuyen en varias líneas en pantallas pequeñas.

Las imágenes anteriores de `gitlab-stats` apuntaban a un repositorio privado y devolvían 404 sin autenticación. Se sustituyeron por las tarjetas públicas de este repositorio. Estas tarjetas muestran actividad de GitHub; no incluyen estadísticas de GitLab. Para volver a mostrar un resumen combinado, primero debe publicarse una versión de esos SVG que pueda consultar un visitante sin iniciar sesión.

## Automatizaciones

| Workflow | Destino | Horario programado |
| --- | --- | --- |
| GitHub-Profile-Summary-Cards | `main`, en `profile-summary-card-output/` | 06:17 UTC / 00:17 Nicaragua |
| GitHub Snake Game | `output`, en la raíz de esa rama | 06:37 UTC / 00:37 Nicaragua |

GitHub puede retrasar las ejecuciones programadas. Las tarjetas usan `UTC_OFFSET: -6` para el gráfico horario. Se conservan todos los temas existentes.

Los jobs que publican necesitan `contents: write`. Las ejecuciones simultáneas del mismo workflow se ponen en cola. Los pushes están limitados a `main` y a los archivos de configuración relevantes, para que la actualización de SVG no vuelva a disparar los generadores.

## Primera publicación

1. Subir los cambios revisados a `main`.
2. En **Actions → GitHub Snake Game → Run workflow**, seleccionar `main` y ejecutar. El push del workflow también dispara esta generación.
3. Confirmar que la rama `output` contiene `github-snake.svg` y `github-snake-dark.svg`. La animación del README estará disponible después de esta ejecución.
4. Ejecutar **GitHub-Profile-Summary-Cards** en `main` para actualizar las tarjetas y el huso horario.

La animación no requiere activar GitHub Pages ni crear un token personal. Ambos workflows utilizan el `GITHUB_TOKEN` proporcionado por GitHub.

## Problemas frecuentes

- **403 al publicar:** comprobar que las políticas del repositorio permiten `contents: write` y que las reglas de la rama destino permiten las escrituras del bot.
- **Animación con 404:** revisar la última ejecución del workflow y los nombres de archivos en `output`. La rama no existirá hasta la primera ejecución correcta.
- **Tarjetas sin actualizar:** revisar el workflow de estadísticas; GitHub puede almacenar las imágenes en caché.
- **Ejecución manual omitida:** seleccionar `main`; los jobs solo publican desde esa rama.

Las versiones de checkout y publicación están fijadas a releases específicas. La acción de tarjetas conserva su referencia original `@release`; revisar cambios del proveedor antes de actualizarla.
