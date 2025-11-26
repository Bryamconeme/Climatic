1. Introducción
Climatic es una innovadora aplicación móvil diseñada para transformar la manera en que las personas interactúan con el clima. Más que un simple pronóstico, Climatic ofrece una experiencia completa y personalizada al usuario, entregando datos meteorológicos en tiempo real con un alto nivel de detalle: temperatura actual, pronóstico extendido para los próximos 7 días, calidad del aire, humedad
Este informe detalla cada etapa exigida por la evaluación: problema, requisitos, diseño, arquitectura, implementación, pruebas, métricas, uso de IA y aprendizajes.

2. Problema y Contexto
El problema central detectado es que muchas personas deben decidir su vestimenta sin contar con información actualizada del clima, lo que genera:

*Incomodidad (frío/calor).
*Mal uso de las prendas (ropa muy gruesa o muy ligera).
*Exposición a lluvia, viento o luz solar.
*Pérdida de tiempo en la mañana al decidir qué usar.
*Variaciones constantes del clima.
*Falta de acceso rápido a información meteorológica.

2.1 Usuario objetivo
Para identificar fue necesario ver que tipo de usuario se ven más afectados por la incertidumbre climática y quiénes requieren información rápida para tomar decisiones. Climatic se enfoca en usuarios que dependen del clima para organizar su día con anticipación y optimizar su preparación diaria.

*Trabajadores que van al trabajo temprano.
*Personas que viven en ciudades con clima cambiante (como Temuco).
*Estudiantes universitarios.

2.3 Impacto esperado:
Es generar un beneficio directo en la rutina del usuario, mejorando su experiencia diaria y reduciendo errores derivados de la estimación subjetiva del clima. El impacto esperado combina comodidad, eficiencia y seguridad, permitiendo al usuario tomar decisiones informadas sobre su vestimenta.

*Aumentar el confort del usuario.
*Reducir errores de estimación climática.
*Mejorar la planificación diaria de vestuario.

3. Alcance y Requisitos del Proyecto
Para construir una solución coherente y funcional, fue necesario definir con claridad qué cubriría el sistema, qué funcionalidades ofrecería y bajo qué criterios técnicos debe operar. Este apartado establece los límites del proyecto y los requisitos que guían el diseño e implementación de Climatic.

3.1 Alcance
El alcance de nuestro proyecto es que el usuario pueda consultar el clima varias veces en el día, una recomendación automática y del outfit según temperatura y condiciones, pudiendo así consultar cualquier ciudad que el usuario quiera.
No contempla: registro de usuarios, preferencias de estilo, estadísticas históricas o alertas avanzadas. por el momento

3.2 Requerimientos funcionales principales
Los requerimientos funcionales definen las capacidades esenciales que Climatic debe ejecutar para entregar una solución útil y operativa. Cada funcionalidad está orientada a procesar información climática, interpretarla mediante reglas internas y presentar recomendaciones de vestimenta de manera clara y precisa. A continuación se detallan los requerimientos funcionales implementados.

RF1: Obtener el clima actual de la ciudad del usuario
*El sistema es capaz de recuperar datos meteorológicos que incluyan temperatura, estado del clima y humedad.
*En la versión Climatic, esta información se obtiene desde un archivo JSON local o datos simulados (mock) para asegurar disponibilidad.
*La consulta se activa al iniciar la aplicación o cuando el usuario solicita actualizar el clima.
*El sistema debe validar la estructura del archivo de datos antes de procesarlo.

RF2: Analizar temperatura, condición (lluvia, sol, viento) y humedad
*La aplicación debe interpretar los datos climáticos para determinar el contexto real del día.
*El módulo de lógica identifica rangos térmicos específicos (frío extremo, frío moderado, templado, cálido).
*Reconoce condiciones como lluvia, viento fuerte y cielo despejado para agregar recomendaciones adicionales.
*La humedad complementa la interpretación, permitiendo sugerencias más precisas (ej. ropa liviana en climas húmedos y calurosos).
*Todas estas variables se procesan en menos de 300 ms.

