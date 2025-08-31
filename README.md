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

#### Generación Local
1. **Generar archivo Excel manualmente:**
   ```bash
   python scripts/build_excel.py
   ```

2. **Abrir el Excel generado:**
   - El archivo `costeo-pasteleria/modelo-costeo.xlsx` contiene todas las tablas y fórmulas configuradas
   - Incluye hojas para tInsumos, tProductos, tReceta, tCompras y Costeo

#### Automatización via GitHub Actions
El repositorio incluye un workflow de GitHub Actions que automatiza la generación del archivo Excel:

1. **Triggers automáticos**: El workflow se ejecuta cuando:
   - Se modifica cualquier archivo CSV en `costeo-pasteleria/`
   - Se modifica el script `scripts/build_excel.py`
   - Se ejecuta manualmente desde la interfaz de GitHub

2. **Ejecución manual**:
   - Ve a la pestaña "Actions" en GitHub
   - Selecciona "Build Excel Costing Model"
   - Haz clic en "Run workflow" y selecciona la rama `costeo-pasteleria`

3. **Configuración requerida**:
   - **IMPORTANTE**: Para que el workflow pueda hacer commits automáticos, ve a:
     - Configuración del repositorio → Actions → General → Workflow permissions
     - Selecciona "Read and write permissions"
   - El workflow instalará automáticamente las dependencias desde `requirements.txt`
   - El archivo Excel actualizado se commitea automáticamente si hay cambios

3. **Actualizar precios:**
   - Modo simple: Editar directamente en `Insumos.csv` 
   - Modo avanzado: Agregar compras en `tCompras.csv` para histórico automático
   - **Con automation**: Los cambios en CSVs en la rama `costeo-pasteleria` activarán automáticamente la regeneración del Excel

### Productos Incluidos

- **Muffin de chocolate (sin relleno)**: 12 unidades por lote, 45 min de producción
- **Muffin de vainilla (sin relleno)**: 12 unidades por lote, 40 min de producción

### Documentación Completa

Ver `costeo-pasteleria/INSTRUCCIONES_COMPRAS.md` para instrucciones detalladas sobre:
- Importación de CSVs a Excel
- Configuración de fórmulas
- Mejores prácticas para actualización de precios
- Gestión del histórico de compras