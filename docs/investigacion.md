# Investigación de la problemática de ClipQuest

> **Base de trabajo en elaboración.** César, Ernesto y Sebastián desarrollarán principalmente este documento, con apoyo de Diego. Antes de entregar, sustituyan las indicaciones de esta base por su investigación y completen las tablas. La extensión sugerida es de 1,500 a 2,500 palabras.

**Alcance acordado:** creadores aficionados que editan sus propias grabaciones de Fortnite. La primera versión buscará escenas a partir de una descripción breve en inglés, dentro de videos de hasta cinco minutos, y devolverá marcas de tiempo y vistas previas para revisión del creador.

## El problema

Localizar escenas dentro de grabaciones es una tarea muy tediosa y en el caso de un gameplay de fortnite esta tarea puede llegar a ser aún más tediosa ya que como mínimo una partida puede durar 20 minutos y en un gameplay juegas más de una partida, esto afecta a los creadores de contenido ya que al momento de subirlo a youtube o alguna otra plataforma será difícil que la gente quiera ver un video tan largo, entonces, al editarlo le tomará mucho tiempo buscar las partes importantes en una grabación de 2 horas o más 
una prueba de esto son estos dos videos


<img width="671" height="229" alt="Captura de pantalla 2026-11-04 171846" src="https://github.com/user-attachments/assets/74116d36-d0b9-4039-a8ca-f21e0ee31691" />
<img width="940" height="222" alt="Captura de pantalla 2026-11-04 172031" src="https://github.com/user-attachments/assets/a0e63c70-4f89-495d-adee-eaf3f377e920" />

En la primera imagen se muestra un video publicado hace **3 días** de apenas **21:46 minutos** con un total de **1.3 millones de visitas**
mientras que en la segunda un video publicado hace **7 años**, mucho más tiempo que el primero tiene una duración de **1:46:59 horas** y un total de **1.5 millones de vistas**.

una diferencia pequeña tomando en cuenta la diferencia de antigüedad de ambos videos



Incluir **al menos dos datos verificables**, con su fecha, ámbito y enlace a la fuente. Integrarlos también en el texto y explicar qué aportan al análisis.

