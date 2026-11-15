# ☁️ Práctica 4.3-P5: Huella Energética de la Nube

Proyecto correspondiente a la **Unidad 4: Escenario económico** (**Tema 4.3: Externalización e internalización de costos**) de la materia **Desarrollo Sustentable (ACD-0908)** en el **Instituto Tecnológico de Mazatlán**. Consiste en una aplicación web conectada a un agente de Inteligencia Artificial mediante webhooks que estima el consumo eléctrico y las emisiones de gases de efecto invernadero ($\text{CO}_2\text{e}$) derivadas de los hábitos digitales diarios.

## 🎥 Video de Demostración

Haz clic en la vista previa o en el enlace directo para ver el funcionamiento de la práctica en YouTube:

* **Enlace directo:** [Ver Práctica 4.3 en YouTube](https://youtube.com/shorts/eOvjx-OMufs?feature=share)

[![Práctica 4.3: Huella Energética de la Nube](https://img.youtube.com/vi/eOvjx-OMufs/0.jpg)](https://youtube.com/shorts/eOvjx-OMufs?feature=share)

## ⚙️ Arquitectura y Flujo del Sistema

El sistema conecta una interfaz web estática alojada en GitHub Pages con un escenario de automatización e IA en Make:

$$
\text{Página web (GitHub Pages)} \rightarrow \text{Webhook de Make} \rightarrow \text{Make AI Agent} \rightarrow \text{Webhook Response} \rightarrow \text{Página web}
$$

### 1. Frontend (`index.html`)

Aplicación web de archivo único (HTML, CSS y JavaScript puro, sin librerías externas) que cumple con los siguientes requerimientos funcionales:

* Título descriptivo y explicación del propósito de la herramienta.
* **8 campos numéricos** (valor mínimo $0$) para capturar el uso digital diario o semanal.
* Botón **"Calcular mi huella"** que envía los datos vía `POST` utilizando `URLSearchParams`.
* Indicador de estado **"Calculando..."** con desactivación temporal del botón durante la petición.
* Contenedor de resultados con formato `white-space: pre-line` para respetar los saltos de línea del reporte.
* Manejo de errores con el mensaje: *"No se pudo calcular, intenta de nuevo."*
* Diseño *responsive* con paleta de colores enfocada en sustentabilidad.

### 2. Automatización e IA en Make (`Huella_Nube_Equipo#`)

El escenario configurado en Make (ID `7042308`, contenido en `Huella_Nube_Equipo#.blueprint.json`) opera de forma instantánea (*Immediately as data arrives*) con tres módulos:

| Elemento | Configuración |
| :--- | :--- |
| **Escenario** | `Huella_Nube_Equipo#` (ID `7042308`, zona `us2.make.com`) |
| **Módulo 1** | `Webhooks > Custom webhook` (`gateway:CustomWebHook`) |
| **Módulo 2** | `Make AI Agent` (`ai-local-agent:RunLocalAIAgent`, modelo `medium`) |
| **Módulo 3** | `Webhooks > Webhook Response` (`gateway:WebhookRespond`, Status `200`) |
| **Headers HTTP** | `Content-Type: text/plain; charset=utf-8` y `Access-Control-Allow-Origin: *` |
| **Criterio de cálculo** | Factor de emisión de $0.444\text{ kg CO}_2\text{e/kWh}$ (SEMARNAT-RENE) |

> 💡 *Nota: Para mostrar la captura de tu escenario de Make directamente en este README, sube la imagen al repositorio como `flujo-make-4-3.jpg` y quita el símbolo `>` de la siguiente línea:*
> `![Escenario Huella_Nube_Equipo# en Make](./flujo-make-4-3.jpg)`

## 🔬 Actividad 1: Investigación Previa y Factor de Emisión

Para calibrar y auditar las estimaciones del agente de IA, se tomaron como referencia dos estudios internacionales sobre el consumo energético del streaming de video:

| Fuente | Consumo de 1 hora de video | Emisiones | Alcance del sistema |
| :--- | :--- | :--- | :--- |
| **IEA (Kamiya, 2020)** | $0.077\text{ kWh/h}$ ($77\text{ Wh/h}$) | $36\text{ g CO}_2\text{e/h}$ (global) | Centros de datos $\sim 5\%$, redes $\sim 20\%$, dispositivos $\sim 75\%$ |
| **The Carbon Trust (2021)** | $0.186\text{ kWh/h}$ ($186\text{ Wh/h}$) | $55\text{ g CO}_2\text{e/h}$ (Europa) | Hogar $\sim 89\%$, redes $\sim 10\%$, centros de datos $\sim 1\%$ |

* **Rango de referencia del video:** De $0.077$ a $0.186\text{ kWh/h}$. La variación obedece a los límites del sistema (inclusión de enrutador y decodificador doméstico), modelos de asignación en redes, tipo de dispositivo, resolución y matriz eléctrica regional.
* **Factor de emisión nacional:** $0.444\text{ kg CO}_2\text{e}$ por $\text{kWh}$ ($0.444\text{ tCO}_2\text{e/MWh}$), correspondiente al Sistema Eléctrico Nacional según el Registro Nacional de Emisiones (**SEMARNAT-RENE**, Aviso del 29 de mayo de 2026).

## 📱 Actividad 2: Datos de Uso Digital

Valores base obtenidos mediante la herramienta *Bienestar digital* (los valores entre corchetes representan estimaciones iniciales a personalizar por cada integrante):

| Campo | Valor | Unidad | Origen |
| :--- | :--- | :--- | :--- |
| `video` | $0.4$ | $\text{h/día}$ | TikTok ($2\text{ h } 35\text{ min} \div 7$) |
| `musica` | `[1]` | $\text{h/día}$ | Estimado |
| `redes` | $0.8$ | $\text{h/día}$ | Social ($5\text{ h } 16\text{ min} \div 7$) |
| `videollamadas` | `[2]` | $\text{h/semana}$ | Estimado |
| `juegos` | $2.0$ | $\text{h/día}$ | Categoría Juegos ($2\text{ h } 1\text{ min}$) |
| `nube` | `[30]` | $\text{GB}$ | Estimado |
| `ia` | `[10]` | $\text{consultas/día}$ | Estimado |
| `correos` | `[5]` | $\text{al día}$ | Estimado |

## 📊 Resultados del Equipo y Verificación (Pasos 8 y 9)

### Tabla de Resultados por Integrante

| Integrante | $\text{kWh}$ por año | $\text{kg CO}_2\text{e}$ por año | Actividad que más consume |
| :--- | :--- | :--- | :--- |
| Julio César García | `[Completar]` | `[Completar]` | `[Completar]` |
| `[Integrante 2]` | `[Completar]` | `[Completar]` | `[Completar]` |
| `[Integrante 3]` | `[Completar]` | `[Completar]` | `[Completar]` |

### Verificación de Suposición de Video (Paso 9)

El agente de IA supuso `[X]` $\text{kWh}$ por hora de video. Al contrastar este dato con la literatura técnica ($0.077$ a $0.186\text{ kWh/h}$, con punto medio cercano a $0.13\text{ kWh/h}$), se verifica si el modelo mantuvo un criterio intermedio razonable o si requiere ajuste en las instrucciones del *System Prompt*, considerando que The Carbon Trust incluye periféricos domésticos en espera mientras la IEA utiliza una frontera de sistema más acotada.

## 📝 Respuestas del Reporte de Sustentabilidad Digital

### 1. ¿Por qué el consumo de energía de la nube es una externalidad? ¿Quién la paga realmente?

Es una **externalidad negativa** porque los centros de datos que sostienen el streaming de video, el almacenamiento en la nube y los modelos de inteligencia artificial demandan grandes cantidades de electricidad y agua de enfriamiento, pero ese costo ambiental no se refleja en el precio que paga el usuario final. En nuestra aplicación web, cada acción digital parece gratuita o cubierta por una tarifa plana, aunque el cálculo demuestra emisiones reales anuales.

Dado que gran parte de la electricidad proviene de fuentes no renovables, el factor del Sistema Eléctrico Nacional ($0.444\text{ kg CO}_2\text{e/kWh}$) convierte este consumo en emisiones de gases de efecto invernadero, sumado a la evaporación de agua dulce en torres de enfriamiento que provoca estrés hídrico local. **Quienes absorben este costo son terceros ajenos a la transacción:** las comunidades cercanas a los centros de datos, los ecosistemas y las generaciones futuras.

### 2. ¿Qué actividad digital genera más consumo en su equipo y por qué?

De acuerdo con los registros de *Bienestar digital*, las actividades con mayor tiempo de uso en el equipo son los **juegos en línea** (alrededor de $2\text{ horas}$ diarias) y las **redes sociales**, seguidas del consumo de **video** (`[completar con la actividad ganadora del Paso 8]`).

El impacto energético depende de las horas de uso, la resolución y, principalmente, del **dispositivo del usuario**. Según la IEA, cerca del $75\%$ de la energía del video ocurre en el dispositivo final (cifra que The Carbon Trust eleva al $89\%$ al incluir enrutadores y equipos en espera). Un televisor LED de $50\text{ pulgadas}$ consume hasta $100\text{ veces}$ más electricidad que un teléfono inteligente; por ello, el uso prolongado en pantallas grandes supera ampliamente el consumo propio de los centros de datos (que representa apenas entre $1\%$ y $5\%$ del total en video).

### 3. ¿Qué tan confiable es la estimación de la IA? ¿Qué suposición cambiaría más el resultado?

La estimación es **orientativa y no un valor exacto**, tal como se especificó en las instrucciones del agente. Durante la verificación se observó que las fuentes base de video difieren casi $2.4\text{ veces}$ ($0.077\text{ kWh/h}$ en IEA frente a $0.186\text{ kWh/h}$ en The Carbon Trust), lo que equivale a $34\text{ g}$ frente a $83\text{ g}$ de $\text{CO}_2\text{e}$ por hora bajo el factor mexicano de $0.444\text{ kg CO}_2\text{e/kWh}$.

La suposición que más alteraría el resultado es el **dispositivo de visualización** (por la diferencia de hasta $100\times$ entre un televisor y un smartphone), seguida de las horas de uso y la calidad de reproducción (SD, HD o 4K). Asimismo, los factores asignados a almacenamiento en la nube, consultas de IA y correos electrónicos cuentan con menor consenso bibliográfico, por lo que presentan mayor margen de incertidumbre.

### 4. ¿Cómo podría internalizarse este costo (impuestos, energía renovable en centros de datos, precios distintos)?

Para que el precio refleje el costo ecológico real existen diversos mecanismos económicos y regulatorios:

* **Impuestos al carbono y mercados de emisiones:** Gravar las emisiones de los centros de datos para incorporar el costo ambiental en la contabilidad del proveedor y en las tarifas finales.
* **Contratos de energía 100% renovable (PPA):** Reducir drásticamente el factor de emisión asociado (acercándolo a cero frente a los $0.444\text{ kg CO}_2\text{e/kWh}$ actuales).
* **Enfriamiento de circuito cerrado:** Obligar el uso de tecnologías sin evaporación de agua dulce para mitigar el estrés hídrico regional.
* **Esquemas tarifarios por consumo y límites por defecto:** Sustituir tarifas planas ilimitadas por cobros escalonados según ancho de banda/almacenamiento y establecer resoluciones estándar (por ejemplo, HD en lugar de 4K por defecto) para visibilizar el costo ante el usuario.

### 5. ¿Qué cambio en su uso digital están dispuestos a hacer?

A partir de los resultados obtenidos en la calculadora, el equipo se compromete a implementar las siguientes acciones:

1. Reducir la calidad de reproducción de video de 4K o Full HD a **720p** cuando no se requiera alta definición.
2. Priorizar el consumo de video en **teléfono móvil o laptop** en lugar de televisores de gran formato.
3. Desactivar la **reproducción automática** y evitar dejar música o videos corriendo en segundo plano.
4. **Descargar** contenido multimedia recurrente en lugar de transmitirlo repetidamente por streaming, y eliminar archivos y fotografías duplicadas en la nube.
5. Formular **consultas más precisas a la IA** para reducir peticiones innecesarias y apagar el enrutador y equipos en espera cuando no estén en uso.

## 📂 Evidencias del Proyecto

* [x] Escenario de automatización `Huella_Nube_Equipo#.blueprint.json` (3 módulos activos).
* [x] Video de demostración publicado en YouTube.
* [ ] Captura de *Bienestar digital* de cada integrante.
* [ ] Captura del escenario `Huella_Nube_Equipo#` y ejecución del webhook (`Run once`).
* [ ] Captura de la página publicada en GitHub Pages mostrando el reporte generado por la IA.

## 👥 Datos del Proyecto

* **Autor / Integrante:** Julio César García
* **Carrera:** Ingeniería en Sistemas Computacionales
* **Institución:** Instituto Tecnológico de Mazatlán
* **Asignatura:** Desarrollo Sustentable (`ACD-0908`)
* **Equipo:** `[Número de equipo]`
* **Otros integrantes:** `[Nombres de los integrantes]`
* **Profesor(a):** `[Nombre del docente]`
* **URL GitHub Pages:** `[Pegar enlace del sitio desplegado]`