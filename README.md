# proyecto-casino-1
# Análisis de Rentabilidad y Comportamiento de Usuarios - iGaming (Bustabit)

Contexto del Negocio
En la industria del juego online (*iGaming*), entender el comportamiento del usuario y controlar el riesgo de la casa es vital para la estabilidad financiera de la plataforma. Este proyecto analiza un dataset de 50.000 apuestas reales con el objetivo de evaluar la salud financiera del casino, identificar patrones de comportamiento según el reloj y proponer mejoras en la gestión operativa y comercial.

 Problemas Solucionados
Desde una perspectiva técnica y de ingeniería, los datos crudos presentaban desafíos estructurados que fueron resueltos en la etapa de ETL (*Extract, Transform, Load*) usando **Power Query**:
* **Tratamiento de Nulos (Data Cleaning):** Se detectó que las columnas de ganancias (`Profit`) y retiros (`CashedOut`) contenían valores nulos (`null`) cuando el jugador perdía. Se estandarizaron reemplazando los nulos por `0` para evitar errores y distorsiones en las funciones de agregación matemática (`SUM`, `AVERAGE`).
* **Tipado de Datos:** Se corrigieron los tipos de datos de las variables clave (`Profit`, `Bet`, `CashedOut`), migrándolos de formato texto a formato numérico (*Decimal/Entero*), habilitando el uso de operadores lógicos relacionales.
* **Ingeniería de Características de Tiempo (Time Splitting):** La fecha original venía en formato de texto ISO 8601 (`2016-11-20T19:44:19Z`). Se transformó a tipo *Fecha/Hora* y se extrajo la **Hora entera (0-23)** en una columna independiente para aislar el factor tiempo del calendario.
* **Estructuración Lógica Condicional:** Usando lógica condicional, se crearon categorías personalizadas:
  * **Columna `Resultado`:** `IF Profit > 0 THEN "Ganó" ELSE "Perdió"`.
  * **Columna `Franja Horaria`:** Agrupamiento de las 24 horas en bloques de gestión operativa (*Madrugada, Mañana, Tarde, Noche*).

 Métricas de Negocio & Análisis DAX
Se implementó un modelo de datos analítico en el lienzo principal implementando las siguientes soluciones:
1. **Control de Flujo de Caja:** Tarjetas dinámicas para el total de dinero ingresado (Suma de `Bet`) frente al total de premios otorgados (Suma de `Profit`).
2. **Medida DAX - Margen de la Casa (*House Edge*):** Se desarrolló una métrica personalizada para calcular la rentabilidad real de la plataforma:
   ```DAX
   Margen Casa = ( SUM(bustabit[Bet]) - SUM(bustabit[Profit]) ) / SUM(bustabit[Bet])

El análisis arrojó un margen general del 67.11%, demostrando una alta retención para este tipo de juego.

Ranking de Usuarios de Alto Valor (Whales): Gráfico de barras horizontales optimizado con filtrado Top 10 mediante valor de ordenamiento por volumen de juego.

Propuestas de Gestión y Optimización (Business Intelligence)
Activación de Horas Muertas: El análisis de la línea de tiempo identificó un estancamiento crítico a las 16:00 hs. Se sugiere implementar promociones automatizadas de créditos o bonos específicos en esa franja para estimular la demanda latente.

Estrategia de Anclaje Nocturno: Se detectó el pico de volumen de juego a las 23:00 hs. Se propone coordinar eventos de entretenimiento en vivo (recitales, shows) en esa franja horaria para potenciar el flujo de clientes hacia el sector analógico/bar del establecimiento físico, apalancando la predisposición al ocio detectada en los datos.
