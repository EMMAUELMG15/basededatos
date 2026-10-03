Artículo 1. Datos Sísmicos

Referencia: Villa Vargas, J. M., Hurtado Avilés, G., y Climent Hernández, J. A. (2026). Cuando México tiembla: la historia contada por los datos. AZCATL Revista de Divulgación de la Ciencia, la Ingeniería y la Innovación, 4(6), 28-33.

1. ¿Qué problema aborda y por qué es importante?

El artículo habla sobre el análisis de los sismos que ocurren en México y sobre la dificultad que existe para interpretar toda la información que se genera sobre estos fenómenos. México se encuentra en el Cinturón de Fuego del Pacífico y sobre cinco placas tectónicas: Norteamérica, Pacífico, Rivera, Cocos y Caribe. Debido a esto, aproximadamente el 70 % del territorio nacional está expuesto a riesgo sísmico, incluyendo zonas con una gran cantidad de población como el Valle de México y la zona metropolitana de Guadalajara.

Uno de los principales problemas es que los sismos no pueden predecirse con exactitud, por lo que resulta importante estudiar los eventos que ya han ocurrido y encontrar patrones mediante los datos históricos. El Servicio Sismológico Nacional (SSN) cuenta con información de los sismos ocurridos en México desde 1900, incluyendo datos como magnitud, profundidad y ubicación. Sin embargo, esta información puede resultar complicada de interpretar para personas que no tienen conocimientos técnicos.

Por esta razón, los autores desarrollaron un sistema de visualización de datos sísmicos que permite transformar la información técnica en mapas, gráficos y reportes más fáciles de interpretar. Además, el sistema puede relacionar los datos de los sismos con información demográfica y económica del INEGI, lo que permite realizar análisis más completos sobre el posible impacto de estos fenómenos.

2. ¿De dónde provienen los datos y qué se hizo con ellos?

La información principal se obtiene del Servicio Sismológico Nacional, de donde se descargó un archivo CSV con más de 300,000 registros de sismos ocurridos en México desde 1900.

También se utilizaron datos del INEGI, principalmente información del Censo de Población y Vivienda 2020 y de los Censos Económicos.

Antes de introducir la información en la base de datos fue necesario realizar una etapa de limpieza y validación. Para esto se desarrolló un programa en Python que revisa los archivos CSV y busca problemas como campos vacíos, coordenadas incorrectas o formatos que no son compatibles. Los registros que no cumplen con las condiciones necesarias son eliminados y posteriormente se genera código SQL con la información validada.

Después de esta limpieza, los datos son cargados en PostgreSQL, donde se organizan mediante una estructura de Data Warehouse. De esta manera, la información queda preparada para poder realizar consultas y análisis posteriormente.

3. ¿Cómo se modeló la información?

La información se organizó mediante un modelo de Data Warehouse dimensional. Este modelo permite relacionar diferentes tipos de información y realizar consultas de manera más eficiente.

Las principales dimensiones utilizadas son:

Dimensión geográfica o de presencia: permite trabajar con los estados y ubicaciones relacionadas con los eventos.
Dimensión tiempo: contiene información como año, mes y fecha.
Dimensión censal: contiene información relacionada con población e indicadores económicos.

También se utiliza una tabla de hechos llamada fact_impacto_sismo_censo, donde se concentra información de los eventos sísmicos, como magnitud, profundidad, coordenadas del epicentro, distancia a poblaciones y diferentes métricas relacionadas con el posible impacto.

La idea principal es que el evento sísmico funcione como el elemento central que puede relacionarse con el tiempo y con información geográfica, poblacional y económica. Esto permite analizar no solamente dónde ocurrió un sismo, sino también qué características tenía la zona cercana al evento.

4. ¿Qué preguntas puede responder el sistema?

Una de las ventajas del sistema es que permite realizar diferentes consultas dependiendo de los filtros que se utilicen. Por ejemplo:

¿Cuántos sismos de magnitud mayor a 2, 4 o 6 ocurrieron durante determinado periodo y en un estado específico?
¿Qué cantidad de población y unidades económicas podrían encontrarse cerca del epicentro de un sismo?
¿Existe alguna relación entre la magnitud de los sismos y su profundidad?
¿En qué meses se presentan más eventos sísmicos?
¿Qué regiones presentan una mayor recurrencia de sismos?
¿Qué localidades con una población considerable se encuentran cerca de zonas donde han ocurrido sismos?

El sistema permite realizar este tipo de análisis mediante mapas interactivos, gráficos estadísticos, mapas de calor y reportes personalizados. Los filtros pueden considerar aspectos como el periodo, magnitud, localidad, profundidad, población o actividad económica.

5. Tecnologías utilizadas

Algo que me parece importante del artículo es que no solamente se explica la base de datos, sino también las tecnologías utilizadas para construir todo el sistema.

Para la limpieza de los datos se utilizó Python, mientras que para la base de datos se utilizó PostgreSQL. Posteriormente se desarrollaron servicios utilizando PHP 8, con un servidor Apache.

Para la parte visual se utilizaron HTML5, CSS3 y Bootstrap, mientras que JavaScript permite que la interfaz sea interactiva. Para la parte de mapas se utilizó OpenStreetMap, lo que permite ubicar los eventos sísmicos de acuerdo con sus coordenadas.

También se utilizó Docker Compose para facilitar la instalación y ejecución del sistema. El proyecto se divide principalmente en un contenedor para la base de datos y otro para el servidor web. Esto permite que los diferentes componentes puedan ejecutarse de manera conjunta sin que el usuario tenga que configurar manualmente cada parte.

6. ¿Cuál es la utilidad del sistema?

El sistema no solamente sirve para observar sismos en un mapa. También puede utilizarse en diferentes áreas.

Por ejemplo, puede ayudar al gobierno en la planeación territorial, identificación de zonas de riesgo, establecimiento de rutas de evacuación y elaboración de planes de acción ante posibles réplicas.

También tiene una aplicación educativa, ya que permite mostrar a las personas el nivel de riesgo sísmico que existe en su región y fomentar una mayor cultura de prevención.

En el área de investigación puede ayudar a estudiar patrones sísmicos y comparar cambios en la actividad sísmica utilizando los aproximadamente 125 años de información disponibles.

7. Limitaciones y trabajo futuro

Una de las principales limitaciones es que la instalación puede resultar complicada para usuarios que no tengan conocimientos sobre las herramientas utilizadas, aunque Docker facilita este proceso.

Otra limitación es que actualmente las visualizaciones son principalmente bidimensionales y dependen de los datos que se encuentran disponibles y actualizados en las fuentes oficiales.

Como trabajo futuro, los autores proponen agregar visualizaciones en 3D, incorporar más fuentes de información oficiales y conectar el sistema con plataformas de alerta temprana..