
Archivos generados por el sistema para cada planta:

- planta_X/datos.csv: Contiene los datos históricos descargados para la planta X. Incluye las variables relevantes y sus valores en formato CSV.
	- Estructura típica:
		- La primera columna corresponde a la fecha y hora de la medición (`date_time` o similar).
		- Las siguientes columnas corresponden a cada variable medida en la planta (por ejemplo: caudal, pH, temperatura, etc.).
		- Cada fila representa una medición en un instante de tiempo.
		- Ejemplo:
			```
			date_time,variable_1,variable_2,variable_3,...
			2024-08-10 00:00:00,valor_1,valor_2,valor_3,...
			2024-08-10 00:01:00,valor_1,valor_2,valor_3,...
			...
			```

- planta_X/descripcion.csv: Contiene la descripción de las variables incluidas en el archivo de datos de la planta X. Incluye información como nombre, unidad, tipo, periodicidad y valores críticos/advertencia.

(X corresponde al número o nombre de la planta)
