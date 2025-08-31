# dulce-morita-bakery

## Modelo de Costeo para Pastelería

Este repositorio contiene un sistema completo de costeo para productos de pastelería, especialmente enfocado en muffins. El sistema genera automáticamente archivos Excel con fórmulas y tablas configuradas para calcular costos, márgenes y precios de venta.

### Características Principales

- **Gestión de Insumos**: Catálogo completo de ingredientes y materiales con precios actualizables
- **Recetas Detalladas**: Especificaciones exactas de cantidades por producto
- **Cálculo Automático**: Conversión de unidades, costos unitarios y totales
- **Pricing Inteligente**: Cálculo de precios con márgenes e IVA
- **Histórico de Compras**: Sistema opcional para seguimiento de precios históricos

### Estructura del Proyecto

```
costeo-pasteleria/
├── Insumos.csv              # Catálogo de ingredientes y materiales
├── Productos.csv            # Definiciones de productos (muffins)
├── RecetaDetalle.csv        # Recetas detalladas por producto
├── tCompras.csv             # Histórico de compras (opcional)
├── INSTRUCCIONES_COMPRAS.md # Guía para manejo de compras
└── modelo-costeo.xlsx       # Archivo Excel generado
```

### Uso Rápido

1. **Generar archivo Excel:**
   ```bash
   python scripts/build_excel.py
   ```

2. **Abrir el Excel generado:**
   - El archivo `costeo-pasteleria/modelo-costeo.xlsx` contiene todas las tablas y fórmulas configuradas
   - Incluye hojas para tInsumos, tProductos, tReceta, tCompras y Costeo

3. **Actualizar precios:**
   - Modo simple: Editar directamente en `Insumos.csv` 
   - Modo avanzado: Agregar compras en `tCompras.csv` para histórico automático

### Productos Incluidos

- **Muffin de chocolate (sin relleno)**: 12 unidades por lote, 45 min de producción
- **Muffin de vainilla (sin relleno)**: 12 unidades por lote, 40 min de producción

### Documentación Completa

Ver `costeo-pasteleria/INSTRUCCIONES_COMPRAS.md` para instrucciones detalladas sobre:
- Importación de CSVs a Excel
- Configuración de fórmulas
- Mejores prácticas para actualización de precios
- Gestión del histórico de compras