RF3: Generar recomendación automática de vestimenta:
*El sistema debe transformar los datos analizados en una recomendación completa para el usuario.
*Las recomendaciones se basan en reglas internas previamente definidas (ej.: temperatura < 10°C = abrigo grueso).
*Se agregan elementos adicionales según condiciones: impermeable, paraguas, cortaviento o bloqueador.
*La recomendación debe ser textual, clara y fácil de entender.
*La salida debe ser consistente y coherente al variar los datos climáticos.

RF4: Mostrar el clima y la recomendación en una interfaz simple
El sistema  presenta la información procesada en una pantalla principal ordenada y comprensible La interfaz incluye:

*Temperatura actual
*Descripción de la condición climática
*Recomendación de outfit

Los elementos se muestran en un diseño practico de usar, sin distracciones.
Se actualiza automáticamente cuando el clima cambia o el usuario solicita refrescar.

RF5: Permitir cambiar manualmente la ciudad
El usuario debe poder elegir otra ciudad para consultar su clima.

*Se incluye un campo de entrada o selector que permite ingresar el nombre de una ciudad.
*Al cambiar la ciudad, el sistema actualiza automáticamente los datos y la recomendación.
*Se valida que la ciudad exista en el archivo JSON o base de datos local

3.3 Requerimientos No Funcionales
 Los requerimientos no funcionales determinan atributos de calidad que afectan la experiencia del usuario, el rendimiento del sistema y su capacidad de mantenerse en el tiempo. Para Climatic, estos requisitos son fundamentales para asegurar que la aplicación sea rápida, fácil de usar, flexible de mejorar y accesible en diversos entornos. A continuación se detallan con mayor profundidad.
Rendimiento

El sistema debe proporcionar una respuesta rápida para garantizar fluidez y evitar frustraciones del usuario. En la versión actual, Climatic utiliza un archivo JSONs, lo que permite mantener tiempos de consulta inferiores a 1 segundo desde que el usuario solicita ver el clima.
 Además:
 
*El procesamiento de reglas de vestimenta debe ejecutarse en menos de 300 ms.
*La carga inicial de la interfaz no debe superar los 2 segundos en equipos estándar.
*No se deben realizar llamadas externas que puedan generar latencias inesperadas.

Usabilidad:
*La interfaz debe ser intuitiva, clara y minimalista, permitiendo que el usuario entienda la recomendación de vestimenta sin conocimientos técnicos ni pasos adicionales.
 Para cumplirlo:
*La pantalla principal debe mostrar de forma destacada temperatura, condición climática y outfit sugerido.
*Los textos deben ser breves, legibles y sin sobrecarga visual.
*Los colores y símbolos climáticos deben ser consistentes para reducir confusión.
*El usuario debe poder cambiar la ciudad en máximo dos interacciones (clicks o toques).
*Toda navegación debe ser lineal, sin menús complejos ni configuraciones avanzadas.

Mantenibilidad:
*El proyecto debe estar diseñado para permitir cambios, mejoras y extensiones sin afectar otras partes del sistema. Climatic adopta una arquitectura por capas y módulos independientes, lo que facilita modificaciones controladas.
 Aspectos específicos:
*La lógica de recomendación está separada en módulos propios, permitiendo añadir nuevos tipos de clima o reglas sin modificar la UI.
*La capa de datos es intercambiable gracias a un patrón Adapter, pudiendo conectarse a APIs reales en el futuro.
*El código está organizado en carpetas claras: /ui, /logic, /data.
*Se incorporan comentarios y nombres de funciones descriptivos para facilitar lectura y depuración
*Las dependencias externas se minimizan para evitar problemas de compatibilidad a largo plazo.

