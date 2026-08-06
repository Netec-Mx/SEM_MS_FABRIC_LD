# Flujo de trabajo de extremo a extremo en Microsoft Fabric con Copilot

## Metadatos

| Campo | Valor |
|-------|-------|
| **Duración** | 30 minutos |
| **Complejidad** | Media |
| **Nivel Bloom** | Aplicar (Apply) |
| **Tipo** | Demostración guiada por el instructor |

## Descripción General

En esta práctica, el instructor ejecutará un flujo de trabajo analítico completo dentro de Microsoft Fabric: desde la ingesta de un archivo Excel con datos de ventas ficticias hacia un Lakehouse, pasando por la transformación y exploración de los datos, hasta la creación de un reporte interactivo en Power BI. Adicionalmente, se demostrará cómo Microsoft Copilot asiste en la generación de consultas, transformaciones y narrativas analíticas dentro de la plataforma.

## Objetivos de Aprendizaje

Al completar esta práctica, los participantes serán capaces de:

- [ ] Conectar una fuente de datos externa (archivo Excel `.xlsx`) a un Lakehouse dentro de Microsoft Fabric
- [ ] Preparar y transformar datos utilizando Dataflow Gen2 (renombrar columnas, filtrar nulos, agregar columnas calculadas)
- [ ] Explorar datos mediante el SQL Analytics Endpoint ejecutando consultas de verificación
- [ ] Crear un reporte de Power BI con al menos tres visualizaciones conectadas a los datos preparados en Fabric
- [ ] Identificar las capacidades de Microsoft Copilot para generar narrativas automáticas y asistir en transformaciones de datos

## Prerrequisitos

### Conocimientos Previos

| Requisito | Detalle |
|-----------|---------|
| Lab 01-00-01 completado | El espacio de trabajo `FabricCurso-Demo` debe existir y estar accesible |
| Interfaz de Microsoft Fabric | Navegación básica entre experiencias (adquirida en Lab 01-00-01) |
| SQL básico | Sentencias SELECT, WHERE, GROUP BY |
| Power Query básico | Familiaridad con transformaciones visuales de datos |

### Acceso y Recursos

| Recurso | Estado requerido |
|---------|-----------------|
| Cuenta Microsoft 365 en tenant educativo | Sesión activa en `https://app.fabric.microsoft.com` |
| Licencia Fabric Trial | Activa con capacidad de prueba asignada al workspace |
| Microsoft Copilot en Fabric | Habilitado por el administrador del tenant |
| Archivo `VentasEjemplo_2024.xlsx` | Disponible en `C:\FabricCurso\Datos\VentasEjemplo_2024.xlsx` |
| Espacio de trabajo `FabricCurso-Demo` | Creado en Lab 01-00-01 con Fabric Trial Capacity |

## Entorno del Laboratorio

### Hardware Mínimo

| Componente | Mínimo | Recomendado |
|------------|--------|-------------|
| Procesador | Intel Core i5 8ª gen / AMD Ryzen 5 3000 | Superior |
| RAM | 8 GB | 16 GB |
| Almacenamiento libre | 10 GB | 20 GB |
| Conexión Internet | 10 Mbps descarga | 25 Mbps |
| Resolución pantalla | 1366 × 768 | 1920 × 1080 |

### Software Requerido

| Software | Versión |
|----------|---------|
| Microsoft Edge | 124.0.2478.97 |
| Google Chrome (alternativa) | 124.0.6367.119 |
| Microsoft Excel | Microsoft 365 Apps, versión 2404 (Build 17531.20152) |
| Microsoft Fabric (SaaS) | April 2024 Release |
| Power BI Desktop (opcional, referencia) | 2.128.751.0 |

### Verificación Previa del Archivo de Datos

Antes de iniciar, confirme que el archivo fuente cumple con la estructura esperada:

| Columna | Tipo | Ejemplo |
|---------|------|---------|
| FechaVenta | DATE | 2024-03-15 |
| Producto | TEXT | Laptop Pro 15 |
| Categoria | TEXT | Electrónica |
| Region | TEXT | Norte |
| Vendedor | TEXT | Ana García |
| UnidadesVendidas | INTEGER | 12 |
| PrecioUnitario | DECIMAL | 899.99 |
| CostoUnitario | DECIMAL | 650.00 |

