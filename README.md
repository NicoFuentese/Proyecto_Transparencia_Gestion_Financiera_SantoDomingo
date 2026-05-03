# Portal de Transparencia

**Integrantes:**
- Ian Guerrero
- Eva Ponce
- Nicolás Fuentes
- Juan Geraldo

**Tema:**
Tema 33: Portal de Transparencia para la Municipalidad de Santo Domingo.

# Cómo ejecutar el proyecto

### Instalar dependencias
  Run `npm install` to install the dependencies.

### Correr el proyecto
  Run `npm run dev` to start the development server.


---

## Introducción

Aunque la Municipalidad de Santo Domingo cumple con la Ley 20.285 sobre Acceso a la Información Pública mediante la publicación mensual de sus finanzas, el formato de estos datos presenta una barrera de usabilidad. Durante el levantamiento de requerimientos, se constató que la información disponible en el Portal de Transparencia del Estado se entrega en archivos CSV y XLS estáticos y segmentados. En consecuencia, extraer métricas elementales sobre el gasto público exige descargar, cruzar y procesar múltiples documentos, un procedimiento que requiere conocimientos técnicos y contables que dificultan el acceso práctico a la información.

Para abordar esta limitación, identificada por el municipio como "Falta de transparencia en la gestión financiera", este proyecto detalla el diseño e implementación de un portal ciudadano multiplataforma. La solución técnica se centra en procesar, reorganizar y visualizar de manera dinámica los datos financieros crudos, reduciendo la fricción en el acceso a la información y permitiendo una evaluación clara del estado presupuestario municipal por parte de usuarios sin formación técnica.

El presente repositorio corresponde a la Entrega Parcial 1 de la asignatura Ingeniería Web y Móvil (ICI 4247), y establece los cimientos analíticos y arquitectónicos del sistema. Para mantener la coherencia del relato técnico, la exposición de los contenidos adopta una progresión lógica que va desde la abstracción del problema hasta su implementación tecnológica, omitiendo el orden numérico estricto de la rúbrica de evaluación.

De este modo, el informe detalla inicialmente la justificación del problema y el usuario objetivo, seguido de la especificación de requerimientos funcionales y no funcionales y la arquitectura de navegación y experiencia de usuario.

## Justificación

### Contexto del problema.

Chile cuenta con un marco normativo robusto en materia de acceso a la información pública. El artículo 8º de la Constitución Política de la República consagra el principio de probidad y publicidad de los actos de los órganos del Estado, y la Ley Nº 20.285 sobre Acceso a la Información Pública (vigente desde abril de 2009) operacionaliza dicho principio mediante dos mecanismos complementarios: la transparencia activa, que obliga a los organismos públicos a publicar mensualmente en sus sitios electrónicos información relativa a su estructura, personal, remuneraciones, compras, transferencias, presupuesto, auditorías y subsidios, entre otras categorías; y la transparencia pasiva, que reconoce a toda persona el derecho de solicitar información a cualquier órgano de la Administración del Estado, debiendo este responder dentro de un plazo legal de 20 días hábiles, prorrogable excepcionalmente por 10 días adicionales (Ley N° 20.285, 2008). Las municipalidades se encuentran expresamente sujetas a estas obligaciones, bajo la fiscalización del Consejo para la Transparencia (CPLT).

En el plano teórico, el derecho de acceso a la información ha sido conceptualizado por el CPLT como un "derecho llave", es decir, un derecho instrumental cuyo ejercicio habilita el acceso a otros derechos fundamentales, tales como salud, vivienda, educación o un medio ambiente libre de contaminación (CPLT, 2018). Esta noción, respaldada por el fallo de la Corte Interamericana de Derechos Humanos en el caso *Claude Reyes vs. Chile* (CIDH, 2006) y por el Objetivo de Desarrollo Sostenible 16 de Naciones Unidas (ONU, 2015), reposiciona el DAI no como un trámite administrativo sino como un instrumento de empoderamiento ciudadano y de control social del Estado.