Portabilidad:
Climatic debe poder ejecutarse en distintos entornos, asegurando versatilidad y facilidad de uso por parte de cualquier evaluador.
 En la versión del proyecto se garantiza:
 
*Ejecución local tanto en Windows, Linux como macOS, siempre que el equipo cuente con Python o JavaScript.
*No requiere instalaciones complejas ni configuraciones especiales: basta con clonar el repositorio y ejecutar un archivo principal (main.py o index.html).
*Todo recurso utilizado (íconos, estilos, JSON) está embebido en el proyecto para evitar dependencias externas.
*El sistema no utiliza hardware específico, por lo que funciona correctamente en computadores estándar de laboratorio o personales.

3.4 Criterios de aceptación:
La aplicación genera una recomendación adecuada y lógica dependiendo del valor de temperatura ingresado o detectado.

*Mostrar recomendación coherente según el rango térmico.
*Mostrar condición climática (lluvia, nubes, sol, etc.).
*App funcional localmente sin errores.
*Resultado reproducible a partir de datos de clima.

4. Arquitectura y Diseño
   
4.1 Arquitectura General
La aplicación Climatic se construyó utilizando una arquitectura por capas, organizada en tres niveles principales que permiten una estructura modular, mantenible y escalable. Cada capa cumple funciones específicas y se comunica únicamente con la capa contigua, evitando dependencias innecesarias.

*Capa de Presentación (UI):
Corresponde a la interfaz gráfica o web con la que interactúa el usuario.
Muestra la información meteorológica y las recomendaciones de vestimenta.
Implementa un diseño minimalista, claro y centrado en la experiencia del usuario
Muestra información meteorológica de diferentes ciudades

*Capa de Lógica:
Procesa la información proveniente de la capa de datos.
Interpreta variables como temperatura, humedad y condición climática (lluvia, sol, viento).
Aplica reglas definidas (por ejemplo: “si llueve → recomendar chaqueta impermeable”).
Genera las recomendaciones automáticas de vestimenta basadas en criterios predefinidos.
Contiene toda la lógica que da sentido a la aplicación.

*Capa de Datos:
Responsable de obtener la información meteorológica.
Puede obtener datos desde una API real (como OpenWeather) o desde un archivo JSON mock para pruebas.
Normaliza y entrega los datos en un formato estándar para que la lógica pueda procesarlos.
Maneja posibles errores (falta de conexión, datos incompletos, ciudad no encontrada, etc.).
Esta arquitectura asegura que cada parte del sistema sea independiente, fácil de modificar y de probar, permitiendo agregar futuras funciones sin afectar el núcleo del proyecto.

4.2 Patrones Utilizados
Patrón MVC (Modelo–Vista–Controlador)
Se adoptó el patrón MVC para estructurar el sistema de manera clara y ordenada:

Modelo (Model):
*Gestiona los datos del clima.
*Representa objetos como Ciudad, ClimaActual o Recomendación.
*Se relaciona con la capa de datos y expone información a la lógica.

Vista (View):
*Corresponde a las pantallas o componentes gráficos
*Presenta el clima y las recomendaciones al usuario
*Presenta una interfaz fácil de usar
*Controlador principal:
*Recibe las acciones del usuario (cambiar ciudad, actualizar clima).
*Solicita la información a la capa de datos.
*Llama a la lógica para generar recomendaciones.
*Indica a la vista qué mostrar.

4.3 Decisiones arquitectónicas relevantes
Durante el desarrollo de Climatic, se tomaron varias decisiones estratégicas para garantizar la estabilidad del sistema, su mantenibilidad y la posibilidad de ampliarlo en el futuro sin reescribir grandes partes del código. Las principales decisiones fueron las siguientes:

1. Uso de datos locales (mock) para evitar dependencias externas inestables
En la fase de desarrollo y especialmente para la demostración del proyecto, se decidió trabajar con archivos JSON locales que simulan la respuesta de una API meteorológica real.

