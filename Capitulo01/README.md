# Recorrido por la Interfaz de Microsoft Fabric — Espacios de Trabajo, Experiencias y OneLake

## 1. Metadatos

| Campo | Valor |
|-------|-------|
| **Duración** | 30 minutos |
| **Complejidad** | Fácil |
| **Nivel de Bloom** | Aplicar |
| **Modalidad** | Demostración guiada por el instructor (instructor-led demo) |
| **Espacio de trabajo resultante** | `FabricCurso-Demo` |

---

## 2. Descripción General

En esta práctica inaugural del curso, el instructor realizará un recorrido guiado por la interfaz web de Microsoft Fabric (https://app.fabric.microsoft.com). Se explorarán los elementos fundamentales de navegación, se crearán los recursos organizativos iniciales del curso y se demostrará cómo OneLake actúa como lago de datos unificado. Al finalizar, el espacio de trabajo `FabricCurso-Demo` quedará creado y configurado como base para todas las prácticas posteriores.

---

## 3. Objetivos de Aprendizaje

Al completar este laboratorio, los participantes serán capaces de:

- [ ] Identificar los elementos principales de la interfaz de Microsoft Fabric: menú de navegación lateral, panel de inicio (Home) y selector de experiencias (Experience Switcher).
- [ ] Crear y configurar un espacio de trabajo (Workspace) asignándole la capacidad Fabric Trial, reconociendo su rol como unidad organizativa de proyectos analíticos.
- [ ] Distinguir las seis cargas de trabajo (Workloads) principales de Microsoft Fabric: Data Factory, Data Engineering, Data Warehouse, Data Science, Real-Time Intelligence y Power BI.
- [ ] Describir el rol de OneLake como capa de almacenamiento centralizada que comparten todos los servicios de la plataforma.
- [ ] Explicar el flujo general de trabajo dentro de Microsoft Fabric, desde la ingesta de datos hasta la visualización.

---

## 4. Prerrequisitos

### Conocimientos previos

| Requisito | Nivel |
|-----------|-------|
| Conceptos básicos de análisis de datos | Familiaridad general |
| Uso de Microsoft Excel o Power BI | Básico |
| Navegación web y gestión de cuentas Microsoft 365 | Básico |

### Acceso y configuración previa

| Requisito | Detalle |
|-----------|---------|
| Cuenta Microsoft 365 | Activa, perteneciente al tenant educativo/de prueba del curso |
| Licencia Microsoft Fabric | Free Trial activada previamente en https://app.fabric.microsoft.com |
| Habilitación del tenant | El administrador debe haber habilitado *"Users can try Microsoft Fabric paid features"* en **Admin Portal > Tenant Settings > Microsoft Fabric** |
| Navegador compatible | Microsoft Edge 124.0.2478.97 **o** Google Chrome 124.0.6367.119 |
| Conexión a Internet | Mínimo 10 Mbps de descarga (25 Mbps recomendado) |

---

## 5. Entorno del Laboratorio

### Hardware mínimo del equipo del instructor

| Componente | Especificación |
|------------|---------------|
| Procesador | Intel Core i5 8.ª gen. o AMD Ryzen 5 3000+ (x64) |
| RAM | 8 GB mínimo (16 GB recomendado) |
| Almacenamiento libre | 10 GB |
| Resolución de pantalla | 1366 × 768 mínimo (1920 × 1080 recomendado) |

### Software requerido

| Software | Versión |
|----------|---------|
| Microsoft Edge | 124.0.2478.97 |
| Microsoft Fabric (SaaS) | April 2024 Release |
| Sistema operativo | Windows 10/11 (64-bit) |

### Estructura de directorios locales (máquina del instructor)

```
C:\FabricCurso\
├── Datos\          ← archivos fuente (VentasEjemplo_2024.xlsx)
├── Capturas\       ← capturas de pantalla de referencia
└── Materiales\     ← guías y presentaciones
```

> **Nota:** En esta práctica no se utilizan archivos de datos locales. La estructura se menciona para que los participantes conozcan la organización del curso.

---

## 6. Instrucciones Paso a Paso

---

### Paso 1 — Iniciar sesión en Microsoft Fabric y verificar la licencia Trial

**Objetivo:** Confirmar que la cuenta del curso tiene acceso a Microsoft Fabric y que la licencia de prueba (Fabric Trial) está activa.

**Instrucciones:**

1. Abra **Microsoft Edge** (versión 124.0.2478.97) en la máquina del instructor.
2. Navegue a la siguiente URL:

   ```
   https://app.fabric.microsoft.com
   ```

3. Inicie sesión con las credenciales Microsoft 365 del curso (cuenta del tenant educativo/de prueba).
4. Una vez cargado el portal, haga clic en el **icono de perfil** (esquina superior derecha).
5. En el menú desplegable, seleccione **"Account info"** o revise la etiqueta que indica el tipo de licencia.
6. Verifique que aparezca la indicación **"Fabric (Trial)"** o **"Trial"** junto al nombre de la cuenta.

**Resultado esperado:**

- El portal de Microsoft Fabric se carga correctamente mostrando la página de inicio (Home).
- En la información de la cuenta se confirma que la licencia de prueba de Fabric está activa.

**Verificación:**

- Si la etiqueta muestra *"Free"* sin mención de Fabric Trial, la prueba no está activada. Consulte la sección de Troubleshooting.
- Si aparece un mensaje indicando que Fabric no está habilitado para el tenant, contacte al administrador.

---

### Paso 2 — Explorar el Panel de Inicio (Home)

**Objetivo:** Reconocer las secciones principales de la página de inicio de Microsoft Fabric y su propósito.

**Instrucciones:**

1. Observe la **página de inicio (Home)** que se muestra tras el inicio de sesión. Identifique las siguientes secciones:

   | Sección | Ubicación | Propósito |
   |---------|-----------|-----------|
   | Barra superior | Parte superior | Búsqueda global, notificaciones, configuración, perfil |
   | Menú de navegación lateral (Nav pane) | Lado izquierdo | Acceso rápido a Home, Browse, Workspaces, Create |
   | Área central de contenido | Centro | Accesos directos recientes, recomendaciones, acciones rápidas |
   | Selector de experiencias (Experience Switcher) | Esquina inferior izquierda del nav pane | Cambiar entre cargas de trabajo |

2. Haga clic en **"Browse"** en el menú lateral izquierdo. Observe que se muestran los elementos recientes, compartidos y favoritos del usuario.

3. Regrese a **"Home"** haciendo clic en el icono de inicio (casa) del menú lateral.

4. En el área central, identifique la sección **"Quick access"** (o "Acceso rápido") que muestra los artefactos usados recientemente.

5. Identifique los **botones de creación rápida** en la parte superior del área central, que permiten crear nuevos artefactos directamente (por ejemplo: *Lakehouse*, *Warehouse*, *Report*, *Dataflow Gen2*).

**Resultado esperado:**

- Los participantes pueden señalar cada sección de la interfaz y describir su función.
- El menú lateral se expande y colapsa correctamente al hacer clic en el icono de hamburguesa (☰).

**Verificación:**

- Confirme que el menú lateral muestra al menos las opciones: *Home*, *Browse*, *Create*, *Workspaces*.
- Confirme que el selector de experiencias es visible en la parte inferior izquierda del panel de navegación.

---

### Paso 3 — Navegar por el Selector de Experiencias (Experience Switcher)

**Objetivo:** Identificar y distinguir las seis cargas de trabajo principales de Microsoft Fabric disponibles a través del selector de experiencias.

**Instrucciones:**

1. Localice el **Selector de Experiencias** en la esquina inferior izquierda del panel de navegación lateral. Actualmente debería mostrar un icono y nombre como *"Power BI"*, *"Data Engineering"* u otra experiencia predeterminada.

2. Haga clic en el selector de experiencias. Se desplegará un panel mostrando todas las experiencias disponibles.

3. Identifique y señale cada una de las siguientes cargas de trabajo:

   | Experiencia | Icono/Color | Propósito principal |
   |-------------|-------------|---------------------|
   | **Power BI** | Amarillo | Visualización de datos, informes interactivos y dashboards |
   | **Data Factory** | Verde | Integración y orquestación de datos (pipelines, dataflows) |
   | **Data Engineering** | Azul | Transformación de datos a escala con Apache Spark y notebooks |
   | **Data Warehouse** | Púrpura | Almacenamiento analítico con SQL estándar (T-SQL) |
   | **Data Science** | Turquesa | Machine learning, experimentación y modelos predictivos |
   | **Real-Time Intelligence** | Naranja | Análisis de datos en tiempo real (streaming, KQL) |

4. Haga clic en **"Data Factory"**. Observe cómo cambia la interfaz: el panel de inicio muestra opciones específicas de esta experiencia (crear Pipeline, Dataflow Gen2, etc.).

5. Repita el proceso seleccionando **"Data Engineering"**. Note las opciones de creación: Lakehouse, Notebook, Spark Job Definition.

6. Seleccione **"Data Warehouse"**. Observe la opción de crear un Warehouse.

7. Seleccione **"Data Science"**. Identifique las opciones: Notebook, Experiment, ML Model.

8. Seleccione **"Real-Time Intelligence"**. Observe las opciones: Eventhouse, KQL Database, KQL Queryset.

9. Finalmente, regrese a **"Power BI"**. Observe las opciones familiares: Report, Paginated Report, Scorecard.

**Resultado esperado:**

- Al cambiar de experiencia, la interfaz adapta su contenido y opciones de creación al contexto de la carga de trabajo seleccionada.
- El selector de experiencias muestra claramente cuál experiencia está activa en cada momento.

**Verificación:**

- Al seleccionar cada experiencia, el nombre de la misma aparece reflejado en el selector (esquina inferior izquierda).
- Las opciones de creación en el área central cambian de acuerdo con la experiencia seleccionada.

---

### Paso 4 — Crear el Espacio de Trabajo del Curso (`FabricCurso-Demo`)

**Objetivo:** Crear el espacio de trabajo principal del curso con la capacidad Fabric Trial asignada, estableciendo la unidad organizativa que se utilizará en todas las prácticas.

**Instrucciones:**

1. En el panel de navegación lateral izquierdo, haga clic en **"Workspaces"**.

2. En el panel que se despliega, haga clic en el botón **"+ New workspace"** (parte superior del panel de workspaces).

3. En el formulario de creación, complete los siguientes campos:

   | Campo | Valor |
   |-------|-------|
   | **Name** | `FabricCurso-Demo` |
   | **Description** (opcional) | `Espacio de trabajo principal del curso de Microsoft Fabric. Contiene todos los artefactos de las prácticas guiadas.` |

4. Expanda la sección **"Advanced"** (o "Avanzado") haciendo clic en ella.

5. En el campo **"License mode"** (Modo de licencia), seleccione **"Trial"** o **"Fabric capacity"**.

6. En el campo **"Capacity"** (si aparece), seleccione la capacidad de prueba disponible (Fabric Trial Capacity). El nombre puede variar según el tenant, pero generalmente incluye la palabra *"Trial"*.

   > ⚠️ **Importante:** Si no se asigna la capacidad Trial, las funcionalidades de Fabric no estarán disponibles en el espacio de trabajo. Solo se tendrán las capacidades básicas de Power BI.

7. Haga clic en **"Apply"** (Aplicar) para crear el espacio de trabajo.

8. Espere a que el sistema confirme la creación. El portal navegará automáticamente al interior del nuevo espacio de trabajo vacío.

**Resultado esperado:**

- El espacio de trabajo `FabricCurso-Demo` aparece en la lista de Workspaces.
- Al acceder al workspace, se muestra vacío con el mensaje *"This workspace is empty"* o similar.
- En la configuración del workspace se confirma que la capacidad asignada es la Trial.

**Verificación:**

1. Haga clic en el icono de **engranaje** (⚙️) o en **"Workspace settings"** (esquina superior derecha dentro del workspace).
2. Navegue a la pestaña **"License info"** o **"Premium"**.
3. Confirme que el campo de capacidad muestra **"Trial"** y que el modo de licencia es **Fabric capacity**.
4. Cierre la ventana de configuración.

```
Resultado esperado en Workspace Settings > License info:
┌─────────────────────────────────────────────┐
│ License mode: Fabric capacity               │
│ Capacity:     Trial (o nombre de la trial)  │
│ Status:       Active                        │
└─────────────────────────────────────────────┘
```

---

### Paso 5 — Explorar la Estructura del Espacio de Trabajo

**Objetivo:** Comprender la organización interna de un espacio de trabajo y cómo los artefactos de distintas experiencias coexisten en él.

**Instrucciones:**

1. Dentro del espacio de trabajo `FabricCurso-Demo`, observe la barra de herramientas superior que incluye:
   - **+ New** (para crear nuevos artefactos)
   - **Filter** (para filtrar por tipo de artefacto)
   - **Sort** (para ordenar elementos)
   - **View** (para cambiar entre vista de lista y vista de cuadrícula)

2. Haga clic en **"+ New"**. Observe el menú desplegable que muestra todos los tipos de artefactos que se pueden crear dentro del workspace, organizados por experiencia:

   - **Data Engineering:** Lakehouse, Notebook, Spark Job Definition, Environment
   - **Data Factory:** Data pipeline, Dataflow Gen2
   - **Data Warehouse:** Warehouse
   - **Data Science:** Notebook, Experiment, ML Model
   - **Real-Time Intelligence:** Eventhouse, KQL Database, KQL Queryset, Eventstream
   - **Power BI:** Report, Paginated report, Scorecard, Semantic model

3. **No cree ningún artefacto todavía.** Cierre el menú haciendo clic fuera de él o presionando `Esc`.

4. Haga clic en el filtro **"Filter"** y observe las categorías disponibles para filtrar artefactos (por tipo: Lakehouse, Notebook, Report, etc.). Esto demuestra que un solo workspace puede contener artefactos de todas las experiencias.

5. Explique a los participantes el **patrón de nomenclatura** que se usará en el curso:

   | Prefijo | Tipo de artefacto | Ejemplo |
   |---------|-------------------|---------|
   | `LH_` | Lakehouse | `LH_VentasCurso` |
   | `DF_` | Dataflow | `DF_LimpiezaVentas` |
   | `SM_` | Semantic Model | `SM_VentasCurso` |
   | `RPT_` | Reporte Power BI | `RPT_VentasCurso` |

**Resultado esperado:**

- Los participantes comprenden que un espacio de trabajo es un contenedor unificado que alberga artefactos de todas las experiencias de Fabric.
- Se entiende que la organización por nomenclatura (prefijos) facilita la gestión de proyectos.

**Verificación:**

- El menú "+ New" muestra opciones de múltiples experiencias, confirmando que el workspace tiene capacidad Fabric (no solo Power BI).
- Si el menú solo muestra opciones de Power BI (Report, Semantic model), la capacidad Trial no está correctamente asignada. Vuelva al Paso 4 y revise la configuración.

---

### Paso 6 — Revisar OneLake como Almacenamiento Unificado

**Objetivo:** Demostrar cómo OneLake centraliza el almacenamiento de todos los artefactos de Fabric, eliminando la necesidad de copias de datos entre servicios.

**Instrucciones:**

1. Para ilustrar el concepto de OneLake, primero cree un artefacto de ejemplo temporal. Dentro del workspace `FabricCurso-Demo`, haga clic en **"+ New"** y seleccione **"Lakehouse"**.

2. En el cuadro de diálogo, ingrese el nombre:

   ```
   LH_DemoOneLake
   ```

3. Haga clic en **"Create"**. Espere a que el Lakehouse se cree (10-30 segundos).

4. Una vez dentro del Lakehouse, observe la estructura que se muestra en el panel izquierdo (Explorer):

   ```
   LH_DemoOneLake
   ├── Tables/       ← tablas gestionadas (formato Delta)
   └── Files/        ← archivos no estructurados (cualquier formato)
   ```

5. Explique a los participantes:
   - Esta estructura reside físicamente en **OneLake**, el lago de datos unificado de Fabric.
   - OneLake es **único por tenant** (como OneDrive es único por usuario).
   - Cada workspace tiene su propia carpeta lógica dentro de OneLake.
   - Los datos almacenados aquí son accesibles desde **cualquier experiencia** de Fabric sin necesidad de copiarlos.
   - El formato de almacenamiento predeterminado es **Delta Lake (Parquet + log de transacciones)**.

6. En la barra de herramientas superior del Lakehouse, identifique los tres modos de acceso:
   - **Lakehouse** (vista de explorador de archivos y tablas)
   - **SQL analytics endpoint** (acceso SQL a las tablas del Lakehouse)
   - **Default semantic model** (modelo semántico para Power BI)

   > Esto demuestra que un mismo dato almacenado en OneLake puede ser consumido por la experiencia de Data Engineering (Spark), Data Warehouse (SQL) y Power BI (modelo semántico) **sin duplicación**.

7. Regrese al workspace haciendo clic en **"FabricCurso-Demo"** en la ruta de navegación (breadcrumb) superior.

8. Observe que en el workspace ahora aparecen tres artefactos generados automáticamente a partir del Lakehouse:

   | Artefacto | Tipo | Generación |
   |-----------|------|------------|
   | `LH_DemoOneLake` | Lakehouse | Creado manualmente |
   | `LH_DemoOneLake` | SQL analytics endpoint | Generado automáticamente |
   | `LH_DemoOneLake` | Default semantic model | Generado automáticamente |

9. **Elimine el Lakehouse de demostración** para mantener limpio el workspace:
   - Haga clic en los **tres puntos (…)** junto al artefacto `LH_DemoOneLake` (tipo Lakehouse).
   - Seleccione **"Delete"**.
   - Confirme la eliminación en el cuadro de diálogo.
   - Los artefactos asociados (SQL endpoint y semantic model) se eliminarán automáticamente.

**Resultado esperado:**

- Los participantes observan cómo un único artefacto (Lakehouse) genera automáticamente puntos de acceso para SQL y Power BI, demostrando la unificación de OneLake.
- Tras la eliminación, el workspace vuelve a estar vacío.

**Verificación:**

- Tras eliminar el Lakehouse, confirme que el workspace `FabricCurso-Demo` no contiene artefactos (muestra el mensaje de workspace vacío).
- Si los artefactos asociados no se eliminan automáticamente, elimínelos manualmente con el mismo procedimiento.

---

### Paso 7 — Explicar el Flujo General de Trabajo en Microsoft Fabric

**Objetivo:** Presentar visualmente el ciclo de vida del dato dentro de Microsoft Fabric, conectando las experiencias exploradas con un flujo de trabajo de extremo a extremo.

**Instrucciones:**

1. Utilizando la pizarra, una diapositiva preparada o dibujando en la pantalla, presente el siguiente flujo de trabajo que se ejecutará a lo largo del curso:

   ```
   ┌──────────────────────────────────────────────────────────────────────┐
   │                    FLUJO DE TRABAJO EN MICROSOFT FABRIC              │
   │                                                                      │
   │  ┌─────────────┐    ┌─────────────┐    ┌─────────────┐             │
   │  │   INGESTA   │───▶│TRANSFORMACIÓN│───▶│ALMACENAMIENTO│            │
   │  │Data Factory │    │Data Engineer.│    │  OneLake    │             │
   │  │(Dataflow    │    │(Notebooks   │    │  (Delta)    │             │
   │  │ Gen2)       │    │ Spark)      │    │             │             │
   │  └─────────────┘    └─────────────┘    └──────┬──────┘             │
   │                                               │                     │
   │                              ┌────────────────┼────────────────┐    │
   │                              ▼                ▼                ▼    │
   │                    ┌─────────────┐  ┌─────────────┐  ┌──────────┐  │
   │                    │   ANÁLISIS  │  │  CIENCIA DE │  │VISUALIZA-│  │
   │                    │    SQL      │  │    DATOS    │  │  CIÓN    │  │
   │                    │Data Warehouse│  │Data Science │  │Power BI  │  │
   │                    └─────────────┘  └─────────────┘  └──────────┘  │
   │                                                                      │
   └──────────────────────────────────────────────────────────────────────┘
   ```

2. Relacione cada etapa con lo que se hará en el curso:

   | Etapa | Experiencia de Fabric | Práctica del curso |
   |-------|----------------------|-------------------|
   | Ingesta | Data Factory (Dataflow Gen2) | Lab 02-00-01: Carga de `VentasEjemplo_2024.xlsx` |
   | Transformación | Data Engineering | Lab 02-00-01: Limpieza y preparación de datos |
   | Almacenamiento | OneLake (Lakehouse `LH_VentasCurso`) | Lab 02-00-01: Almacenamiento en tablas Delta |
   | Análisis | Data Warehouse / SQL Endpoint | Lab 02-00-01: Consultas SQL sobre datos limpios |
   | Visualización | Power BI | Lab 02-00-01: Creación de reporte `RPT_VentasCurso` |

3. Destaque los siguientes puntos clave:
   - **No hay copias de datos** entre etapas: todos los servicios acceden al mismo OneLake.
   - **Un solo modelo de seguridad** gobierna el acceso en todas las experiencias.
   - **Copilot** puede asistir en varias de estas etapas (generación de código, consultas, visualizaciones).
   - El **espacio de trabajo** `FabricCurso-Demo` creado hoy contendrá todos los artefactos del flujo completo.

4. Responda preguntas de los participantes sobre el flujo general.

**Resultado esperado:**

- Los participantes comprenden la secuencia lógica del trabajo con datos en Fabric.
- Queda clara la relación entre las experiencias exploradas en el Paso 3 y las etapas del flujo de trabajo.

**Verificación:**

- Solicite a 2-3 participantes que nombren qué experiencia de Fabric usarían para: (a) mover datos desde un archivo Excel, (b) consultar datos con SQL, (c) crear un dashboard. Las respuestas esperadas son: (a) Data Factory, (b) Data Warehouse/SQL Endpoint, (c) Power BI.

---

## 7. Validación y Pruebas Finales

Para confirmar que el laboratorio se completó exitosamente, verifique los siguientes criterios:

| # | Criterio de validación | Estado esperado |
|---|------------------------|-----------------|
| 1 | Inicio de sesión exitoso en https://app.fabric.microsoft.com | ✅ Portal accesible |
| 2 | Licencia Fabric Trial confirmada en la cuenta | ✅ Etiqueta "Trial" visible |
| 3 | Espacio de trabajo `FabricCurso-Demo` creado | ✅ Visible en lista de Workspaces |
| 4 | Capacidad Trial asignada al workspace | ✅ Confirmado en Workspace Settings > License info |
| 5 | Workspace vacío y listo para la Práctica 2 | ✅ Sin artefactos residuales |
| 6 | Selector de experiencias funcional | ✅ Las 6 experiencias son accesibles |

### Verificación rápida por el instructor:

1. Navegue a **Workspaces** en el panel lateral.
2. Busque `FabricCurso-Demo` en la lista.
3. Acceda al workspace y confirme que está vacío.
4. Abra **Workspace settings** > **License info** y verifique la capacidad Trial.

---

## 8. Solución de Problemas

### Problema 1: La licencia Fabric Trial no aparece activa

**Síntomas:**
- Al revisar la información de la cuenta, solo aparece *"Free"* o *"Pro"* sin mención de Fabric Trial.
- Al crear el workspace, no aparece la opción de seleccionar capacidad Trial en la sección Advanced.
- El menú "+ New" dentro del workspace solo muestra opciones de Power BI (Report, Semantic model).

**Causa:**
La prueba gratuita de Fabric no fue activada previamente para la cuenta del usuario, o el administrador del tenant no habilitó la opción correspondiente en el portal de administración.

**Solución:**

1. Navegue a https://app.fabric.microsoft.com.
2. Si aparece un banner o botón que dice **"Start trial"** o **"Iniciar prueba"**, haga clic en él y acepte los términos.
3. Si no aparece el banner:
   - El administrador del tenant debe acceder a **portal.powerbi.com** > **Admin Portal** > **Tenant Settings**.
   - Buscar la sección **"Microsoft Fabric"**.
   - Habilitar la opción **"Users can try Microsoft Fabric paid features"** = **Enabled**.
   - Esperar 5-15 minutos para que el cambio se propague.
4. Cierre sesión, borre la caché del navegador (`Ctrl + Shift + Delete`) y vuelva a iniciar sesión.
5. Intente activar la Trial nuevamente.

---

### Problema 2: El espacio de trabajo se crea pero no permite crear artefactos de Fabric

**Síntomas:**
- El workspace `FabricCurso-Demo` existe en la lista.
- Al hacer clic en "+ New", solo aparecen opciones básicas de Power BI (Report, Dashboard, Dataflow).
- No aparecen opciones como Lakehouse, Notebook, Warehouse, Pipeline.

**Causa:**
El workspace fue creado sin asignar la capacidad Fabric Trial (quedó en modo "Shared capacity" o "Pro"), lo que limita las funcionalidades disponibles exclusivamente a Power BI.

**Solución:**

1. Dentro del workspace `FabricCurso-Demo`, haga clic en **"Workspace settings"** (icono de engranaje ⚙️).
2. Navegue a la pestaña **"Premium"** o **"License info"**.
3. En el campo **"License mode"**, cambie de *"Pro"* o *"Shared"* a **"Trial"** o **"Fabric capacity"**.
4. En el campo **"Capacity"**, seleccione la capacidad Trial disponible del tenant.
5. Haga clic en **"Apply"** o **"Save"**.
6. Espere 10-20 segundos y recargue la página (`F5`).
7. Verifique que el menú "+ New" ahora muestre todas las opciones de Fabric (Lakehouse, Notebook, etc.).

Si la capacidad Trial no aparece en la lista desplegable, confirme con el administrador del tenant que la capacidad fue creada y que el usuario tiene permisos de *Contributor* o superior sobre ella.

---

## 9. Limpieza

Al finalizar esta práctica, el entorno debe quedar en el siguiente estado:

| Elemento | Estado |
|----------|--------|
| Workspace `FabricCurso-Demo` | **Conservar** — será reutilizado en Lab 02-00-01 |
| Lakehouse `LH_DemoOneLake` | **Eliminado** (se borró en el Paso 6) |
| Cualquier otro artefacto de prueba | Eliminar si fue creado accidentalmente |

**Acciones de limpieza:**

1. Navegue al workspace `FabricCurso-Demo`.
2. Si existe algún artefacto residual (creado por error durante la demostración), elimínelo:
   - Clic en los **tres puntos (…)** junto al artefacto.
   - Seleccione **"Delete"**.
   - Confirme la eliminación.
3. Confirme que el workspace queda completamente vacío.

> ⚠️ **No elimine el workspace `FabricCurso-Demo`.** Este será el contenedor principal para la Práctica 2 (Lab 02-00-01) donde se creará el Lakehouse `LH_VentasCurso` y los demás artefactos del curso.

---

## 10. Resumen

### Lo que se logró en esta práctica:

| Logro | Detalle |
|-------|---------|
| ✅ Acceso verificado | Inicio de sesión exitoso y licencia Trial confirmada |
| ✅ Interfaz explorada | Panel de inicio, menú lateral y selector de experiencias recorridos |
| ✅ Experiencias identificadas | 6 cargas de trabajo principales distinguidas y exploradas |
| ✅ Workspace creado | `FabricCurso-Demo` con capacidad Trial asignada |
| ✅ OneLake demostrado | Creación temporal de Lakehouse para mostrar almacenamiento unificado |
| ✅ Flujo de trabajo explicado | Ciclo completo desde ingesta hasta visualización presentado |

### Conceptos clave para recordar:

- **Microsoft Fabric** es una plataforma de analítica unificada que resuelve el problema de fragmentación de herramientas.
- **OneLake** es el lago de datos único por tenant; todos los servicios leen y escriben en él sin duplicar datos.
- Los **espacios de trabajo** son la unidad organizativa donde coexisten artefactos de todas las experiencias.
- El **selector de experiencias** permite cambiar de contexto entre las cargas de trabajo sin salir de la plataforma.
- Los datos se almacenan en **formatos abiertos** (Delta Lake / Parquet), evitando el bloqueo con un proveedor.

### Conexión con la siguiente práctica:

En el **Lab 02-00-01** (Práctica 2), se utilizará el workspace `FabricCurso-Demo` para:
1. Crear el Lakehouse `LH_VentasCurso`.
2. Cargar el archivo `VentasEjemplo_2024.xlsx` mediante Data Factory.
3. Transformar los datos y crear visualizaciones en Power BI.

### Recursos adicionales:

| Recurso | Enlace |
|---------|--------|
| Documentación oficial de Microsoft Fabric | https://learn.microsoft.com/es-es/fabric/get-started/microsoft-fabric-overview |
| ¿Qué es OneLake? | https://learn.microsoft.com/es-es/fabric/onelake/onelake-overview |
| Guía de inicio con Workspaces | https://learn.microsoft.com/es-es/fabric/get-started/workspaces |
| Ruta de aprendizaje: Introducción a Microsoft Fabric | https://learn.microsoft.com/es-es/training/paths/get-started-fabric/ |
| Licenciamiento y capacidades de Fabric | https://learn.microsoft.com/es-es/fabric/enterprise/licenses |

---
