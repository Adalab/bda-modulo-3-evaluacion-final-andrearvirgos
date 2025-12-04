
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

        - Al sacar las estadísticas descriptivas con df_history.describe(include="O").T -con "O" para aplicarlas sobre los objetos- : Se podría concluir que dado que el count (el total de registros no nulos, 16737) es igual al freq (la frecuencia del valor más común, 16737), y que solo hay 1 valor unique, que todas las entradas de Country son Canadá. Va a ser una columna con varianza cero, no se podrá usar para segmentar clientela, para eso habría que usar la provincia o la ciudad.

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
    

## Apuntes sobre visualizaciones:
- Las gráficas llevan de mano una breve interpretación.
- Incorporar visualizaciones tb en la parte del anállisis.

## Notas sobre Next Steps:
- Como edte ejercicio puede ser infinito, por seguir analizando y desgranando los datos, añadir en este apartado lo que creo que podría seguir analizando, filtrando, visualizando... para que al menos quede constancia de que, de no ser por el tiempo, tenía ideas para seguir haciendo.
