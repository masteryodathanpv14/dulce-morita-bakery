# Modelo de costeo de pastelería (COP) — Punto #1

Este paquete inicial te deja listo el punto #1: importar CSVs y crear Tablas en Excel para costear tus productos. Incluye muffins sin relleno (chocolate y vainilla) como ejemplo. Además, te sugerimos la mejor forma para agregar insumos y actualizar precios.

## 1) Importar y crear Tablas en Excel
1. Crea un libro de Excel nuevo.
2. Importa cada CSV en su propia hoja y conviértelo en Tabla (Ctrl+T), con estos nombres EXACTOS:
   - tInsumos (Insumos.csv)
   - tProductos (Productos.csv)
   - tReceta (RecetaDetalle.csv)
3. Define formatos:
   - COP sin decimales para precios.
   - Porcentajes 0–2 decimales.
   - Fechas AAAA-MM-DD.

## 2) Columnas calculadas recomendadas
En tInsumos agrega a la derecha:
- Unidad_base:
  =SI(O([@Unidad_compra]="kg";[@Unidad_compra]="g");"g";SI(O([@Unidad_compra]="l";[@Unidad_compra]="ml");"ml";"u"))
- Factor_a_base:
  =SI([@Unidad_compra]="kg";1000;SI([@Unidad_compra]="l";1000;1))
- Cant_base_compra:
  =[@Cantidad_compra]*[@Factor_a_base]
- Costo_unit_base:
  =[@Precio_compra_COP]/[@Cant_base_compra]
- Costo_utilizable_base:
  =[@Costo_unit_base]/(1-[@Merma_%]/100)

En tReceta agrega:
- Unidad_base_insumo:
  =BUSCARX([@Codigo_Insumo];tInsumos[Codigo];tInsumos[Unidad_base])
- Costo_unit_insumo_base:
  =BUSCARX([@Codigo_Insumo];tInsumos[Codigo];tInsumos[Costo_utilizable_base])
- Factor_a_base:
  =SI(Y([@Unidad_receta]="kg";[@Unidad_base_insumo]="g");1000;SI(Y([@Unidad_receta]="l";[@Unidad_base_insumo]="ml");1000;1))
- Cant_base:
  =[@Cantidad_receta]*[@Factor_a_base]
- Costo_parcial:
  =[@Cant_base]*[@Costo_unit_insumo_base]

Crea una hoja "Costeo" con por lo menos:
- Costo_materiales_lote:
  =SUMAR.SI.CONJUNTO(tReceta[Costo_parcial];tReceta[Producto];[@Producto])
- Rendimiento_unid:
  =BUSCARX([@Producto];tProductos[Producto];tProductos[Rendimiento_lote_unid])
- MO_lote:
  =BUSCARX([@Producto];tProductos[Producto];tProductos[Tiempo_lote_min])/60*BUSCARX([@Producto];tProductos[Producto];tProductos[Mano_obra_hora_COP])
- Ind_lote:
  =BUSCARX([@Producto];tProductos[Producto];tProductos[Tiempo_lote_min])/60*BUSCARX([@Producto];tProductos[Producto];tProductos[Overhead_hora_COP])
- Empaque_unit_COP:
  =BUSCARX([@Producto];tProductos[Producto];tProductos[Empaque_unit_COP])
- Costo_total_unit:
  =([@Costo_materiales_lote]+[@MO_lote]+[@Ind_lote]) / [@Rendimiento_unid] + [@Empaque_unit_COP]
- Margen_objetivo:
  =BUSCARX([@Producto];tProductos[Producto];tProductos[Margen_objetivo])
- IVA_venta:
  =BUSCARX([@Producto];tProductos[Producto];tProductos[IVA_venta])
- Precio_sin_IVA:
  =[@Costo_total_unit]/(1-[@Margen_objetivo])
- Precio_con_IVA:
  =[@Precio_sin_IVA]*(1+[@IVA_venta])
- Precio_redondeado_100:
  =REDONDEAR.MAS([@Precio_con_IVA];-2)

## 3) La mejor forma de agregar insumos y modificar costos
Hay dos modos. Empieza con el simple y, cuando necesites historial, activa el modo con compras.

- Modo simple (rápido):
  - Agrega filas en tInsumos con nuevos insumos o actualiza Precio_compra_COP y Fecha_actualizacion.
  - Usa Validación de datos en Unidad_compra para restringir a: kg, g, l, ml, u.
  - Ventaja: directo y fácil. Contras: no hay historial.

- Modo con histórico de compras (recomendado al crecer):
  - Crea una hoja/tabla tCompras con las columnas: Codigo, Fecha, Cantidad_compra, Unidad_compra, Precio_compra_COP, Proveedor.
  - Registra cada compra. Mantén tInsumos como catálogo (1 fila por insumo) y vincula el precio vigente desde tCompras.
  - En tInsumos agrega una columna Precio_vigente_COP con fórmula (Excel 365):
    =BUSCARX((MAX.SI.CONJUNTO(tCompras[Fecha];tCompras[Codigo];[@Codigo]));FILTRO(tCompras[Fecha];tCompras[Codigo]=[@Codigo]);FILTRO(tCompras[Precio_compra_COP];tCompras[Codigo]=[@Codigo]))
  - Alternativa sin FILTRO: usa una tabla dinámica o Power Query para obtener "último precio por Código" y relacionarlo a tInsumos.

Sugerencias extra:
- Bloquea encabezados y aplica formato de tabla para que al agregar filas todo se arrastre (formato y fórmulas).
- Mantén un Diccionario de unidades permitido en una hoja (kg, g, l, ml, u) y úsalo para la validación.
- Capacillos están incluidos en la receta como insumo (EMB-003). Empaque_unit_COP en tProductos queda en 0 por ahora; si vendes en caja/bolsa por unidad, pon el valor aquí para sumar al costo unitario.

## 4) Productos y recetas cargados (ejemplo)
- Muffin de chocolate (12 unidades por lote)
- Muffin de vainilla (12 unidades por lote)

Ajusta cantidades o tiempos según tu proceso real; el modelo recalcula automáticamente costos y precios.

## 5) Notas
- Las fórmulas usan BUSCARX (Excel 365/2021). En versiones anteriores, reemplaza por BUSCARV o INDICE/COINCIDIR.
- Merma_% se registra 0–100. Si prefieres 0–1, ajusta la fórmula de Costo_utilizable_base.