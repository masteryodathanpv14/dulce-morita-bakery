# Instrucciones de Compras y Precio Vigente

Este documento complementa el README y explica cómo usar la tabla tCompras para mantener el precio vigente de cada insumo automáticamente.

1) Registra cada compra en costeo-pasteleria/tCompras.csv con las columnas: Codigo, Fecha (AAAA-MM-DD), Cantidad_compra, Unidad_compra, Precio_compra_COP, Proveedor.
2) En tInsumos, usa las siguientes columnas y fórmulas (Excel 365):
   - Precio_vigente_COP:
     =BUSCARX(MAX.SI.CONJUNTO(tCompras[Fecha];tCompras[Codigo];[@Codigo]);FILTRO(tCompras[Fecha];tCompras[Codigo]=[@Codigo]);FILTRO(tCompras[Precio_compra_COP];tCompras[Codigo]=[@Codigo]);[@Precio_compra_COP])
   - Costo_unit_base_vigente:
     =[@Precio_vigente_COP]/[@Cant_base_compra]
   - Costo_utilizable_base_vigente:
     =[@Costo_unit_base_vigente]/(1-([@Merma_%]/100))
3) En tReceta, referencia Costo_utilizable_base_vigente para calcular Costo_parcial por insumo.
4) La hoja Costeo recalculará el costo total unitario y precio sugerido según margen e IVA.

Si tu Excel está en inglés, usa XLOOKUP, MAXIFS y FILTER con comas. El workflow de GitHub genera un archivo modelo-costeo.xlsx con estas tablas y fórmulas ya configuradas.