> **Nota:** El archivo debe contener un mínimo de 500 registros ficticios.

---

## Procedimiento Paso a Paso

### Paso 1: Crear el Lakehouse `LH_VentasCurso`

**Objetivo:** Crear el almacén Lakehouse que servirá como destino central de los datos dentro del espacio de trabajo existente.

**Instrucciones:**

1. Abra Microsoft Edge y navegue a `https://app.fabric.microsoft.com`.
2. En el panel de navegación izquierdo, seleccione **Workspaces** y haga clic en el espacio de trabajo **FabricCurso-Demo**.
3. Verifique que la capacidad asignada sea **Fabric Trial Capacity** (visible en la esquina inferior del panel del workspace).
4. Haga clic en el botón **+ New** (esquina superior izquierda del workspace).
5. En el menú desplegable, seleccione **More options** para ver todos los tipos de artefactos.
6. En la sección **Data Engineering**, seleccione **Lakehouse**.
7. En el cuadro de diálogo de creación, ingrese el nombre exacto:

   ```
   LH_VentasCurso
   ```

8. Haga clic en **Create**.
9. Espere a que Fabric complete la creación (10-20 segundos). Se abrirá automáticamente la vista del Lakehouse.

**Resultado Esperado:**

Se muestra la interfaz del Lakehouse `LH_VentasCurso` con dos secciones vacías: **Tables** y **Files** en el panel del explorador izquierdo.

**Verificación:**

- El nombre `LH_VentasCurso` aparece en la barra de título del Lakehouse.
- En el breadcrumb superior se muestra: `FabricCurso-Demo > LH_VentasCurso`.
- Las secciones Tables y Files están vacías (sin datos aún).

---

### Paso 2: Cargar el archivo de datos al Lakehouse

**Objetivo:** Ingerir el archivo `VentasEjemplo_2024.xlsx` al Lakehouse utilizando la funcionalidad de carga de archivos de la experiencia Data Engineering.

**Instrucciones:**

1. Dentro de la vista del Lakehouse `LH_VentasCurso`, localice la sección **Files** en el panel del explorador izquierdo.
2. Haga clic en los tres puntos (**...**) junto a **Files** y seleccione **Upload** → **Upload files**.
3. En el cuadro de diálogo del explorador de archivos de Windows, navegue a:

   ```
   C:\FabricCurso\Datos\
   ```

4. Seleccione el archivo **VentasEjemplo_2024.xlsx** y haga clic en **Open**.
5. Espere a que la carga se complete (barra de progreso en la parte superior). Con 500+ registros en Excel, la carga tomará aproximadamente 5-15 segundos.
6. Una vez cargado, confirme que el archivo aparece listado bajo la carpeta **Files** en el explorador.
7. Ahora convierta el archivo a una tabla Delta. Haga clic derecho sobre el archivo `VentasEjemplo_2024.xlsx` en el explorador y seleccione **Load to Tables** → **New table**.
8. En el cuadro de diálogo:
   - **Table name:** `ventas_raw`
   - **File type:** se detectará automáticamente como Excel
   - Confirme que la hoja correcta está seleccionada (generalmente `Sheet1` o `Hoja1`)
9. Haga clic en **Load**.
10. Espere la conversión (30-60 segundos). Una notificación confirmará que la tabla fue creada exitosamente.

**Resultado Esperado:**

La tabla `ventas_raw` aparece bajo la sección **Tables** del Lakehouse con un icono de tabla Delta. Al hacer clic en ella, se muestra una vista previa con las 8 columnas del archivo fuente y los registros cargados.

**Verificación:**

- Haga clic en la tabla `ventas_raw` y confirme que la vista previa muestra datos.
- Verifique que el conteo de filas sea ≥ 500 (visible en la barra de estado inferior o ejecutando una consulta en el paso siguiente).
- Confirme que las 8 columnas esperadas están presentes: FechaVenta, Producto, Categoria, Region, Vendedor, UnidadesVendidas, PrecioUnitario, CostoUnitario.

---

### Paso 3: Explorar datos con el SQL Analytics Endpoint

**Objetivo:** Utilizar el SQL Analytics Endpoint del Lakehouse para ejecutar consultas SQL de verificación sobre los datos cargados.