Esto evita problemas típicos de depender de servicios externos, como:

*Fallas de red o internet.
*Límite de consultas en APIs gratuitas.
*Cambios inesperados en el formato de la API.
*Tiempos de respuesta variables que afectarían la demo.

Esta decisión garantiza una prueba estable, controlada y reproducible, sin importar el entorno donde se ejecute la aplicación.

2. Mantener la lógica desacoplada para facilitar escalabilidad y extensiones futuras
Se diseñó la aplicación de manera que la lógica climática y las reglas de recomendación no dependieran de la interfaz gráfica.

Este desacoplamiento permite:
*Añadir nuevas funciones sin alterar la UI (ej: pronóstico extendido, recomendaciones por hora, modo oscuro).
*Probar cada módulo de forma independiente.
*Reutilizar la lógica en otros entornos (por ejemplo, una versión móvil o web).

4. Separación modular de las reglas de recomendación
Las reglas para generar recomendaciones de vestimenta se aislaron en módulos específicos, evitando que se mezclen con:

*Código de la interfaz.
*Obtención de datos.
*Controladores generales.
*Agregar recomendaciones según la hora del día.
*Incorporar alertas personalizadas (alto calor, frío extremo, tormentas).
*Ajustar sugerencias según el perfil del usuario (sensibilidad al frío/calor).

También facilita una futura incorporación de machine learning para hacer recomendaciones más inteligentes sin reestructurar la app.

4. Preparación para integración futura con servicios reales
Aunque se usaron datos mock, la arquitectura se diseñó con una capa de datos que puede cambiar su fuente sin afectar el resto del sistema.

 Esto permite que el proyecto escale hacia:
*APIs meteorológicas reales.
*Bases de datos internas si se agregan perfiles de usuario.
*Módulos de notificaciones o alertas por cambios climáticos.
*Para el desarrollo de Climatic se adoptó un enfoque centrado en la mantenibilidad, la claridad del código y la simplicidad arquitectónica, debido a que el proyecto corresponde a una aplicación pequeña, demostrativa y orientada a presentar un funcionamiento claro ante un jurado técnico. Este enfoque permitió crear un producto comprensible, modificable y fácil de extender sin incrementar la complejidad.

4.1 Código legible
Se priorizó el uso de:

*Funciones cortas y específicas
*Nombres descriptivos para clases, métodos y variables.
*Comentarios esenciales en zonas clave (nunca en exceso).
*Separación lógica por carpetas y módulos.

Objetivo: permitir que cualquier integrante o evaluador pueda entender la estructura del 

4.2 Arquitectura clara y modular
La aplicación sigue un diseño por capas (Presentación → Negocio → Datos), lo que evita que la lógica se mezcle con la interfaz o la obtención de datos.
 Esto facilita:
Reemplazar fácilmente la fuente de datos (mock → API real).
Modificar reglas de recomendación sin tocar la UI.
Agregar nuevas funciones sin romper el sistema.

Se eligió este diseño porque proporciona orden y profesionalismo sin agregar complejidad excesiva.

4.3 Baja complejidad
Dado el alcance del proyecto, se evitó agregar tecnologías, patrones o bibliotecas innecesarias.
Ejemplos:
 
*No se incluyeron frameworks complejos si la lógica podía resolverse con módulos simples.
*No se usó un motor de bases de datos ya que la información del clima es temporal y suficiente con mock JSON.
*No se añadió routing avanzado o navegación compleja en la UI.

4.4 Evitar sobre ingeniería innecesaria
No se implementaron soluciones excesivamente técnicas para un problema sencillo.
Por ejemplo:

*No se incorporaron microservicios ni contenedores.
*No se añadió un sistema de caché distribuido porque un caché local es suficiente.
*No se crearon patrones complejos como Clean Architecture o DDD, ya que exceden el alcance del proyecto.
*No se utilizaron modelos de machine learning porque la recomendación de ropa se resuelve con reglas determinísticas simples.
Esto ayudó a:

