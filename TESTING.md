# Plan de pruebas — ListaCompras

Aplicación: https://zoxpruebasia-coder.github.io/listacompras/
Versión de referencia: v10 (visible en Ajustes).

## Preparación

- **URL para las pruebas:** `https://zoxpruebasia-coder.github.io/listacompras/?reset=1`
  El parámetro `reset=1` borra los datos guardados al cargar y deja la app en su estado inicial (127 productos, sin precios). Así cada ejecución parte de lo mismo.
- **Viewport recomendado:** móvil, 390 × 844. La app está diseñada para vertical.
- **Sin autenticación:** no hay login ni cuentas.
- **Datos:** todo vive en `localStorage` del navegador. Una ejecución nueva equivale a un usuario nuevo.

## Casos

### 1. Carga inicial
1. Abrir la URL con `?reset=1`.
2. Esperar a que desaparezca el aviso de la lista.

**Esperado:** total `0,00 €`; contador `127 productos · 127 sin precio`; chips Todo 127, Mercadona 0, Condis 0, Lidl 0, Consum 0; bloque «Sin tienda» con 127 líneas.

### 2. Buscador de la lista
1. Escribir `le` en «Buscar en la lista…».

**Esperado:** 6 resultados (Lechuga, Leche para niños, Leche condensada, Levadura, Galletas de soda, Lejía); contador `6 de 127 productos`; botón × visible.
2. Pulsar ×.

**Esperado:** vuelve a 127 productos.

### 3. Alta rápida de un producto
1. Pulsar «+ Añadir».
2. Escribir `pan` y elegir «Pan».
3. Tienda `Mercadona`, precio `1,35`, cantidad `2`, pulsar «Añadir».

**Esperado:** aviso «Pan añadido»; la lista se desplaza hasta la línea y la resalta; bloque Mercadona con `Pan — 2 × 1,35 € — 2,70 €`; total `2,70 €`.

### 4. Añadir precio de otra tienda (más barato)
1. Tocar «Pan» en la lista.
2. En «Tienda sin precio» comprobar que **no** aparece Mercadona.
3. Elegir `Lidl`, precio `1,20`, «Añadir precio».

**Esperado:** «Precios guardados» con Lidl (MEJOR, 1,20 €) y Mercadona (1,35 €); la línea se mueve al bloque Lidl; total `2,40 €`; aviso «Más barato en Lidl».

### 5. Un precio más caro no mueve la línea
1. Abrir «Pan» y añadir `Condis` a `1,50`.

**Esperado:** Condis aparece en la lista de precios; la línea sigue en Lidl a 1,20 €; total `2,40 €`; no se mueve.

### 6. Editar un precio guardado
1. En «Precios guardados», pulsar el lápiz de Lidl.
2. Cambiar a `1,10` y «Guardar cambios».

**Esperado:** el formulario pasa a «Editar precio» con la tienda fija y botón Cancelar; tras guardar, Lidl muestra 1,10 € y la línea y el total pasan a `2,20 €`.

### 7. Cantidad con texto
1. Pulsar el lápiz de la línea de Pan.
2. En «En esta compra», pulsar «Editar».
3. Escribir `2 bolsas` en cantidad y guardar.

**Esperado:** la línea muestra `2 bolsas × 1,10 €` y el total `2,20 €`.

### 8. Filtros por tienda
1. Pulsar el chip `Lidl`.

**Esperado:** solo se ve el bloque Lidl.
2. Pulsar el chip `Condis` (sin productos).

**Esperado:** mensaje «Nada en esta tienda».

### 9. Gestión de tiendas
1. Ajustes (⋯) → sección Tiendas → escribir `Eroski` → Añadir.

**Esperado:** aparece la fila Eroski y el chip Eroski en la cabecera.
2. Cambiar el nombre a `Eroski Center` y salir del campo.

**Esperado:** el chip y los selectores muestran el nombre nuevo.
3. Pulsar × en Eroski Center y confirmar.

**Esperado:** desaparece de la lista y de los chips.

### 10. Cerrar compra
1. Pulsar «Cerrar compra» (desde la lista, al final) y confirmar.

**Esperado:** aviso «Compra cerrada · 2,20 €»; la lista se renueva con los 127 productos y sus precios; «Historial» muestra la compra con fecha, tiendas y 2,20 €.

### 11. Persistencia
1. Recargar la página (sin reset).

**Esperado:** siguen el historial y los precios guardados del producto Pan.

### 12. Exportar e importar
1. Ajustes → Exportar datos (JSON).
2. Ajustes → Borrar todos los datos → confirmar.
3. Ajustes → Importar datos → elegir el JSON exportado.

**Esperado:** tras borrar, 127 productos sin precios y sin historial; tras importar, vuelven los precios y la compra.

### 13. Sin conexión
1. Cargar la app con conexión.
2. Activar modo avión y recargar.

**Esperado:** la app abre desde la caché y permite añadir productos.

## Limitaciones conocidas al automatizar

- **`localStorage` por contexto:** cada ejecución nueva empieza limpia. Los casos que dependen de pasos previos (4, 5, 6, 10) deben ejecutarse en la misma sesión, en orden, o repetir los pasos previos.
- **Diálogos nativos:** las confirmaciones usan `confirm()` del navegador. El automatizador debe aceptarlas.
- **Service worker:** la página se sirve primero desde la red; si se prueba una versión recién publicada, esperar uno o dos minutos a que GitHub Pages compile.
- **Sin cámara, sin voz, sin login:** no hay permisos que conceder.
- **Datos de prueba:** cualquier precio o cantidad vale; los importes esperados de este documento corresponden a los valores indicados en cada caso.