**Instrucciones:**

1. En la esquina superior derecha de la vista del Lakehouse, localice el selector de vista que muestra **Lakehouse** como modo actual.
2. Haga clic en el desplegable y seleccione **SQL Analytics Endpoint**.
3. Espere a que se cargue la interfaz del endpoint SQL (5-10 segundos). Aparecerá el editor de consultas SQL con el panel de esquema a la izquierda mostrando la tabla `ventas_raw`.
4. En el editor de consultas, escriba y ejecute la siguiente consulta de conteo:

   ```sql
   -- Verificar el número total de registros cargados
   SELECT COUNT(*) AS TotalRegistros
   FROM dbo.ventas_raw;
   ```

5. Haga clic en **Run** (▶) o presione `Shift + Enter`. Confirme que el resultado es ≥ 500.
6. Ejecute una segunda consulta para explorar la distribución por categoría:

   ```sql
   -- Distribución de ventas por categoría
   SELECT 
       Categoria,
       COUNT(*) AS NumeroRegistros,
       SUM(UnidadesVendidas) AS TotalUnidades,
       ROUND(SUM(UnidadesVendidas * PrecioUnitario), 2) AS IngresoTotal
   FROM dbo.ventas_raw
   GROUP BY Categoria
   ORDER BY IngresoTotal DESC;
   ```

7. Revise los resultados y señale a los participantes cómo el SQL Analytics Endpoint permite consultar datos del Lakehouse sin mover información.
8. Ejecute una tercera consulta para verificar la existencia de valores nulos:

   ```sql
   -- Verificar registros con valores nulos en columnas críticas
   SELECT 
       SUM(CASE WHEN UnidadesVendidas IS NULL THEN 1 ELSE 0 END) AS NulosUnidades,
       SUM(CASE WHEN PrecioUnitario IS NULL THEN 1 ELSE 0 END) AS NulosPrecio,
       SUM(CASE WHEN CostoUnitario IS NULL THEN 1 ELSE 0 END) AS NulosCosto,
       SUM(CASE WHEN Region IS NULL THEN 1 ELSE 0 END) AS NulosRegion
   FROM dbo.ventas_raw;
   ```

**Resultado Esperado:**

- Primera consulta: un valor numérico ≥ 500.
- Segunda consulta: tabla con categorías, conteos y totales de ingreso ordenados de mayor a menor.
- Tercera consulta: valores numéricos que indican la cantidad de nulos por columna (pueden ser 0 o un número pequeño dependiendo del archivo preparado).

**Verificación:**

- Los resultados de las consultas se muestran correctamente en el panel de resultados inferior.
- No se producen errores de sintaxis ni de conexión.
- Los nombres de columna coinciden exactamente con los del archivo fuente.

---

### Paso 4: Crear un Dataflow Gen2 para transformar los datos

**Objetivo:** Construir un Dataflow Gen2 (`DF_LimpiezaVentas`) que limpie y transforme los datos: filtrar nulos, estandarizar nombres de columna y agregar una columna calculada de margen de ganancia.

**Instrucciones:**

1. Regrese al espacio de trabajo `FabricCurso-Demo` haciendo clic en el nombre del workspace en el breadcrumb superior.
2. Haga clic en **+ New** → **More options**.
3. En la sección **Data Factory**, seleccione **Dataflow Gen2**.
4. Se abrirá el editor de Power Query Online. En la barra de título superior, haga clic en el nombre por defecto (`Dataflow 1`) y renómbrelo a:

   ```
   DF_LimpiezaVentas
   ```

5. **Conectar al origen de datos:** En el panel central, haga clic en **Get data** → **More...**.
6. En el buscador de conectores, escriba `Lakehouse` y seleccione **Microsoft Fabric Lakehouse**.
7. Configure la conexión:
   - Seleccione el workspace: **FabricCurso-Demo**
   - Seleccione el Lakehouse: **LH_VentasCurso**
   - Expanda **Tables** y marque la tabla **ventas_raw**
   - Haga clic en **Create**
8. Se cargará una vista previa de los datos en el editor Power Query.

9. **Transformación 1 — Filtrar valores nulos:** 
   - Haga clic en la flecha desplegable del encabezado de la columna `UnidadesVendidas`.
   - Desmarque la opción **(null)** si aparece en la lista de valores.
   - Haga clic en **OK**.
   - Repita para las columnas `PrecioUnitario` y `CostoUnitario`.

