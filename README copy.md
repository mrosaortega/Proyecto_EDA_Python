# Proyecto_EDA_Python
Título del proyecto : EDA de campaña de marketing directo de institución bancaria.


Objetivo del análisis: análisis de una campaña de marketing mediante llamadas telefónicas para el cual se requería más de un contacto con el mismo cliente para determinar si el producto (depósito a plazo bancario) sería suscrito o no.


Herramientas utilizadas
Python: pandas, numpy
Visualización:  seaborn, matplotlib

Pasos a seguir
Análisis preliminar
Doc. 00_Analisis_preliminar_limpieza
Datos:
 0 *Unnamed: 0 * -> columna inválida  
 1   age         -> float64 cambiar a int / tiene nulos
 2   job         -> object /tiene nulos
 3   marital     -> object /tiene nulos / los valores estan en mayus cambiar a minus 
 4   education   -> object /tiene nulos
 5   default     -> float64 cambiar a int es booleano/tiene nulos
 6   housing     -> float64 cambiar a int es booleano/tiene nulos
 7   loan        -> float64 cambiar a int es booleano/tiene nulos
 8   contact     -> 43000 non-null  object 
 9   duration    -> 43000 non-null  int64  
 10  campaign    -> 43000 non-null  int64  
 11  pdays       -> 43000 non-null  int64  
 12  previous    -> 43000 non-null  int64  
 13  poutcome    -> 43000 non-null  object / los valores estan en mayus cambiar a minus 
 14  emp.var.rate ->  43000 non-null  float64
 15  cons.price.idx -> es tipo object deberia ser float - cambiar "," por "." /tiene nulos
 16  cons.conf.idx  -> es tipo object deberia ser float - cambiar "," por "."
 17  euribor3m      -> es tipo object deberia ser float - cambiar "," por "." /tiene nulos
 18  nr.employed    -> es tipo object deberia ser float - cambiar "," por "."
 19  y              -> 43000 non-null  object (yes/no)
 20  date           -> object - cambiar a datetime - meses en español /tiene nulos
 21  latitude       -> 43000 non-null  float64 - no resulta interesante para el análisis
 22  longitude      -> 43000 non-null  float64 - no resulta interesante para el análisis
 23  id_            -> 43000 non-null  object - vamos a modificar el nombre de la columna a "ID"
 
- Variables sociodemográficas: edad, trabajo, estado civil, nivel educativo.
- Variables económicas y financieras: ingresos, préstamos, indicadores macroeconómicos.
- Variables de comportamiento: duración de la llamada, número de campañas, visitas web.
- Variable objetivo: 'y' (binaria: yes / no).

 Aplicamos los principales métodos para obtener información de manera genérica de nuestros datos. Con el método info, shape, describe. 
 
 
# Preparación y limpieza de los datos
Durante la fase de preparación se llevaron a cabo las siguientes acciones:
- Tratamiento de valores nulos y revisión de inconsistencias. Teniendo en cuenta los outliers de las columnas con valores nulos.
- Detección y análisis de outliers mediante técnicas visuales (histogramas, boxplots, gráficos de barras).
- Homogeneización de nombres y tipos de datos.
- Unión de múltiples fuentes de datos.
- Creación de variables derivadas para enriquecer el análisis (entre otros, ingresos ajustados al tamaño del hogar).

 Hemos revisado la cantidad de valores por categoria y revisado si habia erratas. De este método tambien hemos podido sacar la siguiente conclusión: De esta distribución podemos hacer la siguiente explicación de los datos:
   - La mayoria de los clientes tienen la actividad profesional "admin" o "blue-collar"
   - Están casados
   - Tienen estudios universitarios o de instituto
   - El contacto mayor fue por movil
   - No ha habido previo contacto por campaña de marketing
   - No han suscrito producto/servicio
   
Teniendo en cuenta que hay valores nulos que pueden modificar esta conclusion.

Doc 00_limpieza_nulos

Procedemos a gestionar los valores nulos:
 - Columnas numéricas
    Comenzamos visualizando estas columnas, para revisar a simple vista los posibles valores atípicos y la distribución de los datos.

    De las columnas numéricas (sin tener en cuenta los booleanos), únicamente tienen valores nulos : 'age', 'cons.price.idx', 'euribor3m' 
    Columnas booleanas a enmendar: "default", "housing", "loan"

