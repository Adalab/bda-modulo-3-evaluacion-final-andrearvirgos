
## Apuntes sobre la exploración y limpieza de datos:

- 'Customer Loyalty History.csv'

    * El documento contiene la información del historial de cada cliente.

    * df_history está formado por 16737 filas y 16 columnas.
    * Loyalty Number (Identificador único para cada cliente y columna clave para unir los 2 datasets): Dato tipo int.
    * Country: Dato tipo object.             
    * Province: Obj.        
    * City: Obj.                
    * Postal Code: Obj.          
    * Gender: Obj.               
    * Education: Obj. 
    * Salary: Dato tipo float. Presenta un 25,32% de nulos.  
    * Marital Status: Obj. 
    * Loyalty Card: Obj. 
    * CLV: Flt.
    * Enrollment Type: Obj. 
    * Enrollment Year: int.
    * Enrollment Month: int.
    * Cancellation Year:  Flt. Presenta un 87,65% de nulos.
    * Cancellation Month:  Flt. Presenta un 87,65% de nulos, igual que la columna anterior(relacionada), ¿sentido? Estas 2 columnas son el mes y el año de cancelación de la membresía.
        - Si el grueso de sus clientela sigue suscrita, eso justifica la elevada presencia de nulos, entendiendo que éstos responden a ello (nulos= suscripción activa?)
        - Estos nulos son valores faltantes, que Numpy completa con NaN por simplicidad y legibilildad.
        - A pesar del elevado número de nulos, no será una columna susceptible de ser borrada, porque se perderían datos valiosos (de clientela aún suscrita), podría transformar datos (su categoría es numérica, y son fechas - si quisiera hacer a posteriori una media del año de cancelaciones, si he convertido los NaN en 0-1, distorsionaría el resultado-), ¿una columna nueva del estado? ACTIVO-NO ACTIVO y conservo las otras por lo que pueda devenir?

    * Al sacar las estadísticas descriptivas con df_history.describe(include="O").T -con "O" para aplicarlas sobre los objetos- : Se podría concluir que dado que el count (el total de registros no nulos, 16737) es igual al freq (la frecuencia del valor más común, 16737), y que solo hay 1 valor unique, que todas las entradas de Country son Canadá. Va a ser una columna con varianza cero, no se podrá usar para segmentar clientela, para eso habría que usar la provincia o la ciudad.
    * En las estadísticas descrptivas de las varibales numéricas, destacar:
        - Loyalty Number es un identificador único, que no se usa para cálculos estadísticos.
        - CLV tiene un count de 16737, sin nulos. Lo destacable es que tiene un valor max de 83325.38 mucho más elevado que el 75% de los datos(8940.58). ¿Outliers?
        - Enrollment Year/Month presenta datos coherentes tanto para año (mínimo 2012, max. 2018), como para el mes(1-12).
        - La columna salario presenta rarezas y habrá que limpiarla, porque devuelve datos poco coherentes: Su recuento es de 12499, tiene 4238 valores nulos. ¿Gente que no percibe salario?; el valor mínimo es NEGATIVO ¿error de registro?; el valor máximo es mucho más alto que el 75%, posibles outliers en columna salario.

    * Gestión de Nulos: 
        - COLS. Cancalletion Year y Cancellation Month: No son errores de registro, si no información implícta (esa mayoría de nulos significa "no cancellation"). Para no alterar datos, crear una columna extra, llamada "Status", binaria 0-1 Cancelled-Active. 


    * Hay 4 columnas con datos tipo float, 3 con integers y 9 cuyos datos son tipo objeto.




- 'Customer Flight Activity.cvs' 

    * El documento aporta la información transaccional/temporal sobre la actividad de vuelo de los clientes, registrada mes a mes.

    * df_activity está formado por 405624 filas y 10 columnas:
    * Loyalty Number (Identificador único para cada cliente y columna clave para unir los 2 datasets).
    * Year
    * Month
    * Flights Booked
    * Flights with Companions
    * Total Flights
    * Distance
    * Points Accumulated
    * Points Redeemed
    * Dollar Cost Points Redeemed

    * Todas las columnas son integers, menos 'Points Accumulated' que es un float.
    * No hay valores nulos.
    