10. **Transformación 2 — Agregar columna calculada de Margen:**
    - En la cinta superior, haga clic en la pestaña **Add Column**.
    - Seleccione **Custom Column**.
    - En el cuadro de diálogo:
      - **New column name:** `MargenGanancia`
      - **Custom column formula:**
      
        ```
        ([PrecioUnitario] - [CostoUnitario]) / [PrecioUnitario]
        ```
      
    - Haga clic en **OK**.
    - La nueva columna `MargenGanancia` aparecerá con valores decimales (proporción entre 0 y 1).

11. **Transformación 3 — Agregar columna de Ingreso Total:**
    - Repita el proceso de **Add Column** → **Custom Column**.
    - **New column name:** `IngresoTotal`
    - **Custom column formula:**
    
      ```
      [UnidadesVendidas] * [PrecioUnitario]
      ```
    
    - Haga clic en **OK**.

12. **Configurar destino de datos:**
    - En la esquina inferior derecha del editor, haga clic en el icono de configuración del destino de datos (o en la cinta: **Home** → **Add data destination** → **Lakehouse**).
    - Seleccione el workspace **FabricCurso-Demo** y el Lakehouse **LH_VentasCurso**.
    - En **Table name**, seleccione **New table** e ingrese: `ventas_preparadas`.
    - En **Update method**, seleccione **Replace**.
    - Haga clic en **Next** y luego **Save settings**.

13. **Publicar el Dataflow:**
    - Haga clic en el botón **Publish** en la esquina inferior derecha.
    - El Dataflow se ejecutará automáticamente. Espere a que el estado cambie a **Succeeded** (1-3 minutos).

**Resultado Esperado:**

El Dataflow `DF_LimpiezaVentas` se publica y ejecuta exitosamente. En el Lakehouse `LH_VentasCurso`, aparece una nueva tabla `ventas_preparadas` con las columnas originales más `MargenGanancia` e `IngresoTotal`, sin registros con valores nulos en las columnas numéricas.

**Verificación:**

- Regrese al Lakehouse `LH_VentasCurso` y confirme que la tabla `ventas_preparadas` existe bajo **Tables**.
- Haga clic en la tabla para ver la vista previa: debe mostrar 10 columnas (8 originales + 2 calculadas).
- La columna `MargenGanancia` debe mostrar valores entre 0 y 1.
- La columna `IngresoTotal` debe mostrar valores positivos coherentes con UnidadesVendidas × PrecioUnitario.

---

### Paso 5: Crear el Modelo Semántico

**Objetivo:** Generar un modelo semántico (Semantic Model) basado en la tabla `ventas_preparadas` para habilitar la creación de reportes en Power BI.

**Instrucciones:**

1. Dentro del Lakehouse `LH_VentasCurso`, cambie a la vista **SQL Analytics Endpoint** (selector en la esquina superior derecha).
2. En el panel izquierdo del SQL Analytics Endpoint, localice la sección **Model** en la barra de navegación inferior (o haga clic en la pestaña **Model** en la barra superior del endpoint).
3. Se abrirá la vista de modelado. Aquí verá las tablas disponibles como entidades del modelo.
4. Alternativamente, para crear un modelo semántico dedicado con nombre específico:
   - Regrese al workspace `FabricCurso-Demo`.
   - Haga clic en **+ New** → **More options**.
   - En la sección **Power BI**, seleccione **Semantic model**.
   - **Name:** `SM_VentasCurso`
   - **Workspace:** FabricCurso-Demo
   - Seleccione como fuente el **SQL Analytics Endpoint** de `LH_VentasCurso`.
   - Marque la tabla `ventas_preparadas`.
   - Haga clic en **Confirm**.

5. Una vez creado, se abrirá la vista del modelo. Verifique que la tabla `ventas_preparadas` está visible con todas sus columnas.

6. **(Opcional)** En la vista de modelo, puede configurar formatos:
   - Haga clic en la columna `MargenGanancia` y establezca el formato como **Percentage** en el panel de propiedades.
   - Haga clic en `IngresoTotal` y establezca el formato como **Currency**.