*Mantener el código liviano.
*Reducir el tiempo de desarrollo.
*Mejorar el rendimiento final.

4.5 Resumen del Enfoque
El proyecto Climatic está diseñado para ser:

*Fácil de entender
*Fácil de modificar
*Fácil de escalar en el futuro

5. Implementación y Lógica
5.1 Tecnologías utilizadas
(Según versión del proyecto Climatic que construiste)

Lenguaje principal: Java / JavaScript
Framework UI: HTML-CSS
Gestión de datos: JSON local
Control de versiones: Git + GitHub

5.2 Aspectos críticos implementados
Conversión de datos climáticos a reglas de vestimenta:

*Si temperatura < 10°C → abrigo grueso, bufanda
10°C–18°C → polerón, chaqueta ligera.
19°C–25°C → ropa ligera
25°C → ropa fresca, hidratación

Identificación de condiciones meteorológicas:

*Lluvia → impermeable/paragua
*Viento fuerte → cortaviento.
*Soleado → lentes y bloqueador.

5.3 Integración de componentes
El sistema inicia en la Capa de Datos, donde se obtiene la información meteorológica necesaria para generar la recomendación.
Este módulo puede obtener los datos desde:

*Un archivo JSON local (modo demostración).
*Una API real (futuras versiones).

Los datos proporcionados incluyen:

Temperatura actual (°C)
Condición general (lluvia, sol, nublado, viento)
Humedad (si se utiliza)

5.4 La Lógica del proyecto es analizar los datos y generar una recomendación
Una vez obtenidos los datos meteorológicos, estos son enviados a la Capa logica  donde se produce el proceso crítico del sistema.
La lógica realiza tres operaciones:
Clasifica la temperatura dentro de un rango, por ejemplo:

<10°C = frío extremo
10–18°C = frío moderado
19–25°C = templado
25°C = caluroso

Evalúa la condición climática, aplicando reglas específicas:

lluvia → impermeable
sol intenso → bloqueador / lentes
viento → cortaviento
nublado → ropa neutra

Genera una recomendación combinada, por ejemplo:

*Chaqueta ligera → impermeable → pantalón cómodo.*
Ropa fresca y lentes de sol.
*Este módulo actúa como el cerebro del sistema, aplicando reglas estrictas, mantenibles y almacenadas en funciones o estructuras separadas.
Demostración Funcional
*La demostración funcional tiene como objetivo evidenciar que Climatic no solo está correctamente implementado, sino que además cumple con los requerimientos funcionales definidos. Durante la presentación se mostrará el flujo completo del sistema, desde la carga de datos hasta la generación de la recomendación final.

6.1 Pantalla inicial con clima actual
La presentación comenzará mostrando la pantalla principal, donde el usuario puede visualizar de forma inmediata la información meteorológica actual.
La interfaz incluye:

*Temperatura actual (número en °C).
*Iconografía del clima (sol, nube, lluvia, viento, etc.).
*Condición del día (“Soleado”, “Lluvioso”, “Nublado”, etc.).
*Ciudad actual (por defecto: Temuco / o la ciudad configurada en el JSON). 

Esta pantalla permite validar que el módulo de datos se cargó correctamente y que la información fue interpretada por la capa de presentación.

6.2 Salida del sistema (Resultados mostrados)
Luego se mostrará la salida generada automáticamente por el sistema: una recomendación completa basada en la temperatura y la condición del día.

La salida incluye tres elementos clave:
1. Temperatura
Se muestra de forma clara, por ejemplo:
 “Temperatura actual: 13°C” y dira el outfit del dia
2. Condición del día
El sistema indica el clima:
Lluvia moderada
Nublado
Soleado
Viento fuerte
Esto se usa internamente para ajustar la recomendación de vestimenta.