| Dato o indicador | Cifra y periodo | Población o ámbito | Relación con el problema | Fuente con enlace |
| --- | --- | --- | --- | --- |
|Tasa de retención promedio para videos de larga duración (más de 10-20 minutos). | Entre 30% y 45% de retención promedio (datos del sector de analítica de video y reportes de la plataforma al cierre de 2025/2026). |Creadores de contenido de formato largo y usuarios de YouTube a nivel global. | Demuestra que la gran mayoría de la audiencia abandona los videos largos antes de la mitad, lo que valida la dificultad de retener usuarios en gameplays extensos sin una buena estructuración o cortes clave. | [Estudio de Retención de Audiencia en YouTube - LeNosTube / Informes de Métricas 2026](https://www.lenostube.com/en/youtube-audience-retention-average-good-and-best-benchmarks/). |
| Volumen total de horas visualizadas en plataformas de contenido de videojuegos | **8.8 mil millones** de horas acumuladas durante el año. | Comunidad global de usuarios y creadores de contenido de videojuegos en YouTube. | Refleja la enorme saturación y competencia masiva que existe en la plataforma, donde los creadores necesitan destacar rápidamente y retener a la audiencia para no perder tracción frente a miles de millones de horas de contenido disponible. | [Reporte anual de métricas de YouTube Gaming - Tubefilter / Stream Hatchet](https://www.tubefilter.com/2026/03/05/youtube-gaming-hit-8-8-billion-hours-of-watchtime-in-2025/). |


## Cómo se resuelve hoy

Alternativas reales utilizadas actualmente

| Solución o alternativa | Qué ofrece | Limitaciones para este caso de uso |
| --- | --- | --- | --- |
| Revisión manual en línea de tiempo | Control absoluto fotograma a fotograma para encontrar momentos destacados de la partida. | Es extremadamente tardado y tedioso |
| Capítulos automáticos nativos | Segmentación automática basada en silencios o metadatos generales de la plataforma. | No entiende el contexto visual del juego |
| Herramientas de transcripción de voz (Speech-to-Text) | Búsqueda por palabras clave dichas por el creador en el audio del video. | Falla si el streamer pasa varios minutos concentrado sin hablar o si el momento clave solo tiene efectos de sonido y música. |



## Por qué con IA y por qué este modelo

Las alternativas actuales buscan por tiempo, por metadatos o por audio, pero ninguna entiende lo que se ve en pantalla. Un modelo como CLIP relaciona texto con imágenes, así que puede encontrar un fotograma a partir de una descripción, aunque el creador no estuviera hablando en ese momento.
| Modelo y enlace | Parámetros | Licencia y fuente | Ventajas para ClipQuest | Desventajas o límites |
| --- | --- | --- | --- | --- |
| [CLIP ViT-B/32](https://huggingface.co/openai/clip-vit-base-patch32) | Aproximadamente 151.28 millones | [MIT](https://github.com/openai/CLIP/blob/main/LICENSE) | Permite relacionar imágenes y descripciones de texto | Su rendimiento en Fortnite debe probarse; la Model Card describe dificultades con categorías muy específicas y conteo de objetos |
|[google/siglip-base-patch16-224](https://huggingface.co/google/siglip-base-patch16-224) | Alrededor de 86 millones | Apache 2.0 | Utiliza una función de pérdida basada en Sigmoid en lugar de Softmax, lo que le permite escalar de manera más eficiente en tareas de recuperación imagen-texto de cero disparo (zero-shot). | Es rápido pero requiere ajustar o alinear los prompts descriptivos al contexto visual específico de los videojuegos (como las mecánicas particulares de Fortnite) para maximizar su precisión al buscar escenas exactas. |


¿Qué hace el modelo?
- Toma las descripciones textuales ingresadas por el usuario y fotogramas extraidos
- Evalúa la cercanía matemática entre el texto de búsqueda y la imagen
  

¿Qué hace la App?
- Extraccion de fotogramas
- Registrar y almacenar tiempos
- Tomar resultados del modelo y ordenarlos
- Presentar resultados


## Usuarios y requisitos

Describir al usuario principal a partir del alcance acordado: una persona que graba sus partidas de Fortnite, edita su contenido y necesita localizar escenas para preparar clips. Explicar qué sabe hacer, qué información recuerda de la escena y qué espera recibir del sistema.

Requisitos ya acordados que deben desarrollar y justificar:

- Recibir una grabación propia o autorizada de Fortnite de hasta cinco minutos.
- Aceptar una descripción breve en inglés sobre contenido visible en la escena.
- Comparar la consulta con fotogramas del video y conservar sus marcas de tiempo.
- Mostrar momentos ordenados por similitud, acompañados de una vista previa y su tiempo.
- Permitir que el creador revise los resultados y seleccione las escenas que le sirven.
- Proteger las grabaciones y evitar la publicación de datos personales.

Completar los criterios de aceptación. Las metas de precisión, tiempo y cantidad de resultados deberán acordarse con el equipo y comprobarse con pruebas.

| Requisito | Cómo se comprobará | Criterio de aceptación acordado |
| --- | --- | --- |
| Búsqueda de una escena relacionada con la consulta | | |
| Presentación de vistas previas y marcas de tiempo | | |
| Utilidad frente a una revisión manual | | |
| Manejo de grabaciones y archivos temporales | | |

## Impacto ético

Analizar los siguientes aspectos, aplicándolos al caso de ClipQuest y respaldando con fuentes las afirmaciones sobre el modelo o la infraestructura:

- **Privacidad y permisos:** consentimiento para utilizar las grabaciones, conversaciones, nombres de usuario y otros datos visibles; conservación y eliminación de archivos.
- **Sesgos y límites del modelo:** elementos visuales o situaciones que CLIP podría representar peor y cómo se evaluarán en Fortnite.
- **Personas afectadas y responsabilidad:** consecuencias de un resultado equivocado, quién revisará las sugerencias y quién responderá por la publicación de un clip.
- **Costo energético:** relación entre la ejecución en Colab con GPU, la duración del procesamiento, el número de fotogramas y los recursos utilizados. Separar las estimaciones de las mediciones que se obtengan durante las pruebas.

| Riesgo | A quién podría afectar | Mitigación propuesta | Responsable | Cómo verificar la medida |
| --- | --- | --- | --- | --- |
| | | | | |
| | | | | |
| | | | | |

## Referencias

Reunir **al menos cinco fuentes confiables**, con enlace, y citarlas en los apartados donde se utilicen. Agregar las fuentes de los datos del problema, las alternativas existentes y el modelo de comparación. Las siguientes fuentes técnicas ya corresponden al modelo elegido:

1. OpenAI. [Model Card de openai/clip-vit-base-patch32](https://huggingface.co/openai/clip-vit-base-patch32).
2. Radford, A. et al. (2021). [Learning Transferable Visual Models From Natural Language Supervision](https://arxiv.org/abs/2103.00020).
3. OpenAI. [Licencia MIT del repositorio oficial de CLIP](https://github.com/openai/CLIP/blob/main/LICENSE).

Antes de la entrega, comprobar que cada cifra y comparación tenga una fuente, que las citas del texto correspondan a esta lista y que todos los enlaces funcionen. Esta base todavía no constituye la investigación terminada.