**Resultado Esperado:**

El modelo semántico `SM_VentasCurso` queda creado en el workspace `FabricCurso-Demo`, conectado a la tabla `ventas_preparadas` del Lakehouse.

**Verificación:**

- En el workspace, el artefacto `SM_VentasCurso` aparece con el icono de modelo semántico (dataset).
- Al abrirlo, la tabla `ventas_preparadas` es visible con sus 10 columnas.

---

### Paso 6: Crear el Reporte de Power BI

**Objetivo:** Construir un reporte de Power BI (`RPT_VentasCurso`) con tres visualizaciones clave conectadas al modelo semántico.

**Instrucciones:**

1. Desde el workspace `FabricCurso-Demo`, localice el modelo semántico `SM_VentasCurso`.
2. Haga clic en los tres puntos (**...**) junto al modelo y seleccione **Create report** (o **Auto-create a report** para una versión rápida).
3. Se abrirá el editor de reportes de Power BI en el navegador. El panel **Data** a la derecha mostrará los campos de la tabla `ventas_preparadas`.

4. **Visualización 1 — Gráfico de barras: Ventas por Categoría:**
   - En el panel de visualizaciones, seleccione el icono de **Clustered bar chart** (gráfico de barras agrupadas).
   - Arrastre el campo `Categoria` al eje **Y-axis**.
   - Arrastre el campo `IngresoTotal` al área **X-axis**.
   - El gráfico mostrará las categorías ordenadas por ingreso total.
   - Ajuste el título del gráfico a: "Ventas por Categoría".

5. **Visualización 2 — Tarjeta KPI: Ventas Totales:**
   - Haga clic en un área vacía del lienzo para deseleccionar el gráfico anterior.
   - En el panel de visualizaciones, seleccione el icono de **Card** (tarjeta).
   - Arrastre el campo `IngresoTotal` al área **Fields** de la tarjeta.
   - La tarjeta mostrará la suma total de ingresos.
   - Ajuste el título a: "Ventas Totales (2024)".
   - En el panel de formato, configure el valor para mostrar formato de moneda.

6. **Visualización 3 — Gráfico de líneas: Tendencia Mensual:**
   - Haga clic en un área vacía del lienzo.
   - Seleccione el icono de **Line chart** (gráfico de líneas).
   - Arrastre el campo `FechaVenta` al eje **X-axis**. Power BI creará automáticamente una jerarquía de fecha (Año > Trimestre > Mes > Día).
   - En el eje X, haga clic en la flecha de expansión para bajar al nivel de **Month** (Mes).
   - Arrastre `IngresoTotal` al eje **Y-axis**.
   - Ajuste el título a: "Tendencia Mensual de Ventas".

7. **Organizar el lienzo:**
   - Redimensione y posicione las tres visualizaciones para que el reporte sea legible:
     - Tarjeta KPI en la esquina superior izquierda.
     - Gráfico de barras en la mitad derecha.
     - Gráfico de líneas en la parte inferior, ocupando todo el ancho.

8. **Guardar el reporte:**
   - Haga clic en **File** → **Save**.
   - **Name:** `RPT_VentasCurso`
   - **Workspace:** FabricCurso-Demo
   - Haga clic en **Save**.

**Resultado Esperado:**

El reporte `RPT_VentasCurso` se guarda en el workspace con tres visualizaciones funcionales que muestran datos reales del Lakehouse: barras por categoría, tarjeta de KPI y línea de tendencia mensual.

**Verificación:**

- Las tres visualizaciones muestran datos (no están vacías ni muestran errores).
- La tarjeta KPI muestra un valor numérico coherente (suma de todos los ingresos).
- El gráfico de barras muestra múltiples categorías con barras de diferentes longitudes.
- El gráfico de líneas muestra una tendencia a lo largo de los meses de 2024.
- El reporte aparece en el workspace `FabricCurso-Demo` con el nombre `RPT_VentasCurso`.

---

### Paso 7: Demostración de Microsoft Copilot en Power BI

**Objetivo:** Mostrar cómo Copilot genera narrativas automáticas y sugiere visualizaciones adicionales dentro del reporte de Power BI.

**Instrucciones:**

