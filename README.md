# Proyecto_EDA_Python
primer paso: análisis preliminar
Doc. 01_analisis_preliminar_csv
 Se ha revisado la información del DataFrame - con lo que se ha podido ver que algunas columnas no estaban en el dtype correcto. 
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
 
 
 se ha revisado la cantidad (en numero y porcentaje) de valores nulos de cada columna. Principales estadisticos de las columnas numéricas. Tras esto, Analizamos los valores unicos de algunas columnas que parecian tener valores atípicos. 
  Ej. columna "pdays" se ha observado que la mayor cantidad de valores corresponde al numero "999" considerando el significado de este valor 
    pdays: Número de días que han pasado desde la última vez que se contactó con el cliente durante esta campaña.
    los valores estan cercanos al 0 y en el 999, lo que se puede inferir que en su mayoria no se ha contactado aun con muchos clientes y con los que se ha contactado ha sido muy reciente. - Posteriormente en el archivo 03_nulos hemos modificado este valor por "no contactado".
 Las columnas a modificar han sido: cambiar de "float" a "int" columnas : " age", "default", "housing", "loan". Pero hay nulos en estas columnas por lo que no se puede aun modificar y columna "date" tipo date - tambien tiene nulos.

 en cuanto a los nombres de las columnas solo hemos hecho un cambio en "id_" modificando a "ID". Dentro de los valores de las columnas hemos puesto en minisculas todos los valores ya que se mezclaban palabras en mayusuculas con minisculas.
 Hemos revisado la cantidad de valores por categoria y revisado si habia erratas.

 Doc 03_limpieza_nulos
 Tenemos columnas con "," en vez de "." por lo que estaban identificadas como "object". Han sido modificadas a "float".
 Se ha intentado identificar razon por la que alguno de los valores sea nulo pero no lo hemos identificado.
 Se han generado graficas para ver y entender la distribucion de los valores en aqueññas columnas en las que hay nulos, es decir, necesitabamos verificar si las columnas de valores nulos tenian valores atipicos y en base a esto poder elegir el mejor valor a reemplazar (media, mediana u otra categoria). adicionalmente se ha calculado el porcentaje de valores atipicos mediante el calculo de cuartiles, rango intercuartilico:
    En la columna AGE tenemos un total de 576 outliers, lo que representa un 1.3395348837209302 % del total
    - Hay outliers entre 70-100 pero la de mayor cantidad de valores estan entre los 30 -50 años. Cogeremos la mediana para evitar sesgos.
    En la columna CONS.PRICE.IDX tenemos un total de 0 outliers, lo que representa un 0.0 % del total
    En la columna EURIBOR3M tenemos un total de 0 outliers, lo que representa un 0.0 % del total
    
    
    - Columnas "default" "housing" "loan" - variables binarias : en "default" y "loan" se ve que lo que mas se repite es el 0 mientras que en "housing" parece estar repartido. 
        - por lo que para "default" vamos a *modificar los valores desconocidos por "unknown"*
        - para la columna "housing" los datos están valanceados, estando la mayoria/moda en el 1 por lo que podemos *modificar los NaN por 1*
        - para "loan" aunque tambien la cantidad de valores entre 0 y 1 es grande, no hay tantos valores nulos, siendo la moda 0, *modificados Nan por 0*





●	campaign: El número de contactos realizados durante esta campaña para este cliente. 
    La mayor parte de los valores estan distribuidos entre el 1 y el 10 aunque concentrados entre el 1 y 3, llegando los valores hasta el 56.
●	
●	previous: Número de veces que se ha contactado con el cliente antes de esta campaña.
    El valor mas repetido es el 0, seguidos del 1 y 2, habiendo valores hasta el 7