- Análisis breve previo al mergeo de los archivos. Para poder identificar más fácilmente las estadísticas descriptivas (en el df_history usamos el describe(include="O").T porque este df contiene objetos que queremos revisar), para visualizar duplicados y conocer qué filas podrían eliminarse de cara a un proceso de merge más rápido y eficiente.

- Creé una función para recorrer las columnas de los dos df, y comprobar los duplicados de filas por columna. Por si fuera correcto eliminar alguna fila inútil, de cara a facilitar (y abreviar) el proceso de mergeo. Y este es un breve repaso por los resultados:

    - df_activity:
    * Loyalty Number: Confirma misma cantidad de clientes que en la columna gemela del df_history.
    * Year: 2 valores únicos. Los registros son de 2017 o 2018
    * Month: 12, uno por mes (lógico).
    * Flights Booked: 22 valores únicos (una repetición alta es normal ya que muchos clientes pueden coincidir en su número de vuelos reservados)
    * Flights with Companions: 12 valores únicos, del máximo de personas con las que ha reservado vuelos alguien.
    * Total Flights: 33 valores únicos (una repetición alta es normal ya que muchos clientes pueden coincidir en su número de vuelos reservados)
    * Distance: 4746 distancias únicas repetidas (normal, porque los vuelos son constantes en sus trayectos, del punto A al punto B)
    * Points Accumulated: 1549 puntos acumulados a lo largo del mes.
    * Points Redeemed: 587 puntos convalidados por algo, a lo largo de un mes.
    * Dollar Cost Points Redeemed: 49 valoraciones distintas de coste ($) de los puntos convalidados a lo largo del mes.

    - df_history:
    * Loyalty Number: Confirma misma cantidad de clientes que en la columna gemela del df_activity.
    * Country : Esta columna podría eliminarse, solo hay 1 registro único, Canadá.
    * Province: 11 (factible, porque oficialmente Candá tiene 13 provincias)
    * City: 29 (este dato raro -obviamente hay más de 29 ciudades en Canadá- podría analizarse contrastando ésta con las columnas de provincias y de códigos postales)
    * Postal Code: 55 (otro dato raro -obviamente hay más de 55 códigos postales en Canadá, son como 900.000- de modo que serán los cp de las ciudades que aparecen como registros únicos. Y podría analizarse contrastando ésta con la columna de ciudades)
    * Gender: 2   (Lo esperado, Female/Male). Columna susceptible de transformación, en Next Steps.
    * Education: 5 (En vistazo rápido:'High school or below', 'College', 'Master', etc)
    * Salary: 5891 valores únicos de cifras de salario percibido.
    * Marital Status: 3 (Single, Married, Divorced).
    * Loyalty Card: 3 (Star, Aurora, Nova)
    * CLV: 7984 valores únicos posibles.
    * Enrollment Type: 2 (Standard, Promotion).
    * Enrollment Year: 7 (años en los que pudo inscribirse el cliente).
    * Enrollment Month: 12, pura lógica. 
    * Cancellation Year: 7 (años en los que pudo inscribirse el cliente).
    * Cancellation Month: 13 (12 meses posibles + el valor NaN, que indica 'No cancelado/Activo').

- Limpieza de anomalías en ambos datasets, pre-merge:
    * Transformar el registro negativo de la columna 'Salary' del df_history, a NaN como estrategia de gestión de riesgos (paso intermedio): Podría tratarse de un error tipográfico, o un error al introducir la cifra, pero el hecho de que sea un valor tan raro en comparación a los demás hace que la suposición sea demasiado arriesgada. Con lo que veo preferible tratarlo como un valor nulo y que así se agrupe con el resto de nulos que presenta esa columna (4238). 
    * En la Fase 2, gestionaré todos esos nulos agrupados, imputándolos con la mediana (que es más robusta a los outliers y a los nulos, que la media), para que no altere las analíticas.