4. Recomendación completa de outfit
La lógica de negocio produce un mensaje que integra todos los análisis, por ejemplo:

Usa una chaqueta impermeable, polerón liviano y pantalones cómodos.
Ropa fresca, camiseta de algodón y lentes de sol.
Abrigo grueso, bufanda y guantes recomendados.

6.3 Validación de criterios funcionales

Durante la demostración se comprobará que la aplicación responde correctamente a cambios en los datos de clima, validando así su comportamiento dinámico.
1. Coherencia entre temperatura y vestimenta aparte se revisará que:

Temperaturas bajas → ropa abrigada.
Temperaturas medias → ropa ligera/moderada.
Temperaturas altas → ropa fresca.

3. Cambio de clima produce cambio de recomendación
   
Se modificará la condición (lluvia, sol, nublado) y se verificará:
lluvia → se agrega impermeable
viento fuerte → se agrega cortaviento
sol intenso → se agrega protección solar


Esta prueba válida que la lógica de negocio interpreta correctamente los distintos estados climáticos.

6.4 Flujo completo de la demostración

*Se inicia la app → carga de datos desde JSON.
*Se muestra la pantalla inicial con clima actual.
*El sistema genera y muestra la recomendación.
*Se modifica la temperatura y se vuelve a ejecutar → nueva recomendación.
*Se modifica la condición del clima y se vuelve a ejecutar → nueva recomendación acorde.

6.5 Resultado esperado

El público puede ver cómo la app responde inmediatamente a cambios en los datos, aparte de ver el clima en una aplicacion comoda y facil de usar

7. Despliegue y Entorno:
El despliegue de Climatic se realizó considerando simplicidad, compatibilidad A continuación se detalla el entorno, los requisitos y los pasos necesarios para ejecutar correctamente el sistema en una máquina local.

7.1 Entorno de ejecución
Climatic está diseñado para ejecutarse de forma local, sin necesidad de conexión a Internet ni servidores externos, 
El entorno utilizado fue:

Sistema operativo: Windows 10/11
Ejecución local: Permite probar y evaluar el sistema sin depender de un backend en línea.
Arquitectura: Aplicación ejecutada directamente desde Python/JavaScript según versión implementada (CLI, GUI o Web local).
 
7.2 Dependencias del proyecto
Climatic requiere un conjunto mínimo de dependencias para funcionar:

*Node.js + npm
Librerías básicas del entorno (React)
Servidor local de desarrollo o uso directo de HTML/JS

7.3 Pruebas realizadas
Se desarrollaron pruebas unitarias enfocadas exclusivamente en la función que genera la recomendación de vestimenta y vision clara del clima y sus derivados
Estas pruebas verificaron:

*Que cada rango de temperatura produzca la sugerencia esperada.
*Que condiciones climáticas especiales (lluvia, viento, sol intenso) modifiquen correctamente la recomendación base de datos.
*Manejo adecuado de valores extremos o atípicos.

Se realizaron pruebas manuales para validar la experiencia completa desde el punto de vista del usuario:

*Cargar la aplicación.
Obtener el clima desde el archivo JSON 
Procesar datos de temperatura y condición.
Mostrar la recomendación final en pantalla.

Con estas pruebas se evaluó:
Que la aplicación iniciará sin errores.
Que el clima se cargará correctamente.
Que la recomendación fuera coherente con los datos mostrados
Que la interfaz mostrará toda la información sin fallas visuales*

Nos indico lo siguiente

<5°C: ropa térmica + abrigo grueso.
6°C–15°C: chaqueta ligera o cortaviento.
16°C–24°C: vestimenta casual y ligera.
≥25°C: ropa fresca o deportiva.

Además, se verificó que:
Lluvia agregara impermeable o paraguas.
Día soleado agregara lentes de sol.
Viento fuerte agregara cortaviento.