Una vez gestionados los valores nulos de las columnas categoricas, se gestionan los valores nulos de las booleanas y por último:
- Columnas categóricas.
   

Después, revisamos los valores de las columnas que sin valores nulos tienen valores atípicos. Comprobamos a partir de qué valor estos valores dejan de tener sentido y modificamos, usando los quantiles. 
Columnas "pdays", "duration", "campaign", "previous".


Análisis del archivo excel:
01_analisis_preliminar_excel
Comenzamos leyendo y analizando mediante el metodo info cada hoja del excel, que se corresponde con los datos de 2012, 2013 y 2014.
Comprobando así que todas las hojas tienen las mismas columnas:
 0   Unnamed: 0          non-null  int64         
 1   Income              non-null  int64         
 2   Kidhome             non-null  int64         
 3   Teenhome            non-null  int64         
 4   Dt_Customer         non-null  datetime64[ns]
 5   NumWebVisitsMonth   non-null  int64         
 6   ID                  non-null  object  

En ninguna hoja hay valores nulos, los valores tienen el mismo tipo, en cada hoja hay diferente cantidad de valores.
Dado que no hay diferentes columnas, podemos realizar mediante el metodo concat la unión de las 3 hojas.

Se aplican estadísticos para sacar conclusiones, agrupando por diferentes categorías y aplicando la media.
Guardamos este archivo en csv.


Por último, unimos las dos bases de datos convertidas a csv en el archivo 00_union_analisis mediante el metodo merge left.

Decidimos aplicar un cambio a la columna "y", duplicando la información pero de forma numérica.{"no": 0, "yes": 1}
Dado que consideramos que esta variable "y" es la principal de cara a las siguientes comprobaciones estadísticas y el análisis en general.
Se ha observado que la campaña no ha tenido mucho éxito dado que el mayor porcentaje de resultados de y es "no" o en su defecto "0". Indica si el cliente ha suscrito un producto o servicio (Sí/No). 

Realizamos análisis bivariado entre las variables explicativas y la variable objetivo.
Respondiendo a las siguientes preguntas:

- ¿Las llamadas más largas convierten más? - Vemos que no, que tanto para contratar como para no contratar la duracion tiene una media similar.

- ¿Más ingresos → más probabilidad de “yes”? No, para ambas opciones, la media de los ingresos es similar.

- ¿Insistir en muchas campañas sirve o perjudica? aqui quizas podemos decir que cuantos mayores intentos más se ha respondido que no.

- Para el rango de edad, es mas amplio para los que contratan, aunque la media para ambas opciones se mantiene bastante similar cercana a los 40 años.

Las caracterisiticas personales no afectan mucho a la hora de contratar servicios. Se comprobueban si afectan más las variables económicas. 
- "Euribor" sí afecta a la hora de la decición, pues los que contratan tiene la media bastante mas baja que los que que no contratan. Cuanto mas bajo es el euribor, mas gente contrata.

En general, para todas las variables económicas, los que contratan tienen medias mas bajas que los que no contratan.



Creación de variables derivadas para enriquecer el análisis
"total_hijos y "income_hijos"


Hacemos una matriz de correlación para ver que valores están relacionados. Vemos que los valores que tienen correlación (negativa) son aquellos valores económicos no relacionados con las caterísticas de los clientes.

# Principales hallazgos

Los principales resultados obtenidos en el análisis exploratorio son:

El análisis es exploratorio y no establece relaciones causales.

La campaña presenta una baja tasa de conversión, lo que es consistente con campañas de marketing masivas.

La duración de la llamada es una de las variables más asociadas a la conversión. Así como las variables macroeconómicas.

Las variables macroeconómicas muestran fuertes correlaciones entre sí, lo que sugiere la presencia de multicolinealidad.

Las variables sociodemográficas presentan diferencias entre los grupos, aunque con menor capacidad explicativa que las variables relacionadas con la campaña.

El número de contactos realizados en la campaña parece estar asociado negativamente con la probabilidad de conversión.