Sin embargo, la existencia de un marco legal robusto y de un portal de transparencia operativo no garantiza por sí sola el ejercicio efectivo del derecho. La evidencia empírica reciente muestra una brecha sistemática entre la información disponible y la información comprensible:

* Según el Estudio Nacional de Transparencia 2020, un 80 % de la ciudadanía percibe el acceso a la información pública como difícil y un 81 % como lento, pese a que un 85 % la considera necesaria (CPLT, 2021).
* El Informe de Fiscalización 2024 del CPLT, aplicado a las 345 municipalidades del país, reportó un cumplimiento promedio de 78,1 % en transparencia activa, con una caída de 1,2 puntos respecto al período anterior y casos críticos como la Municipalidad de María Elena (2,82 %) o Calama (46,06 %); apenas cuatro municipios alcanzaron el 100 % (CPLT, 2025).
* A nivel histórico, en sus primeros diez años de vigencia la Ley 20.285 generó cerca de 840.000 solicitudes de acceso a información y más de 26.000 amparos ante el CPLT, evidenciando que la vía pasiva sigue siendo necesaria precisamente porque la información publicada no resulta suficiente o digerible (CPLT, 2018).

A esta brecha estructural se suma una brecha de equidad documentada por el propio CPLT: quienes solicitan información son mayoritariamente mujeres de nivel socioeconómico medio-bajo, mientras quienes logran reclamar formalmente cuando la información les es denegada son hombres con educación universitaria de nivel socioeconómico alto, configurando una asimetría que "podría reforzar las exclusiones existentes" (Nash, Rodríguez y Chacón, 2016, citado en CPLT, 2018).

El caso particular de Santo Domingo se inscribe en este patrón. Durante la reunión de levantamiento, el representante municipal precisó que la información financiera se carga manualmente cada mes en el Portal de Transparencia mediante archivos CSV y XLS, organizados por categoría (remuneraciones, ejecución presupuestaria, transferencias, auditorías, etc.). Esta arquitectura, válida desde el punto de vista del cumplimiento legal, traslada al ciudadano la totalidad de la carga de interpretación: para responder preguntas longitudinales o agregadas debe descargar y consolidar múltiples archivos, lo que en la práctica disuade el ejercicio del derecho y termina, paradójicamente, generando solicitudes formales por la vía de transparencia pasiva que consumen tiempo del personal municipal (J. P. Vidal, comunicación personal, 15 de abril de 2026). Los datos del CPLT confirman la urgencia: las áreas donde la ciudadanía percibe mayor necesidad de acceso a la información son salud (82 %), educación (74 %) y vivienda (60 %) (CPLT, 2021), todas ellas reguladas o prestadas en buena parte por los municipios.

El problema, por tanto, no es la ausencia de información, sino su inaccesibilidad práctica: información publicada en formatos crudos, fragmentada temporal y temáticamente, sin contextualización, sin lenguaje ciudadano y sin posibilidad de manipulación dinámica. La consecuencia es la erosión del control social y de la rendición de cuentas que la propia Ley 20.285 buscaba habilitar.

### Pertinencia de una solución web y móvil

La elección de una plataforma web responsive con enfoque *mobile-first* se justifica por tres razones.

Primero, continuidad con el canal natural del derecho: la transparencia activa se ejerce, por mandato legal, a través de sitios electrónicos (Ley 20.285, Art. 7º). Una solución web no genera fricción con los hábitos de consulta ya establecidos, sino que enriquece la capa de presentación de información que ya vive en internet.

Segundo, prevalencia móvil: en Chile, la mayoría de los accesos ciudadanos a sitios públicos ocurre desde dispositivos móviles, por lo que el diseño debe priorizar pantallas pequeñas (>= 320 px) sin sacrificar la experiencia en escritorio. Esto se refleja en el RNF de diseño *responsive mobile-first* especificado en el numeral 3.

