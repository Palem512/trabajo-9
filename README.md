# InventarioLab — Maquetado estático

Proyecto entregable: interfaz estática con formularios accesibles y validados.

## Cómo usar
1. Copiar archivos a un repo y publicar con GitHub Pages.
2. Estructura: `index.html`, `login.html`, `productos-*.html`, `contacto.html`, `css/`, `img/`.

## Principios de usabilidad aplicados
- Consistencia: navegación y estilos coherentes entre pantallas.
- Visibilidad del estado: mensajes y campos `readonly`/`disabled` explicados.
- Prevención de errores: validación nativa (required, pattern, min/max).

## Prueba del maletero
Colocar la vista "Alta de producto" en el menú principal y texto claro en la landing para que un usuario sin instrucción encuentre rápidamente dónde crear un producto.

## Notas
- Formularios usan `action="#"` (dummy). Para backend, ajustar `action` y método.
- `readonly` -> el campo se envía; `disabled` -> el campo NO se envía. Documentado en Documento Funcional.
