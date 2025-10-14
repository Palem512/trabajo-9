# Documento Funcional — InventarioLab
**Laboratorio de Programación — 6° G**

**Título del proyecto:** InventarioLab — Sistema de Inventario (Frontend)

**Descripción (resumen):**
InventarioLab es una interfaz web para la gestión básica de productos de un comercio/almacén, pensada como entrega práctica de frontend. Permite autenticación, alta/edición/listado de productos con filtros y un formulario de contacto. La entrega es estática (GitHub Pages) con formularios HTML5 y validaciones nativas; la lógica de backend se simula o queda pendiente.

---

## 1. Contexto y alcance
**Objetivos**
- Practicar maquetado semántico (HTML5) y estilos (CSS).
- Implementar formularios accesibles con validación HTML5 (required, type, pattern, min/max, step).
- Diseñar flujos de usuario, mensajes y prototipos.

**Público y roles**
- **Admin:** completo (alta, edición, desactivación, gestión de categorías/proveedores).
- **Vendedor:** listado, filtros, ajuste de stock; no puede eliminar categorías.
- **Visitante/Soporte:** puede enviar mensajes por Contacto; visualización limitada.

**Secciones y funcionalidades principales**
- `/login.html` — autenticación.
- `/productos-alta.html` — formulario de alta de producto.
- `/productos-editar.html` — edición (ID interno readonly).
- `/productos-listado.html` — tabla con filtros y acciones.
- `/contacto.html` — soporte/sugerencias.
- Opcionales (simulados): baja/desactivación, ajuste de stock, gestión de categorías, exportar listado.

---

## 2. Casos de uso por formulario

### Login
- **Propósito:** permitir acceso según rol.
- **Reglas:** `email` con `type="email"` y `required`; `password` `required` y `minlength=6`.
- **Mensajes:** "Email inválido", "Contraseña requerida", "Credenciales incorrectas" (simulado).

### Alta de producto
- **Propósito:** crear producto nuevo con datos completos.
- **Reglas:** SKU alfanumérico único (`pattern`), nombre obligatorio, categoría y proveedor obligatorios, precio ≥ 0 (step 0.01), stock ≥ 0.
- **Mensajes:** "Complete el campo SKU", "Precio inválido", "Stock no puede ser negativo", confirmación "Producto creado".

### Edición de producto
- **Propósito:** modificar datos existentes.
- **Reglas:** ID interno mostrado en `readonly` (se envía); algunos campos pueden estar `disabled` por permisos (no se envían). Misma validación que Alta.
- **Mensajes:** "Cambios guardados", "No tiene permisos para modificar este campo".

### Listado de productos
- **Propósito:** visualizar y filtrar productos; acceso rápido a edición.
- **Reglas:** filtros por texto (nombre/SKU), por categoría y estado; exportar (simulado); paginación maquetada.
- **Mensajes:** "No se encontraron productos", "Resultados X de Y".

### Contacto
- **Propósito:** recibir consultas, sugerencias y reportes de stock.
- **Reglas:** nombre `required` (minlength 2), `email` con `type="email"` required, asunto `required`, mensaje `required` (minlength 10).
- **Mensajes:** "Mensaje enviado", validación de email o longitud cuando corresponda.

---

## 3. Campos y validaciones (por pantalla)

> **Nota sobre campos autogenerados y bloqueados**
> - `readonly`: se muestra, no editable, **se envía** en el form (ideal para ID interno que debe viajar).
> - `disabled`: se muestra inhabilitado y **no se envía** (usar cuando el dato no debe viajar por permisos o cuando queremos ocultar su envío).
> - `hidden`: campo no visible pero **se envía** (usar para valores que el usuario no debe ver pero que el servidor necesita).

### Producto (Alta / Edición)
- `id` — hidden (alta) / readonly (edición). Ej: número autogenerado.
- `sku` — text, `required`, `pattern="[A-Za-z0-9\-]+"`, `minlength=3`, `maxlength=20`.
- `nombre` — text, `required`, `minlength=2`, `maxlength=100`.
- `categoria` — select, `required`.
- `proveedor` — select, `required`.
- `stock_actual` — number, `required`, `min=0`, `step=1`.
- `stock_minimo` — number, `required`, `min=0`, `step=1`.
- `precio_unitario` — number, `required`, `min=0`, `step=0.01`.
- `estado` — radio/select (`activo`/`inactivo`), `required`.

### Login
- `email` — `type="email"`, `required`.
- `password` — `type="password"`, `required`, `minlength=6`.
- `recordarme` — checkbox (opcional).

### Contacto
- `nombre` — text, `required`, `minlength=2`.
- `email` — `type="email"`, `required`.
- `asunto` — select (Consulta técnica / Sugerencia / Error de stock), `required`.
- `mensaje` — textarea, `required`, `minlength=10`, `maxlength=2000`.

---

## 4. Flujos de usuario y manejo de errores

### Alta de producto (flujo ideal)
1. Usuario completa el formulario y pulsa **Guardar**.
2. Navegador ejecuta validación nativa; si hay errores, campos marcarán mensajes nativos.
3. Si pasa validación, mostrar banner con `role="status"` o `role="alert"` según sea éxito/error: "Producto creado" o error simulado (p.ej. SKU duplicado).
4. Navegación: redirigir a `/productos-listado.html` o ancla `#resultado`.

### Edición
- Mostrar `id` en `readonly`. Si usuario intenta editar campo `disabled`, se mostrará texto explicativo (tooltip/leyenda). Botones: **Guardar cambios** y **Reset** (restablecer valores del formulario).

### Listado / Filtros
- Filtros en un `form` con `method="get"` o `post` y `action="#"` (dummy). Los resultados se muestran bajo la sección de filtros; si no hay resultados, mostrar CTA a alta de producto.

### Contacto
- Enviar muestra mensaje de éxito y limpia formulario (simulado). Errores de validación impiden el envío y marcan campos.

---

## 5. Prototipo / Wireframes (baja-media fidelidad)

**Login**
- Contenedor centrado, fondo con gradiente, caja con logo, H1 "Iniciar sesión", campos Email y Password, checkbox "Recordarme", botón "Entrar".

**Alta de producto**
- Header con logo y nav.
- Form dividido en dos columnas: datos identificatorios (SKU, Nombre, Categoría, Proveedor) y datos numéricos (Precio, Stock actual, Stock mínimo, Estado).
- Botones: Guardar, Reset.

**Edición**
- Igual que Alta; en la parte superior `ID (readonly)`. Si algún campo está bloqueado por rol aparece `disabled` y nota explicativa.

**Listado**
- Zona superior: filtros (texto, categoría, estado) y botones (Buscar, Exportar).
- Zona inferior: tabla con columnas SKU, Nombre, Categoría, Stock, Stock mín., Precio, Estado, Acciones (Editar).
- Paginación maquetada y select "Ítems por página".

**Contacto**
- Form sencillo con Nombre, Email, Asunto (select) y Mensaje (textarea), botón Enviar.

---

## Entregables A (requeridos)
- Documento Funcional en MD/PDF (este archivo).  
- Capturas o enlace al prototipo (Figma/Whimsical/pen-paper) referenciado en README del repo.

---

**Observaciones técnicas y accesibilidad**
- Usar etiquetas semánticas (`header`, `nav`, `main`, `footer`), `label[for]` y `fieldset/legend`.
- Mensajes con `role="alert"` y estados con `aria-invalid` cuando aplique.
- Contraste de colores accesible y focus visible en inputs.
- Formularios usarán `method` y `action="#"` hasta integrar backend.

---