Por último, sostenibilidad operativa: el modelo propuesto se construye sobre los archivos CSV que el municipio ya produce y publica, sin demandar trabajo adicional al equipo de transparencia más allá de la carga mensual existente. Esto fue una restricción explícita de la contraparte municipal y se traduce en el RF de carga y procesamiento automatizado de archivos CSV (numeral 5 del listado de requerimientos funcionales).

### Usuario objetivo

La plataforma contempla dos roles diferenciados, alineados con el RF de autenticación por roles.

**Rol primario — Ciudadano (acceso público, sin registro).** Vecinos de Santo Domingo, periodistas locales y de medios regionales, dirigentes vecinales, organizaciones de la sociedad civil, estudiantes e investigadores que requieren consultar la gestión financiera del municipio. Se trata de un usuario heterogéneo en edad y formación, mayoritariamente sin conocimientos técnicos contables o presupuestarios, que accede de forma intermitente —típicamente ante una duda puntual o un evento que activa el interés (cuenta pública, controversia local, postulación a beneficios)— y desde dispositivos móviles. Sus necesidades centrales son: (i) responder preguntas concretas sin necesidad de recorrer todo el portal, (ii) comparar períodos, (iii) entender cifras agregadas en lenguaje cotidiano, y (iv) descargar evidencia para reutilizarla.

**Rol secundario — Administrador Municipal (acceso autenticado).** Funcionario o funcionaria de la unidad de transparencia o del área de comunicaciones del municipio, responsable de la carga mensual de los archivos CSV correspondientes a cada categoría obligatoria. Su perfil es el de un usuario administrativo recurrente con conocimiento del dominio normativo pero sin competencias avanzadas de desarrollo. Sus necesidades son operativas: una interfaz protegida con autenticación robusta (JWT, según el RNF de seguridad), procesos de carga simples sin edición de código, validaciones automáticas que prevengan errores de formato y trazabilidad de las cargas realizadas.

Este proyecto se justifica como una respuesta concreta a una brecha documentada entre el cumplimiento formal de la Ley de Transparencia y el ejercicio efectivo del derecho de acceso a la información pública, alineándose con la noción del DAI como derecho llave e instrumentalizándola al nivel local mediante una plataforma que prioriza la comprensibilidad, la equidad de acceso y la sostenibilidad operativa.

---

## Requerimientos del sistema

A partir del problema documentado en la Sección 2 y de los dos perfiles de usuario identificados, esta sección operacionaliza la propuesta de solución mediante la especificación de los requerimientos funcionales (RF) y no funcionales (RNF) que el sistema debe satisfacer. Los requerimientos se desprenden directamente de las necesidades expuestas: el ciudadano necesita información comprensible, filtrable y descargable; el administrador municipal necesita un mecanismo de carga sostenible y seguro; ambos comparten la exigencia de que la plataforma sea accesible, responsiva y respete los lineamientos del Framework Digital del Gobierno de Chile, alineado con los estándares WCAG 2.1.

Cada requerimiento se identifica mediante un código único (`RF-XX` o `RNF-XX`), un actor responsable, una descripción funcional y un criterio de aceptación medible que permitirá su verificación durante las entregas posteriores.

### Roles del sistema

La plataforma define dos roles con permisos diferenciados, conforme a lo enunciado en la sección siguiente:

* **Ciudadano.** Usuario público con acceso de solo lectura. No requiere registro ni autenticación. Puede consultar todas las vistas de datos, aplicar filtros temporales y descargar información en los formatos disponibles.
* **Administrador Municipal.** Funcionario autenticado mediante JWT. Hereda los permisos del Ciudadano y adicionalmente puede cargar archivos CSV mensuales, revisar el historial de cargas y gestionar metadatos asociados a las categorías obligatorias por ley.

