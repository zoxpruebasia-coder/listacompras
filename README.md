# Carrito · lista de la compra

Aplicación web (PWA) para construir el ticket de la compra antes de pasar por caja: lista agrupada por tienda, precios, total e historial. Sin cuentas, sin servidor, sin conexión.

## Qué hace esta primera versión

- **Lista agrupada por tienda** — Mercadona, Condis, Lidl, Consum y un bloque «Sin tienda». Cada bloque con su recuento y subtotal.
- **127 productos ya cargados** de tu lista de Keep, sin precios, con las cantidades detectadas (manzana ×4, pan ×2, arroz ×3…).
- **Buscador**: filtra la lista por nombre mientras escribes, o busca en el catálogo al añadir.
- **Cantidad libre**: admite «2 bolsas» o «1,5 kg»; si contiene un número, se usa para el total.
- **Alta rápida**: buscar en el catálogo, elegir tienda, precio y cantidad. Si el producto no existe, se crea al vuelo.
- **Añadir desde el catálogo o la ficha** con un toque, sin pasar por el buscador.
- **Precio en otra tienda**: botón «＋ Añadir precio en otra tienda» en la línea, o «+ Añadir precio» en la ficha del producto.
- **Ficha de producto** con la matriz de precios por tienda: añadir precio en otra tienda, ver cuál es el más barato y desde cuándo.
- **Precio por kilo, litro o unidad**: si el envase trae 1,5 L o 500 g, se calcula el €/kg o €/L y la comparación es justa.
- **Cerrar compra**: sella la compra con fecha, tiendas y total; traslada los precios al catálogo y deja el catálogo a la vista para elegir los productos de la lista siguiente.
- **Historial** por meses, con el detalle de cada compra.
- **Gráfico del mes**: una barra por compra, con fecha e importe, y flechas para navegar entre meses.
- **Exportar e importar** todos los datos en un JSON.

## Lo que no hace (a propósito)

- No lee etiquetas ni códigos de barras (fase 4 descartada).
- No tiene presupuesto: no hay límite, solo total y referencia de la última compra.
- No sincroniza entre móviles. Eso es la fase 2, con Firebase.
- No aplica ofertas automáticamente; se marcan al guardar un precio.

## Publicarla en GitHub Pages

1. Entra en `github.com` y crea una cuenta si no tienes.
2. **New repository** → nombre `carrito` → **Public** → Create.
3. En el repositorio vacío: **uploading an existing file** y sube estos cuatro archivos:
   - `index.html`
   - `manifest.webmanifest`
   - `sw.js`
   - `icon.svg`
4. **Settings → Pages → Source: Deploy from a branch → Branch: main / (root) → Save**.
5. Espera uno o dos minutos. La dirección será `https://TUUSUARIO.github.io/carrito/`.
6. Ábrela en Chrome del móvil y usa el menú **⋮ → Instalar aplicación** (o «Añadir a pantalla de inicio»).

La primera carga necesita internet; a partir de ahí funciona sin conexión.

## Instalarla en Android

- Chrome → menú ⋮ → **Instalar aplicación**.
- Queda con su icono, a pantalla completa y sin barra de navegador.
- Para actualizar, se sustituye `index.html` en el repositorio; la app se refresca sola en la siguiente apertura.

## Tus datos

Todo se guarda en el almacenamiento del navegador de ese móvil. No viaja nada a ningún sitio.

- **Exportar** en Ajustes descarga un JSON con catálogo, precios e historial.
- **Importar** lo restaura en otro móvil o tras borrar datos.
- Si borras los datos del sitio en Chrome, se pierde todo: exporta de vez en cuando.

## Desarrollo

Los archivos de la app son los cuatro de la raíz. En `_dev/` quedan las pruebas:

```
node _dev/_test.js
node _dev/_check.js
```

`_test.js` ejecuta la lógica sin navegador (semilla, líneas, precios, cierre, persistencia). `_check.js` comprueba que todos los identificadores que usa el script existen en el HTML.

## Siguiente fase

Sincronización familiar con Firebase (login de Google, grupo por código, lista compartida en tiempo real y caché offline). El modelo de datos ya está preparado: identificadores estables, precios con fecha e historial de compras.