8.2 Métricas aplicadas
Para evaluar la calidad y rendimiento del sistema se definieron métricas específicas:
Tiempo de respuesta del módulo
Se midió el tiempo requerido para:

*Leer datos del clima.
*Procesarlos.
*Generar una recomendación.
*Mediciones por debajo de 1 segundo de respuesta

Conteo de Errores
Se revisaron posibles errores en:

*Lectura del JSON.
*Interpretación de datos climáticos.
*Ejecución de reglas.
*Renderizado de la vista.

Resultado:
 0 errores detectados durante pruebas estándar.

Estructuras condicionales.
Lógica de combinación entre temperatura y condición.

8.3 Mantenimiento
El sistema fue diseñado pensando en la facilidad de expansión futura y la sostenibilidad del código.
Código modular y extensible

*La aplicación se construyó con módulos separados:
*Módulo de clima: obtiene datos (mock o API futura).
*Módulo de recomendación: analiza el clima y crea la sugerencia.
*Módulo de interfaz: muestra resultados al usuario.

Esto permite:
*Añadir nuevos tipos de ropa sin modificar la interfaz.
*Cambiar o extender las reglas sin afectar otros componentes.
*Incluir nuevas condiciones climáticas (nieve, tormenta eléctrica, granizo).
*Facilidad para integrar API real en el futuro
*La arquitectura permite reemplazar fácilmente el origen del clima:
*Actualmente: JSON local
Futuro: API real como OpenWeather.

Para integrar solo es necesario sustituir el módulo de lectura de datos, sin tocar la lógica de recomendación ni la vista

9. Uso de Inteligencia Artificial
Se incorporaron herramientas de Inteligencia Artificial de manera responsable y documentada. La IA se utilizó como un apoyo complementario, principalmente para acelerar tareas y objetivos especificos.

9.1 Modelos utilizados
Se emplearon distintos modelos de IA en fases específicas del proyecto:

ChatGPT (GPT-5.1 y GPT-4o)
Utilizado para análisis de requisitos, generación de ideas de diseño, redacción de documentación técnica y asistencia en la creación de estructuras de código.
Se seleccionó por su capacidad para entender contexto técnico y producir contenido consistente y detallado.

GitHub Copilot (si aplica)
Utilizado principalmente dentro del editor para sugerencias de código simples o para completar estructuras repetitivas.
Funcionó como apoyo al escribir funciones básicas o plantillas iniciales.

9.2 Fases donde se utilizó IA
La IA se aplicó en puntos estratégicos para optimizar tiempo y mejorar claridad sin comprometer la calidad del proyecto.

Análisis de requisitos
Se consultó a la IA para verificar claridad, redundancia y completitud de los requisitos funcionales y no funcionales.

Diseño de diagramas UML
Se utilizaron descripciones generadas por IA para estructurar casos de uso, diagramas por capas y relaciones entre módulos.

Documentación del repositorio
Se generamos plantillas para README, explicaciones de arquitectura y descripciones de módulos.

9.3 Prompts representativos
A continuación se muestran ejemplos reales de instrucciones utilizadas durante el desarrollo:

*Genera reglas de recomendación de outfit según temperatura y condiciones meteorológicas.
*Optimiza esta función para mejorar la legibilidad y separar lógica de la interfaz.
*Explica la relación entre módulos en una arquitectura MVC.
*Redacta un apartado técnico para el informe del proyecto Climatic.”
E*stos prompts fueron ajustados para obtener respuestas precisas y técnicamente coherentes.

9.4 Validación humana
Cada resultado generado por IA pasó por un proceso de revisión humana obligatorio en el cual vimos lo siguiente

*Revisión manual de código: Se verificó que las funciones fueran correctas, eficientes y comprensibles.
*Correcciones de lógica :Se ajustaron recomendaciones, reglas y diagramas que contenían supuestos incorrectos.
*Adaptación a estándares del proyecto: Se adecuaron nombres de funciones, estilo de documentación y estructura de carpetas.