1. Con el reporte `RPT_VentasCurso` abierto en modo de edición, localice el icono de **Copilot** en la cinta superior (icono con forma de estrella/chispa, generalmente en la pestaña **Home**).
2. Haga clic en el botón **Copilot**. Se abrirá el panel de Copilot en el lado derecho del editor.
3. **Generar narrativa automática:**
   - En el campo de texto de Copilot, escriba:

     ```
     Genera un resumen ejecutivo de este reporte describiendo las principales tendencias de ventas.
     ```

   - Presione **Enter** o haga clic en **Send**.
   - Copilot generará un texto descriptivo que resume los hallazgos principales del reporte (categoría con mayor venta, tendencia mensual, etc.).
   - Muestre a los participantes cómo este texto puede insertarse como una visualización de tipo **Narrative** en el reporte.

4. **Solicitar sugerencia de visualización:**
   - En el panel de Copilot, escriba:

     ```
     Sugiere una visualización adicional que muestre el margen de ganancia promedio por región.
     ```

   - Copilot propondrá un tipo de gráfico y la configuración de campos. Puede ofrecer crear la visualización directamente.
   - Si Copilot ofrece la opción de agregar la visualización, haga clic en **Add to report** para demostrar la capacidad.

5. **Generar una medida DAX con Copilot:**
   - En el panel de Copilot, escriba:

     ```
     Crea una medida DAX que calcule el crecimiento porcentual de ventas mes a mes.
     ```

   - Copilot generará una fórmula DAX similar a:

     ```dax
     CrecimientoMoM = 
     VAR VentasMesActual = [Total Ventas]
     VAR VentasMesAnterior = CALCULATE([Total Ventas], DATEADD('ventas_preparadas'[FechaVenta], -1, MONTH))
     RETURN
     DIVIDE(VentasMesActual - VentasMesAnterior, VentasMesAnterior, 0)
     ```

   - Explique a los participantes que esta fórmula puede revisarse, ajustarse y aplicarse directamente al modelo.

6. Señale a los participantes las siguientes capacidades clave de Copilot en Power BI:
   - Generación de narrativas textuales automáticas.
   - Sugerencia de visualizaciones basadas en los datos disponibles.
   - Creación de medidas DAX mediante lenguaje natural.
   - Respuesta a preguntas sobre los datos del reporte.

**Resultado Esperado:**

Copilot responde con narrativas coherentes, sugerencias de visualización relevantes y fórmulas DAX sintácticamente correctas basadas en el contexto del modelo semántico.

**Verificación:**

- El panel de Copilot muestra respuestas textuales relacionadas con los datos del reporte.
- Las sugerencias de visualización hacen referencia a campos existentes en el modelo (`Categoria`, `Region`, `MargenGanancia`, etc.).
- La fórmula DAX generada es sintácticamente válida (sin errores de compilación si se intenta aplicar).

> **Nota importante:** Si Copilot no está disponible o muestra un mensaje de que no está habilitado, verifique que el administrador del tenant haya activado la configuración en: **Admin Portal** → **Tenant Settings** → **Copilot and Azure OpenAI Service** → **Enabled**. La capacidad Fabric Trial también debe estar activa.

---

### Paso 8: Demostración de Copilot en Data Engineering

**Objetivo:** Mostrar brevemente cómo Copilot asiste en la escritura de código de transformación dentro de un Notebook de Spark en la experiencia Data Engineering.

**Instrucciones:**

1. Regrese al workspace `FabricCurso-Demo`.
2. Haga clic en **+ New** → **More options** → **Notebook** (en la sección Data Engineering).
3. Se abrirá un nuevo Notebook de Spark. En la primera celda, localice el icono de **Copilot** (chat/asistente) en la barra de herramientas del notebook o en el panel lateral.
4. Active el panel de Copilot haciendo clic en el icono.
5. En el campo de texto de Copilot, escriba:

   ```
   Lee la tabla ventas_preparadas del Lakehouse y calcula las ventas totales por región y mes, mostrando los resultados ordenados por mes.
   ```

6. Copilot generará código PySpark similar a:

   ```python
   # Código sugerido por Copilot
   from pyspark.sql.functions import col, sum, month, year

   df = spark.read.format("delta").load("Tables/ventas_preparadas")

   df_resultado = (
       df.withColumn("Mes", month(col("FechaVenta")))
         .withColumn("Anio", year(col("FechaVenta")))
         .groupBy("Region", "Anio", "Mes")
         .agg(sum("IngresoTotal").alias("VentasTotales"))
         .orderBy("Anio", "Mes")
   )

   display(df_resultado)
   ```