- Revisión post-merge:
    * Hay solo 3 columnas con datos tipo float. Una es la de 'Salary', que nos interesa en ese formato, al tratarse de dólares.
    * La segunda columna es 'Points Accumulated', que aunque llamaba la atención por ser sus datos de tipo float, contraintuitiamente, pueden ser (y son) con decimales.
    * El CLV es una métrica financiera clave que representa el beneficio neto total que una empresa puede esperar de un cliente a lo largo de su relación. Por lo tanto tiene sentido que sea un dato decimal ($).

    * 



## Apuntes sobre visualizaciones:

- Las gráficas llevan de mano una breve interpretación.
- Incorporar visualizaciones tb en la parte del anállisis.

## Notas sobre Next Steps:

- Como este ejercicio puede ser infinito, por seguir analizando y desgranando los datos, añadir en este apartado lo que creo que podría seguir analizando, filtrando, visualizando... para que al menos quede constancia de que, de no ser por el tiempo, tenía ideas para seguir haciendo.

- Tras gestionar los nulos de la columnas Cancellation Year/Month, creando una columna binaria con el estado. Lo siguiente sería plantearse la idoneidad de eliminar esas dos columnas, ya que en su mayoría de resgistros no aporta un dato más específico que si está activa o no. O quizás estudiar la manera de integrar los datos reales de cancelación en el estado "cancelled" por si fueran necesarios -> Mejor antes del mergeo, por eficiencia del tiempo de ejecución. ✔️

- Country es una columna susceptible de ser eliminada, porque no aporta información nueva (varianza cero), que no vaya ya implícita en otras columnas como provincia, ciudad, código postal. Ya que el país por defecto, sería Canadá. ✔️

- Transformación para la columna Gender, para que contemple otras identidades de género (non-binary, a-gender, gender-fluid), para trabajar en post de las políticas de inclusión de la empresa, y por ende, en su imagen de marca.

- Corregir mezcla de idiomas: Los datasets están creados en inglés, el enunciado del ejercicio en castellano y aunque he procurado ceñirme a eso (nombres de columnas en inglés, etc.) he sucumbido a nombrar en castellano algunas variables. Lo correcto sería unificar todo el código a un mismo idioma, en este caso el inglés, que era el original. 

- Consultar la lógica detrás de que el tipo de dato de la columna 'Points Accumulated' sea un decimal. ¿De qué modo están calculando esos puntos para que den unos resultados tan 'complejos'? ¿Podría mejorarse?

- Calcular la antigüedad de la clientela, usando las columnas de 'Enrollment Year/Month' por una cuestión lógica, si la empresa tiene un pla de fidelización por puntos es que les interesa retener a esa clientela. Y la antigüedad en la pertenencia a algo, está correlacionada con la lealtad y el riesgo de abandono. Desde una perspectiva de negocio, podría ser interesante ver cómo se relacionan esos datos, junto con los de los puntos acumulados, etc. Para concluir de qué manera estimular tanto la permanencia en el programa, como la afiliación de nuevas personas.

- Consultar a la empresa la política de acumulación de puntos para clientela sin tarjeta de fidelización. La gráfica muestra que se están acumulando unos puntos que no corresponden a ninguna de las 3 categorías indexadas. Proponer la creación de esa categoría (hasta ahora por defecto), para recoger de manera más eficiente esa acumulación. Y como idea de negocio, si son puntos de clientes que no los van a reclamar/redimir, porque no son miembros: Opción 1, trabajar en pos de la suscripción de esa clientela que está perdiendo puntos/descuentos sin saberlo. U opción2, proponer a la aerolínea llevar a cabo un proyecto social que destine esas millas acumuladas/descuentos/o lo que sea, a facilitar la mobilidad a personas con menos recursos (o personas desplazadas de su lugar de origen que no puedan permitirse viajar,etc).

- Una vez agregadas más catergorías para 'Gender' podrían estudiarse nuevamente los datos, porque quizá, abriendo la mano a más realidades de identidad de género, esos resultados que nos devuelven las gráficas hoy puedan variar. Lo hagn o no, indiscutiblemente esta actualización de la forma de recoger información, redundará en beneficio de la empresa a varios niveles. 