Los requerimientos cuyo actor es exclusivamente el Administrador (`RF-10`, `RF-11`) se ejecutan tras autenticación; los demás están disponibles públicamente sin barreras de acceso.

## Requerimientos funcionales

| ID | Nombre | Actor | Descripción | Criterio de aceptación |
| :--- | :--- | :--- | :--- | :--- |
| **RF-01** | Filtro temporal de datos | Ciudadano | Desde la pantalla principal, el usuario selecciona un mes y un año mediante dos selectores y un botón *Ingresar*. La selección define el período activo del sistema y se mantiene visible en la cabecera durante toda la navegación posterior. | El selector permite elegir cualquier mes y año desde el inicio de operaciones del municipio hasta el período más reciente con datos publicados. Tras presionar *Ingresar*, el sistema redirige a la grilla de categorías mostrando "Período activo: Mes Año". |
| **RF-02** | Dashboard de cifras destacadas | Ciudadano | La pantalla principal incluye un panel con tres indicadores resumen del período más reciente (presupuesto ejecutado, personal activo, número de transferencias), presentados en formato de tarjeta con icono y cifra prominente. | El panel muestra exactamente tres tarjetas con valores actualizados al período más reciente disponible. Cada cifra se acompaña de una etiqueta descriptiva en lenguaje no técnico. |
| **RF-03** | Navegación por categorías de transparencia | Ciudadano | Tras aplicar el filtro temporal, el sistema despliega una grilla con las diez categorías de información obligatorias por la Ley 20.285: Estructura y Organización, Remuneraciones del Personal, Contrataciones y Compras, Transferencias de Fondos, Ejecución Presupuestaria, Subsidios y Beneficios Sociales, Actos y Resoluciones Municipales, Auditorías e Informes de Control, Trámites y Servicios, y Mecanismos de Participación Ciudadana. Cada categoría es accesible mediante una tarjeta con icono, título y descripción breve. | La grilla muestra las diez categorías obligatorias por ley. La selección de una tarjeta redirige a la vista de detalle correspondiente sin recarga completa de página, conservando el período activo. |
| **RF-04** | Vista de detalle por categoría con tabla | Ciudadano | Para las categorías que presentan datos tabulares (Remuneraciones, Contrataciones, Transferencias, Subsidios, Actos y Resoluciones, Auditorías, Trámites), el sistema muestra una tabla paginada y ordenable con los campos relevantes a cada categoría, filtrada por el período activo. | La tabla permite ordenar ascendente/descendente por cualquier columna y paginar con al menos 10 registros por página. En móvil adopta scroll horizontal o transformación a tarjetas. Cada vista incluye su título y descripción contextual. |
| **RF-05** | Visualización gráfica de categorías financieras | Ciudadano | Para las categorías Ejecución Presupuestaria y Transferencias de Fondos, además de la tabla, el sistema presenta un gráfico (torta o barras) acompañado de una bajada ciudadana en lenguaje simple (ej.: "El 38 % se destinó a Obras Públicas"). | El gráfico es interactivo con tooltips e incluye leyenda con porcentajes o montos. La bajada ciudadana se actualiza dinámicamente con los datos del período filtrado y no excede 60 palabras. |
| **RF-06** | Visualización de Estructura y Organización | Ciudadano | La categoría Estructura y Organización presenta el organigrama municipal y la jerarquía de unidades, mediante un componente visual jerárquico (árbol o diagrama). | El organigrama refleja la estructura vigente al período activo. Cada nodo identifica nombre de la unidad y autoridad responsable. |
| **RF-07** | Visualización de Mecanismos de Participación | Ciudadano | La categoría Mecanismos de Participación Ciudadana lista las consultas ciudadanas, audiencias públicas y otros instrumentos participativos disponibles, con fecha, estado y enlace al documento o formulario asociado. | Cada entrada muestra fecha, tipo de mecanismo, estado (abierto, cerrado, en consulta) y enlace funcional al recurso. |
| **RF-08** | Sección educativa sobre Ley de Transparencia | Ciudadano | La pantalla principal incluye un banner permanente, en lenguaje ciudadano, que explica qué es la Ley 20.285, qué obligaciones impone al municipio y por qué es relevante para la comunidad, con enlace a fuentes oficiales. | Texto en español de máximo 250 palabras, sin tecnicismos jurídicos. Incluye al menos un enlace a la Biblioteca del Congreso Nacional o al Portal de Transparencia oficial. |
| **RF-09** | Descarga de datos | Ciudadano | Desde cada vista de detalle, el usuario descarga la información en formato CSV o PDF para uso propio. | Ambos formatos están disponibles. El nombre del archivo incluye categoría y período (ej.: `remuneraciones_2026-03.csv`). |
| **RF-10** | Carga y procesamiento de archivos CSV | Administrador | El administrador carga archivos `.csv` mensuales por categoría a través de una interfaz protegida. El sistema procesa, valida y publica los datos sin requerir edición manual de código o base de datos. | La carga acepta archivos hasta 10 MB. El sistema valida estructura y reporta errores específicos (columna faltante, tipo de dato incorrecto). Los datos están disponibles en las vistas públicas en menos de 5 minutos tras la carga. |
| **RF-11** | Autenticación por roles | Administrador | El sistema diferencia dos roles: Ciudadano (acceso público sin registro) y Administrador (acceso autenticado vía JWT para carga y gestión de datos). El acceso al panel administrativo se realiza mediante el botón *Login* en la cabecera. | Las rutas `/admin/*` devuelven HTTP 401 ante peticiones sin token o con token expirado. Las contraseñas se almacenan cifradas con bcrypt. Tras 3 intentos fallidos consecutivos se aplica bloqueo temporal de 5 minutos. |