7. Haga clic en **Accept** o copie el código a la celda del notebook.
8. Ejecute la celda haciendo clic en **Run cell** (▶) o presionando `Shift + Enter`.
9. Muestre a los participantes:
   - Cómo Copilot interpreta la solicitud en lenguaje natural.
   - Cómo genera código PySpark válido con las funciones apropiadas.
   - Cómo el resultado se muestra como tabla interactiva en el notebook.

10. **(Opcional)** Pida a Copilot una transformación adicional:

    ```
    Agrega una columna que clasifique las regiones en "Alto rendimiento" si las ventas superan 50000 y "Bajo rendimiento" en caso contrario.
    ```

11. Cierre el notebook sin guardar (es solo una demostración) o guárdelo como `NB_DemoCopilot` si desea conservarlo.

**Resultado Esperado:**

Copilot genera código PySpark funcional que se ejecuta correctamente sobre la tabla del Lakehouse, mostrando resultados tabulares.

**Verificación:**

- El código generado por Copilot se ejecuta sin errores.
- La tabla de resultados muestra columnas Region, Anio, Mes y VentasTotales con datos coherentes.
- Los participantes pueden observar la interacción entre lenguaje natural y código generado.

---

## Validación y Pruebas

Al finalizar todos los pasos, verifique que los siguientes artefactos existen en el workspace `FabricCurso-Demo`:

| Artefacto | Tipo | Estado esperado |
|-----------|------|-----------------|
| `LH_VentasCurso` | Lakehouse | Contiene tablas `ventas_raw` y `ventas_preparadas` |
| `DF_LimpiezaVentas` | Dataflow Gen2 | Estado: Succeeded (última ejecución exitosa) |
| `SM_VentasCurso` | Semantic Model | Conectado a `ventas_preparadas` |
| `RPT_VentasCurso` | Power BI Report | 3 visualizaciones funcionales con datos |
| SQL Analytics Endpoint | (automático) | Generado automáticamente con `LH_VentasCurso` |

**Prueba de integridad del flujo:**

1. En el workspace, abra `RPT_VentasCurso`.
2. Verifique que las visualizaciones cargan datos actualizados.
3. Interactúe con el gráfico de barras (haga clic en una categoría) y confirme que los demás gráficos se filtran correctamente (cross-filtering).
4. Confirme que la tarjeta KPI muestra un valor total coherente con la suma de `IngresoTotal` verificada en el Paso 3.

---

## Solución de Problemas

### Problema 1: La tabla no aparece en el SQL Analytics Endpoint

**Síntomas:** Después de cargar datos al Lakehouse y crear la tabla `ventas_raw`, al cambiar al SQL Analytics Endpoint la tabla no es visible en el panel de esquema.

**Causa:** El SQL Analytics Endpoint puede tardar hasta 60 segundos en sincronizar los metadatos de nuevas tablas Delta creadas en el Lakehouse. En algunos casos, la caché del navegador también puede mostrar un estado desactualizado.

**Solución:**

1. Espere 30-60 segundos y haga clic en el botón **Refresh** (🔄) en el panel del explorador de esquema del SQL Analytics Endpoint.
2. Si la tabla sigue sin aparecer, cierre la pestaña del navegador y vuelva a abrir el Lakehouse desde el workspace.
3. Verifique que la tabla fue creada correctamente regresando a la vista **Lakehouse** (no SQL Endpoint) y confirmando que la tabla existe bajo **Tables**.
4. Si el problema persiste, verifique que la capacidad Fabric Trial no se ha pausado: en el workspace, confirme que no aparece un banner de "capacity paused".

---

### Problema 2: Copilot no está disponible o muestra mensaje de error

**Síntomas:** Al hacer clic en el icono de Copilot (en Power BI o en el Notebook), aparece un mensaje como "Copilot is not available" o "This feature requires Copilot to be enabled by your administrator", o el icono simplemente no aparece en la interfaz.

