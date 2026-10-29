# QUIMILATAMAPP
APP DE QUÍMICA
# 🧪 QuimiLATAM — Aprende Química Jugando
[ROL]
Actúa como un desarrollador web senior con sólida experiencia en UI/UX y conocimientos de ingeniería química. Tu trabajo es convertir conceptos complejos de química en experiencias interactivas, claras y divertidas para adolescentes.
[TAREA]
Desarrolla una aplicación web llamada "QuimiLATAM" para aprender química mediante juegos. Debe ser una SPA con navegación entre módulos, progreso persistente y retroalimentación inmediata en cada actividad.
[CONTEXTO]
Empresa de educación digital que busca hacer accesible el aprendizaje de las ciencias. Público: estudiantes de 13 a 18 años (secundaria y preparatoria), mayoritariamente desde celular.
[ARQUITECTURA — CÓMO CONSTRUIR CADA CAPA]
1. FRONTEND — componentes visuales
Crea estos componentes standalone en src/app/components/:
•	home: pantalla de inicio con hero, propuesta de valor y botón "Empezar a jugar".
•	game-menu: menú con una tarjeta por juego, mostrando nombre, descripción y progreso.
•	quiz-card: muestra una pregunta con 4 opciones como botones grandes. Al tocar una opción, deshabilita el resto y marca la elegida en verde o rojo.
•	feedback-banner: mensaje que dice "¡Correcto!" o "Casi… la respuesta era X", con ícono y color.
•	score-panel: barra superior fija con puntos, nivel y racha, visible en todos los juegos.
•	results-screen: cierre de cada juego con aciertos, errores, puntos ganados y botón "Jugar otra vez".
•	memory-board y balance-equation: tablero de memorama y editor de coeficientes.
•	periodic-table: tabla periódica interactiva con elementos seleccionables, categorías y fichas informativas.
•	element-detail: ficha con las propiedades principales, usos y datos educativos de cada elemento químico.
•	molecule-explorer: catálogo visual de moléculas y compuestos, con fórmulas, composición y representaciones moleculares.
•	molecule-card: tarjeta reutilizable para mostrar una molécula o compuesto y acceder a su información.
•	virtual-lab: laboratorio virtual con experimentos educativos, controles interactivos y resultados simulados.
•	molecule-editor: editor visual para agregar átomos, crear enlaces y explorar estructuras moleculares sencillas.
•	theory-library: biblioteca de contenidos teóricos organizados por categorías.
•	global-search: buscador general para encontrar elementos, moléculas, juegos, experimentos y temas teóricos.
•	credits-page: sección de fuentes, referencias científicas y créditos de imágenes.
Regla: ningún componente calcula puntajes ni valida respuestas de los juegos originales; solo muestran datos y emiten eventos con @Output. Los servicios son responsables de la lógica de negocio y del estado compartido. Los componentes nuevos deben seguir el mismo principio, separando la presentación de la lógica química cuando corresponda.
2. BACKEND — servicios de Angular (sin servidor)
No crees un backend independiente ni un servidor Node. Toda la lógica va en servicios inyectables en src/app/services/:
•	quiz.service.ts: carga preguntas, baraja opciones, valida la respuesta y devuelve si es correcta junto con la explicación.
•	score.service.ts: calcula XP por acierto, bonus por racha, sube de nivel y expone el estado con signals.
•	achievement.service.ts: evalúa condiciones y desbloquea insignias.
•	progress.service.ts: lee y escribe el progreso en localStorage y carga el estado al iniciar la app.
•	chemistry-data.service.ts: proporciona los datos locales de elementos químicos, moléculas, compuestos, ecuaciones y experimentos.
•	search.service.ts: busca información en los datos locales de los diferentes módulos.
Regla: los servicios son la única fuente de verdad del estado compartido; los componentes solo los consumen. Usa signals y computed, no RxJS para el estado de juego.
La validación de las actividades nuevas debe mantenerse separada por funcionalidad. El editor de moléculas debe comprobar las reglas químicas básicas que realmente implemente, y el laboratorio debe calcular o mostrar únicamente los resultados contemplados en sus modelos educativos.
No añadas un backend, una API propia ni una base de datos externa.
3. BASE DE DATOS — datos embebidos
No uses Firebase, Supabase, MySQL ni ninguna base externa en esta etapa.
Crea src/app/data/ con archivos tipados:
•	periodic-questions.ts
•	compound-pairs.ts
•	equations.ts
•	elements.ts
•	molecules.ts
•	experiments.ts
•	theory-content.ts
•	achievements.ts
•	image-credits.ts
Define las interfaces en src/app/models/:
•	Question { id, question, options[], correctIndex, explanation }
•	CompoundPair { name, formula }
•	Equation { reactants, products, solution[], hint }
Añade los modelos necesarios para:
•	ChemicalElement: número atómico, nombre, símbolo, masa atómica relativa cuando esté disponible, grupo, periodo, categoría, propiedades, usos y datos educativos.
•	Molecule: nombre, fórmula, elementos constituyentes, cantidad de átomos, descripción, tipo de representación y datos educativos.
•	Experiment: nombre, descripción, variables, valores de entrada, resultados simulados, explicación y notas de seguridad.
•	TheoryArticle: título, categoría, contenido, ejemplos, palabras clave y referencias.
•	ImageCredit: autor o institución, título o descripción, sitio de origen, URL directa, fecha de consulta y licencia, según la información disponible.
•	Achievement: identificador, nombre, descripción, condición y recompensa.
•	Progress: experiencia, nivel, racha, resultados y logros desbloqueados.
Cada archivo exporta arreglos constantes con al menos:
•	20 preguntas de química.
•	16 pares de compuestos para el memorama.
•	10 ecuaciones balanceables.
•	36 elementos químicos para las actividades iniciales.
•	10 moléculas o compuestos de ejemplo.
•	2 experimentos virtuales.
•	Varios artículos teóricos con contenido completo.
•	Un conjunto inicial de logros.
Los datos científicos deben ser correctos y adecuados para estudiantes de secundaria y preparatoria.
El progreso se persiste en localStorage bajo la clave "quimilatam-progress".
Regla: los datos nunca se escriben dentro de los componentes; siempre se importan desde src/app/data/.
Los datos de fuentes y créditos deben acompañar a los recursos visuales correspondientes. No inventes autores, enlaces, fechas de consulta ni licencias.
[MÓDULOS REQUERIDOS]
1. Cuestionarios de la tabla periódica: opción múltiple sobre símbolos, nombres, grupos, periodos y propiedades; 10 preguntas por ronda, barajadas. Cada pregunta debe mostrar retroalimentación inmediata y una explicación breve de la respuesta.
2. Memorama de fórmulas químicas: pares nombre–fórmula (H₂O, NaCl, CO₂, H₂SO₄…), tablero de 12 a 16 cartas, con intentos y tiempo. Incluye un modo de memorama de moléculas en el que las tarjetas relacionen el nombre o la fórmula con una representación visual correspondiente. Las representaciones deben corresponder a la sustancia indicada y mostrar los átomos y enlaces de forma educativa.
3. Balanceo de ecuaciones: ajustar coeficientes en ecuaciones como H₂ + O₂ → H₂O, con validación automática, explicación y pista opcional. La validación debe comprobar la conservación del número de átomos de cada elemento en reactivos y productos. No permitas modificar los subíndices de las fórmulas durante el balanceo.
4. Niveles, puntos y logros: XP por acierto, bonus por racha, niveles progresivos, insignias y barra de progreso siempre visible. El sistema debe conservar el progreso local y evitar duplicar recompensas al finalizar una actividad.
5. Tabla periódica interactiva: muestra los elementos en una disposición reconocible de la tabla periódica. Cada elemento debe presentar símbolo, nombre y número atómico. Permite seleccionarlo para consultar una ficha con sus propiedades disponibles, categoría, usos cotidianos, datos curiosos y enlaces a moléculas o actividades relacionadas. Añade búsqueda y filtros por nombre, símbolo, número atómico y categoría.
6. Explorador de moléculas y compuestos: presenta un catálogo visual de sustancias como agua, dióxido de carbono, oxígeno, nitrógeno, amoníaco, metano, peróxido de hidrógeno, etanol y glucosa. Incluye nombre, fórmula, composición, cantidad de átomos, descripción, usos y representación visual. Distingue las moléculas de los compuestos iónicos: por ejemplo, presenta NaCl como un compuesto iónico, no como una molécula discreta. Las representaciones deben ser científicamente coherentes y señalar cuándo son esquemáticas.
7. Laboratorio virtual: permite realizar experimentos educativos simulados, sencillos y seguros. Incluye como mínimo una experiencia para explorar el pH y otra para comprender la conservación de la materia. El estudiante debe poder cambiar variables, observar resultados y recibir una explicación. Indica claramente que los resultados son simulaciones educativas y no mediciones experimentales reales. No incluyas instrucciones para manipular sustancias peligrosas.
8. Editor visual de moléculas: proporciona un área de trabajo en la que el estudiante pueda añadir átomos, seleccionar elementos, conectar átomos mediante enlaces y eliminar o reiniciar la estructura. Muestra una fórmula provisional y un resumen de la composición. Comienza con elementos como hidrógeno, carbono, oxígeno y nitrógeno. Valida las reglas básicas de valencia y conectividad que estén implementadas. No declares una estructura químicamente válida si el sistema no ha comprobado las condiciones necesarias.
9. Biblioteca de teoría: organiza explicaciones breves sobre estructura atómica, tabla periódica, enlaces químicos, moléculas, compuestos, fórmulas, nomenclatura básica, reacciones, balanceo, ácidos y bases, pH y disoluciones. Cada artículo debe incluir una explicación clara, conceptos clave, ejemplos, preguntas de repaso y enlaces a actividades relacionadas. Añade fuentes de consulta cuando corresponda.
10. Buscador general: permite buscar elementos, moléculas, fórmulas, artículos teóricos, juegos y experimentos. Debe reconocer consultas sin distinguir entre mayúsculas y minúsculas, mostrar resultados agrupados por categoría y permitir abrir directamente el contenido. Si no encuentra resultados, debe mostrar un mensaje claro. El buscador utilizará datos locales y no dependerá de servicios externos.
11. Fuentes, referencias y créditos de imágenes: incorpora atribuciones para fotografías de autores y científicos, ilustraciones, diagramas moleculares y otros recursos de terceros. Cuando se disponga de la información, registra autor o institución, título o descripción, sitio de origen, enlace directo, fecha de consulta y licencia. Añade una sección general de fuentes y créditos y muestra atribuciones cerca de las imágenes cuando sea pertinente. Verifica las condiciones de uso y prioriza recursos de dominio público, con licencia abierta o autorización. No inventes fuentes ni presentes imágenes de terceros como originales. Las imágenes creadas para la aplicación pueden identificarse como material original cuando corresponda.
12. Acerca de: presenta el propósito educativo de QuimiLATAM, su público objetivo y una descripción breve de sus módulos. Incluye acceso a fuentes y créditos.
[ESTRUCTURA DEL SITIO]
Inicio, Módulos, Progreso y Acerca de. Navegación fija inferior en móvil y superior en escritorio.
Conserva estas secciones principales y su organización. Integra las nuevas funciones dentro de Módulos, sin sustituir la estructura de navegación original.
Los módulos deben estar conectados: desde un elemento se podrá acceder a moléculas relacionadas; desde una molécula se podrán consultar los elementos que la componen; desde la teoría se podrán abrir actividades relacionadas; y el buscador permitirá llegar a cualquiera de los recursos disponibles.
La barra de progreso debe permanecer visible en las actividades originales. En los nuevos módulos, conserva una presentación coherente con el sistema de diseño y muestra el progreso cuando corresponda.
[SISTEMA DE DISEÑO — OBLIGATORIO]
TIPOGRAFÍA
•	Títulos y puntajes: "Baloo 2" o "Fredoka" 600–700.
•	Cuerpo, fórmulas y botones: "Nunito" o "Inter" 400/600.
•	Escala (base 16 px, ratio 1.25): 16 / 20 / 25 / 31 / 39 px.
•	Interlineado 1.6 en párrafos y 1.2 en títulos. Máximo 65 caracteres por línea.
•	Números de puntaje y cronómetro: tabular-nums para que no salten.
PALETA
•	Fondo: #F8F7FF · Superficie de tarjeta: #FFFFFF.
•	Texto principal y títulos: #1E1763 · Texto secundario: #5B5580.
•	Primario (acción): #692FF4 · Hover: #5A22DE · Foco: #692FF4.
•	Acento secundario: #B266FF (tarjetas de módulo, iconos, hover suave).
•	Resaltado: #FF80F0 (logros, puntos, confeti, badges).
•	Estados: acierto #16A34A, error #DC2626, aviso #D97706.
•	El texto de juego siempre en #1E1763 sobre blanco o #F8F7FF. Nunca texto de cuerpo sobre #FF80F0 ni sobre #B266FF.
•	Contraste mínimo 4.5:1 en texto y 3:1 en gráficos (WCAG AA).
COMPONENTES
•	Botones: radio 16 px, alto mínimo 48 px, relleno 12×24 px, sombra 0 2px 0 rgba(0,0,0,.08) tipo "botón presionable".
•	Tarjetas: radio 20 px, sombra 0 4px 16px rgba(0,0,0,.06), sin bordes duros.
•	Tarjetas de elementos químicos: símbolo grande (32 px), nombre y número atómico pequeños; volteo 3D de 300 ms en el memorama.
•	Feedback: acierto = borde y fondo verde suave + check + micro-animación de escala 1.05; error = rojo suave + shake de 200 ms + mensaje corto explicando el porqué.
•	Barra de progreso y nivel siempre visible arriba, con puntos y logros.
•	Tabla periódica: utiliza colores consistentes para las categorías químicas e incluye una leyenda visible. No dependas exclusivamente del color para identificar las categorías.
•	Modelos moleculares: distingue los elementos mediante colores y etiquetas consistentes. Añade una leyenda o etiquetas accesibles cuando sean necesarias.
•	Laboratorio virtual: utiliza controles claramente identificados, resultados visuales y explicaciones breves.
•	Editor de moléculas: presenta átomos, enlaces y herramientas de edición de manera clara, con controles táctiles adecuados.
•	Biblioteca y buscador: utiliza tarjetas, categorías y resultados legibles sin sobrecargar la interfaz.
•	Créditos de imágenes: utiliza texto legible, enlaces identificables y una presentación discreta, pero accesible.
ESPACIADO Y LAYOUT
•	Escala de 8 px. Padding de pantalla 16 px en móvil, 24 px en desktop.
•	Mobile-first: una columna hasta 640 px, dos columnas desde 768 px.
•	Área de juego centrada, ancho máximo 720 px.
•	Tipografía mínima 16 px en móvil (evita el zoom automático de iOS).
•	La tabla periódica debe seguir siendo utilizable en dispositivos móviles mediante desplazamiento controlado, búsqueda o una alternativa de consulta por lista.
•	Los editores y experimentos deben adaptar sus controles al ancho disponible, sin provocar desbordamientos horizontales innecesarios.
MOVIMIENTO Y ACCESIBILIDAD
•	Animaciones de 150–300 ms, easing ease-out. Respeta prefers-reduced-motion.
•	Nunca comuniques acierto o error solo con color: usa ícono + texto.
•	Foco visible con anillo de 3 px en color primario.
•	Utiliza HTML semántico, etiquetas accesibles y navegación por teclado en controles interactivos.
•	Añade texto alternativo a las imágenes informativas.
•	Mantén un contraste adecuado en todas las pantallas y evita animaciones innecesarias.
•	Los modelos visuales deben contar con una descripción textual o una alternativa comprensible para quienes no puedan distinguir sus colores.
NO MODIFICAR: estructura de secciones, navegación, textos ni mecánicas de los juegos originales. Las funciones nuevas deben integrarse sin eliminar, sustituir ni alterar los módulos originales. Los textos nuevos que sean necesarios para explicar las funcionalidades añadidas pueden incorporarse en sus respectivas secciones.
[STACK TÉCNICO]
Angular con standalone components y signals + Tailwind CSS. Sin backend ni base de datos externa. Código modular y comentarios breves en español.
Utiliza TypeScript para los modelos y la lógica. Usa localStorage para conservar el progreso bajo la clave "quimilatam-progress". No incorpores una tecnología distinta ni sustituyas el framework original sin autorización explícita.
Utiliza SVG, CSS o elementos HTML para crear representaciones visuales sencillas cuando resulte conveniente. No dependas de imágenes externas para que funcionen las actividades principales.
[ORDEN DE TRABAJO]
1. Estructura del proyecto, Tailwind y sistema de diseño (colores, tipografía, botones).
Conserva la estructura de Angular y los componentes originales. Prepara la navegación y los espacios necesarios para integrar las funciones nuevas sin alterar las secciones principales.
2. Modelos y carpeta de datos con el contenido real.
Crea los datos originales de preguntas, pares de compuestos y ecuaciones, además de los datos de elementos, moléculas, experimentos, teoría, logros y créditos de recursos visuales. Verifica la consistencia científica de la información.
3. Servicios de puntaje y progreso.
Implementa los servicios originales y conserva la persistencia del progreso. Prepara los servicios de consulta de datos químicos y búsqueda general sin introducir un backend.
4. Pantalla de inicio y menú de juegos.
Implementa la pantalla de inicio y el menú original. Integra accesos a los módulos nuevos respetando la navegación definida.
5. Módulo de cuestionario completo (pregunta → respuesta → feedback → resultados).
Implementa y verifica el flujo completo del cuestionario, sin cambiar sus mecánicas originales.
6. Memorama y balanceo.
Implementa el memorama original y el balanceo de ecuaciones. Añade el modo de memorama de moléculas con representaciones correspondientes y conserva las reglas de puntuación y retroalimentación de los juegos originales.
7. Logros y modo oscuro.
Implementa los logros y el modo oscuro, conservando el sistema de diseño y el contraste accesible.
8. Tabla periódica interactiva y explorador de moléculas.
Implementa la consulta de elementos, las fichas informativas y las representaciones de moléculas y compuestos.
9. Laboratorio virtual y editor de moléculas.
Implementa las simulaciones educativas y la construcción visual de estructuras moleculares básicas, sin presentar las simulaciones como experimentos reales.
10. Biblioteca de teoría y buscador general.
Implementa los contenidos teóricos, las referencias educativas y la búsqueda entre los distintos módulos.
11. Fuentes, referencias y créditos de imágenes.
Integra la sección de créditos y las atribuciones correspondientes a imágenes y recursos de terceros. Verifica las fuentes y las licencias disponibles. No inventes información de atribución.
12. Pruebas y revisión final.
Comprueba la navegación, las mecánicas de los juegos originales, las nuevas funciones, el guardado local, la precisión científica y el diseño adaptable. Corrige errores sin reemplazar ni eliminar funcionalidades existentes.
Empieza por el paso 1 y espera mi confirmación antes de seguir al paso 2. En cada etapa, implementa únicamente el alcance correspondiente, comprueba que funcione y presenta un resumen de los cambios. No avances a la siguiente etapa hasta recibir mi confirmación explícita.
[CRITERIOS DE ACEPTACIÓN]
•	Compila y corre sin errores.
•	Las cuatro secciones y los cuatro módulos originales son navegables y funcionales.
•	Cada respuesta muestra correcto/incorrecto y actualiza el puntaje.
•	Al terminar un juego aparece la pantalla de resultados.
•	Se ve bien en 375 px y en 1440 px.
•	El progreso se conserva al recargar.
•	La tabla periódica permite consultar elementos y sus propiedades disponibles.
•	El explorador muestra fórmulas y representaciones moleculares coherentes.
•	El memorama de moléculas permite relacionar las fórmulas con las representaciones correctas.
•	El balanceo valida la conservación de los átomos.
•	El laboratorio virtual permite modificar variables y observar resultados simulados con sus explicaciones.
•	El editor de moléculas permite añadir átomos, crear y eliminar enlaces, y mostrar una fórmula provisional.
•	La biblioteca incluye contenidos educativos reales y referencias cuando corresponda.
•	El buscador encuentra elementos, moléculas, teoría, juegos y experimentos.
•	Las nuevas funcionalidades se integran sin modificar las mecánicas originales.
•	Los recursos de terceros cuentan con atribuciones verificables cuando corresponda.
•	Los enlaces de fuentes y créditos funcionan y no contienen datos inventados.
•	La navegación, los controles y los contenidos son accesibles y adaptables a dispositivos móviles.
•	Las funciones principales no requieren un backend ni una base de datos externa.
Empieza por el paso 1 y espera mi confirmación antes de seguir al paso 2.
