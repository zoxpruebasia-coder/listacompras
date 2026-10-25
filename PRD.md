# PRD — ListaCompras

## Resumen

Aplicación web (PWA) para preparar la compra del supermercado antes de pasar por caja: lista de productos agrupada por tienda, precios por tienda con histórico, total en curso e historial de compras. Uso personal y familiar, sin cuentas ni servidor.

- **URL publicada:** https://zoxpruebasia-coder.github.io/listacompras/
- **URL para pruebas (estado limpio):** https://zoxpruebasia-coder.github.io/listacompras/?reset=1
- **Plataforma:** navegador móvil (Chrome Android), instalable como PWA.

## Usuarios

Personas que hacen la compra semanal en varias cadenas (Mercadona, Condis, Lidl, Consum) y quieren saber cuánto van a gastar y dónde está más barato cada producto.

## Alcance funcional

### 1. Lista de la compra
- Una sola lista, agrupada por tienda, con subtotal por bloque y total global.
- Bloque «Sin tienda» para productos sin asignar.
- Cada línea muestra nombre, cantidad, precio y total de la línea.
- Añadir productos desde el buscador del catálogo (127 productos iniciales).
- Al añadir, la lista se desplaza hasta el producto nuevo y lo resalta.
- Buscador de la lista por nombre, con contador de resultados.

### 2. Precios por tienda
- Un producto puede tener varios precios, uno por tienda, con fecha.
- El formulario de precio solo ofrece tiendas sin precio para ese producto.
- Si el precio nuevo es más barato que el de la línea, la línea se mueve a esa tienda y actualiza el total.
- Si es más caro, no se mueve.
- Cada precio se puede editar o borrar.
- Precio unitario calculado (€/kg, €/L) cuando se indica el contenido del envase.
- Etiqueta «MEJOR» en el precio más bajo.

### 3. Compra e historial
- Cerrar compra: guarda fecha, tiendas, líneas y total; traslada los precios al catálogo y deja la lista vacía mostrando el catálogo para elegir los productos de la siguiente compra.
- Historial agrupado por mes, con detalle de cada compra.

### 4. Tiendas
- Tiendas iniciales: Mercadona, Condis, Lidl, Consum.
- Añadir, renombrar y eliminar tiendas desde Ajustes.
- Al eliminar una tienda se borran sus precios y sus productos quedan sin tienda.

### 5. Cantidad
- Acepta texto libre («2 bolsas», «1,5 kg», «un poco»).
- Si contiene un número, se usa para calcular el total; si no, cuenta como 1.

### 6. Datos
- Todo se guarda en `localStorage` del navegador.
- Exportar e importar todo el estado en JSON.
- Funciona sin conexión tras la primera carga.

## Fuera de alcance

- Cuentas de usuario, sincronización entre dispositivos y backend.
- Lectura de etiquetas, códigos de barras o tickets por cámara.
- Presupuesto máximo y alertas de gasto.
- Comparación automática con precios oficiales de las cadenas.

## Requisitos no funcionales

- Móvil primero; diseño vertical, botones grandes.
- Sin permisos del navegador (cámara, ubicación, notificaciones).
- Respuesta inmediata: añadir un producto no debe superar 5 segundos.
- Idioma: español (España), importes en euros.

## Criterios de aceptación principales

1. La lista muestra 127 productos iniciales sin precio y total 0,00 €.
2. Añadir un producto con tienda y precio lo coloca en su bloque y suma al total.
3. Añadir un precio más barato en otra tienda mueve la línea y actualiza el total.
4. El selector de tienda no ofrece tiendas que ya tengan precio para ese producto.
5. Cerrar la compra deja la lista vacía y crea una entrada en el historial.
6. Recargar la página conserva precios, lista e historial.
7. La app sigue funcionando sin conexión.

## Datos de prueba

No requiere credenciales. Cada ejecución con `?reset=1` parte del estado inicial. Cualquier precio positivo es válido; el plan detallado con importes concretos está en `TESTING.md`.