## Requerimientos no funcionales

| ID | Nombre | Categoría | Descripción | Criterio de aceptación |
| :--- | :--- | :--- | :--- | :--- |
| **RNF-01** | Diseño responsivo (mobile-first) | Usabilidad | La interfaz se adapta correctamente a móviles (mínimo 320 px), tablets y escritorio, siguiendo los cinco breakpoints del Framework Digital del Gobierno (`< 576 px`, `>= 576 px`, `>= 768 px`, `>= 992 px`, `>= 1200 px`). Las tablas adoptan scroll horizontal o tarjetas en móvil; los gráficos se reescalan al viewport. | La aplicación es operativa sin scroll horizontal involuntario en pantallas desde 320 px. Las tablas se transforman correctamente en breakpoints `< 576 px`. Los gráficos mantienen legibilidad de etiquetas a partir de 320 px. |
| **RNF-02** | Rendimiento de carga | Rendimiento | El sistema prioriza la carga visual del contenido principal mediante prácticas de optimización web: declaración de scripts JavaScript al final del `<body>`, lazy-loading de imágenes y minimización de solicitudes bloqueantes durante el render inicial. | First Contentful Paint (FCP) inferior a 1.8 segundos en escritorio y Time to Interactive (TTI) inferior a 3.5 segundos en conexión móvil 3G simulada (Chrome DevTools, Lighthouse). |
| **RNF-03** | Seguridad de autenticación | Seguridad | Las rutas administrativas se protegen mediante JSON Web Tokens (JWT) con expiración. Las contraseñas se almacenan con hash bcrypt (factor de costo >= 10) y nunca se transmiten en texto plano. Toda comunicación cliente-servidor utiliza HTTPS. | Las rutas `/admin/*` responden 401 ante peticiones sin token o con token expirado. Las contraseñas en base de datos contienen hashes bcrypt verificables. Tras 3 intentos fallidos consecutivos se aplica throttling. |
| **RNF-04** | Accesibilidad web (WCAG 2.1 AA) | Usabilidad | La plataforma cumple el nivel WCAG 2.1 AA, alineándose con los lineamientos del Framework Digital del Gobierno: contraste mínimo de 4.5:1 en texto normal, alternativas textuales en gráficos, navegación completa por teclado, etiquetas semánticas en formularios y compatibilidad con lectores de pantalla. | Verificación con axe DevTools y Lighthouse sin issues de severidad crítica. Todos los flujos principales (filtrar período, navegar categorías, descargar datos) son completables únicamente con teclado. |
| **RNF-05** | Lenguaje ciudadano | Usabilidad | Toda información financiera se acompaña de un texto interpretativo ("bajada ciudadana") en lenguaje simple, evitando tecnicismos contables o presupuestarios. Un usuario sin formación técnica debe comprender el dato sin recurrir a fuentes externas. | Cada vista de datos numéricos incluye al menos un párrafo interpretativo de máximo 60 palabras. Nivel de legibilidad "fácil" o superior según el índice Fernández-Huerta. |