**Causa:** Copilot requiere tres condiciones simultáneas: (1) habilitación por el administrador del tenant, (2) capacidad Fabric activa (no pausada ni expirada), y (3) la región del tenant debe ser compatible con Azure OpenAI Service.

**Solución:**

1. Verifique la configuración del administrador:
   - Navegue a `https://app.powerbi.com/admin-portal/tenantSettings`.
   - Busque la sección **Copilot and Azure OpenAI Service**.
   - Confirme que está **Enabled** para toda la organización o para el grupo de seguridad del instructor.
2. Verifique que la Fabric Trial Capacity esté activa:
   - En el workspace, confirme que no hay advertencias sobre la capacidad.
   - En **Settings** → **License info**, verifique la fecha de expiración del trial.
3. Si la región no es compatible, Copilot no estará disponible. En este caso, explique la funcionalidad verbalmente o muestre capturas de pantalla preparadas previamente (ubicadas en `C:\FabricCurso\Capturas\`).
4. Como alternativa para la demostración, muestre la documentación oficial de Copilot con ejemplos: `https://learn.microsoft.com/fabric/get-started/copilot-fabric-overview`.

---

## Limpieza

> **Importante:** NO elimine los artefactos creados en esta práctica. El workspace `FabricCurso-Demo` y todos sus contenidos (`LH_VentasCurso`, `DF_LimpiezaVentas`, `SM_VentasCurso`, `RPT_VentasCurso`) deben permanecer intactos como referencia para los participantes durante el resto del curso.

Si se creó el notebook de demostración de Copilot y no desea conservarlo:

1. En el workspace `FabricCurso-Demo`, localice el notebook `NB_DemoCopilot` (si fue guardado).
2. Haga clic en los tres puntos (**...**) junto al notebook.
3. Seleccione **Delete** y confirme.

---

## Resumen

En esta práctica se completó un flujo de trabajo analítico de extremo a extremo en Microsoft Fabric:

| Fase | Carga de trabajo | Artefacto creado |
|------|-----------------|------------------|
| **Ingesta** | Data Engineering (Lakehouse) | `LH_VentasCurso` → tabla `ventas_raw` |
| **Transformación** | Data Factory (Dataflow Gen2) | `DF_LimpiezaVentas` → tabla `ventas_preparadas` |
| **Exploración** | SQL Analytics Endpoint | Consultas SQL de verificación |
| **Modelado** | Power BI (Semantic Model) | `SM_VentasCurso` |
| **Visualización** | Power BI (Report) | `RPT_VentasCurso` |
| **IA Asistida** | Microsoft Copilot | Narrativas, DAX y código PySpark |

### Conceptos Clave Demostrados

- **Data Factory** (Dataflow Gen2) permite transformar datos visualmente sin código, aplicando filtros, columnas calculadas y configurando destinos en el Lakehouse.
- **Data Engineering** (Lakehouse + SQL Analytics Endpoint) proporciona almacenamiento unificado en formato Delta Lake con acceso SQL inmediato sin duplicación de datos.
- **OneLake** actúa como capa de almacenamiento compartida: los datos cargados en el Lakehouse están disponibles instantáneamente para consultas SQL, modelos semánticos y reportes de Power BI.
- **Microsoft Copilot** acelera el trabajo analítico al generar código, fórmulas DAX y narrativas descriptivas a partir de solicitudes en lenguaje natural.
- La **nomenclatura consistente** (prefijos `LH_`, `DF_`, `SM_`, `RPT_`) facilita la organización y gestión de artefactos en entornos empresariales.

### Recursos Adicionales

- [Documentación oficial de Data Factory en Microsoft Fabric](https://learn.microsoft.com/es-es/fabric/data-factory/data-factory-overview)
- [Descripción general del Lakehouse en Microsoft Fabric](https://learn.microsoft.com/es-es/fabric/data-engineering/lakehouse-overview)
- [Creación de reportes en Power BI dentro de Fabric](https://learn.microsoft.com/es-es/fabric/data-engineering/create-powerbi-report)
- [Copilot en Microsoft Fabric - Visión general](https://learn.microsoft.com/es-es/fabric/get-started/copilot-fabric-overview)
- [Dataflow Gen2 en Microsoft Fabric](https://learn.microsoft.com/es-es/fabric/data-factory/create-first-dataflow-gen2)