La IA apoyó el proyecto, pero la decisión final siempre la tomo el desarrollador

9.5 Consideraciones éticas
El uso de IA se realizó siguiendo criterios éticos y académicos:

*Uso responsable: La IA no se empleó para generar código completo sin entendimiento del funcionamiento; solo actuó como herramienta de apoyo.
*Transparencia: Se dejó explícito en la documentación del proyecto que se utilizó IA en distintas etapas, tal como exige la rúbrica institucional.
*No reemplazo del trabajo humano: Las decisiones técnicas, el diseño final y la implementación fueron realizados por el equipo desarrollador, evitando depender completamente de la IA.
*Cuidado con datos y privacidad:No se proporcionó a la IA información sensible ni credenciales de acceso.

10. Aprendizajes, Limitaciones y Próximos Pasos

Aprendizajes:
Separación de capas lógicas (Clean Architecture básico), Entendimos la importancia de dividir el sistema en capas independientes: obtención de datos, procesamiento de reglas y presentación.
Esto permite que cambiar una parte (por ejemplo, la fuente de datos del clima) no afecte a toda la aplicación, también mejora la mantenibilidad y la escalabilidad del proyecto.

10.1 Construcción de reglas claras y mantenibles

Se aprendió a diseñar reglas térmicas coherentes para generar recomendaciones según rangos de temperatura.
La lógica se estructuró pensando en evitar condiciones duplicadas o incoherentes.

Esto facilita futuras ampliaciones, como agregar recomendaciones personalizadas u otros factores climáticos.
Manejo profesional del repositorio

*Se trabajó con un repositorio estructurado, incluyendo carpetas claras, commits ordenados y mensajes explicativos.
*Se aprendió a mantener orden en el proyecto, lo que facilita el trabajo en equipo y la revisión del código.
*Integración de IA de forma ética y controlada
*Se comprendió cómo usar IA como herramienta de apoyo, sin reemplazar la lógica principal del proyecto.
*La IA fue utilizada para generar documentación, mejorar la redacción y apoyar el análisis.

Se mantuvo siempre el control humano en las decisiones técnicas.

10.2 Validación del funcionamiento local

Se confirmó por pruebas que la app funciona correctamente en entorno local sin depender de Internet.
Se aprendió a realizar test rápidos de valores para garantizar la coherencia del resultado.

10.3 Limitaciones actuales

*Dependencia de datos climáticos locales
*El proyecto no utiliza aún una API real, por lo que los datos deben ingresarse manualmente.
*Esto reduce la precisión y el realismo respecto a una aplicación final en producción.

10.4 Recomendaciones genéricas

*Las sugerencias entregadas por la app no consideran preferencias personales del usuario.
*No se incluyen factores como estilo de vida, sensibilidad al frío/calor, ni historial de uso.
*Ausencia de historial o configuración del usuario

La aplicación no guarda registros de consultas, gustos, parámetros ni ajustes.

10.5 Cobertura limitada de condiciones ambientales
Actualmente se consideran principalmente temperatura y condición visible (sol, nubes, lluvia).
Nse evalúan variables como viento, humedad o sensación térmica real.
La UI es funcional, pero simple.

No incluye animaciones, iconografía avanzada ni un diseño profesional completo aún.

10.6 Próximos Pasos

*Integrar una API meteorológica real (OpenWeather o WeatherAPI)
*Permitir que la aplicación obtenga datos en tiempo real.
*Aumentar precisión, variedad de condiciones y valores térmicos reales.
*Guardar consultas realizadas.
*Optimizar la interfaz gráfica

11. Conclusion
    En conclusión, nuestro proyecto Climatic demuestra cómo una aplicación sencilla puede ayudar a las personas en su día a día. A través de datos básicos del clima, la app entrega recomendaciones claras sobre cómo vestirse o qué esperar según la temperatura y las condiciones del ambiente.