---


# Flujo del Proyecto

El flujo de uso de la plataforma se inicia cuando el ciudadano ingresa al portal y entra a la pantalla principal. Esta vista actúa como punto único de entrada al sistema y reúne tres elementos: un encabezado de bienvenida que presenta la municipalidad, un formulario central con dos selectores (mes y año) acompañado de un botón *Ingresar*, y una sección educativa permanente que explica brevemente qué es la Ley 20.285 y por qué garantiza el derecho ciudadano a consultar la información pública. En la parte inferior se muestran tres tarjetas con cifras destacadas del último período disponible: presupuesto ejecutado, personal activo y número de transferencias del mes.

El usuario selecciona el mes y el año del período que desea consultar, y al presionar el botón *Ingresar* el sistema redirige a la pantalla de categorías de transparencia, transmitiendo el período seleccionado como contexto activo. A partir de este momento, ese período se mantiene visible en la cabecera de cada vista posterior bajo la etiqueta "Período activo: \[mes\] \[año\]", de modo que el usuario nunca pierde la referencia temporal sobre la que está consultando datos.

En la pantalla de categorías, el sistema despliega una grilla con las diez áreas de información obligatorias por la Ley 20.285: Estructura y Organización, Remuneraciones del Personal, Contrataciones y Compras, Transferencias de Fondos, Ejecución Presupuestaria, Subsidios y Beneficios Sociales, Actos y Resoluciones Municipales, Auditorías e Informes de Control, Trámites y Servicios, y Mecanismos de Participación Ciudadana. Cada categoría se presenta como una tarjeta interactiva que incluye un icono representativo, su nombre y una breve descripción de su contenido, lo que permite al ciudadano elegir intuitivamente el área que le interesa explorar.

Cuando el usuario selecciona una de las tarjetas, el sistema lo conduce a la vista de detalle correspondiente, conservando el período activo como filtro implícito sobre los datos mostrados. La estructura interna de cada vista de detalle se adapta al tipo de información que despliega, pero todas comparten un patrón común: un breadcrumb superior que permite regresar a la grilla de categorías, el título y descripción contextual de la categoría, una barra de herramientas con un buscador por palabra clave y un botón de descarga, y el componente principal de visualización de los datos.

En las categorías que presentan información tabular (Remuneraciones, Contrataciones, Subsidios, Actos y Resoluciones, Auditorías, Trámites y Mecanismos de Participación) la información se despliega en una tabla paginada y ordenable que permite al usuario navegar registro por registro, ordenar por cualquier columna y filtrar por nombre o cargo según la categoría. En las vistas de carácter financiero (Ejecución Presupuestaria y Transferencias de Fondos) la tabla se complementa con un gráfico interactivo que ilustra visualmente la distribución del gasto, acompañado de una breve interpretación en lenguaje ciudadano (por ejemplo, "El 38 % del presupuesto se destinó a Obras Públicas") que traduce el dato técnico en una afirmación comprensible para usuarios sin formación contable. En la categoría Estructura y Organización, en cambio, la información se presenta como un organigrama jerárquico que refleja las unidades del municipio y sus autoridades responsables.

Desde cualquier vista de detalle, el ciudadano puede descargar la información actualmente visible mediante el botón *Descargar CSV*, lo que genera un archivo nombrado automáticamente con la categoría y el período consultado (por ejemplo, `remuneraciones-Marzo-2026.csv`). Esta funcionalidad permite al usuario reutilizar los datos en herramientas externas sin necesidad de procesar la información en el portal mismo.

En paralelo al flujo público, el sistema ofrece un canal restringido orientado al personal municipal. En la esquina superior derecha del encabezado, el botón *Login* da acceso a la pantalla de autenticación, separada visualmente del resto del portal mediante un fondo distintivo y una identificación clara como "Acceso exclusivo para funcionarios municipales". El formulario solicita correo electrónico y contraseña, y aplica tres validaciones secuenciales: que ambos campos estén completos, que la contraseña tenga al menos seis caracteres, y que el correo pertenezca al dominio institucional `@santodomingo.cl`. Si alguna validación falla, el sistema muestra un mensaje de error específico sin abandonar la pantalla de login. Cuando las credenciales son válidas, el sistema genera un token JWT con expiración de una hora, lo persiste en el navegador, y redirige automáticamente al panel de administración.

En el panel de administración, el funcionario accede a una vista dividida en dos áreas funcionales. A la izquierda se encuentra el formulario de carga de archivos, donde el administrador selecciona la categoría a actualizar, el mes y año correspondientes, y luego carga el archivo CSV mediante una zona de arrastrar-y-soltar o un selector tradicional. Una vez confirmada la carga, una barra de progreso animada acompaña el procesamiento del archivo y, al finalizar, un mensaje de éxito confirma que los datos están disponibles en el portal público. A la derecha, un panel de historial muestra cronológicamente las cargas realizadas, indicando para cada una la fecha, la categoría afectada, el nombre del archivo y un indicador visual del resultado (éxito o error). Al concluir la sesión, el administrador puede cerrarla mediante el botón *Cerrar sesión* del encabezado, lo que invalida el token y devuelve la navegación al portal público.

Adicionalmente, el sistema integra una barra de herramientas de accesibilidad persistente en la parte superior de todas las vistas públicas, que permite al usuario activar el modo de alto contraste, ajustar el tamaño de la tipografía en tres niveles e iniciar la lectura del contenido en voz alta. Las preferencias seleccionadas se almacenan localmente y se reaplican automáticamente en visitas posteriores, garantizando una experiencia accesible y persistente conforme a los lineamientos WCAG 2.1 AA.

---

## Referencias

* **CIDH (2006)**. Corte Interamericana de Derechos Humanos. *Caso Claude Reyes y otros vs. Chile. Sentencia de 19 de septiembre de 2006*. San José, Costa Rica.
* **CPLT (2018)**. Consejo para la Transparencia. *El Derecho de Acceso a la Información Pública como Derecho Llave para el acceso a otros derechos fundamentales*. Cuaderno de Trabajo Nº 10. Santiago, Chile.
* **CPLT (2021)**. Consejo para la Transparencia. *Estudio Nacional de Transparencia 2020 — Informe Final*. Santiago, Chile.
* **CPLT (2025)**. Consejo para la Transparencia. *Informe de Fiscalización Municipal 2024 — Transparencia Activa*. Santiago, Chile.
* **Ley N° 20.285 (2008)**. *Sobre Acceso a la Información Pública*. Diario Oficial de la República de Chile, 20 de agosto de 2008.
* **ONU (2015)**. Organización de las Naciones Unidas. *Objetivos de Desarrollo Sostenible — ODS 16: Paz, justicia e instituciones sólidas*. Nueva York: Naciones Unidas.