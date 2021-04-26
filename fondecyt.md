# Machine learning and Distributionaly Robust

## Abstrat
  
Este trabajo investiga cómo los enfoques estocásticos implementados en los problemas de planificación energética bajo incertidumbre y las hipótesis de distribuciones consideradas en los parámetros inciertos impactan las decisiones estratégicas de inversión. Basado en un modelo del sistema energético suizo, implementamos un enfoque de programación estocástica en dos etapas, asumiendo diferentes distribuciones para los parámetros inciertos más importantes según un análisis de sensibilidad previa. Luego, comparamos las distintas soluciones estocásticas calculadas con soluciones publicadas en la literatura que usan el enfoque de la optimización robusta. El impacto del enfoque y de la distribución es tal sobre la decisión de inversión que concluimos a la necesidad de usar las técnicas de optimización robusta en distribución en nuestro trabajo futuro.

## Introduccion

La planificación de los sistemas de energía propone una estrategia óptima de inversión en tecnologías eficientes y qué recursos serán necesarios para satisfacer la demanda futura. Los modelos de planificación son herramientas muy importantes para el análisis de los sistemas energéticos ya que a través de ellos se puede garantizar el suministro de energía de manera asequible, competitiva, segura, sostenible y confiable para el crecimiento y desarrollo de un país o comunidad.
Una característica inherente en estos modelos es la carencia de datos fiables y presencia de errores de medidas en los parámetros de entrada, lo que ha hecho que estos sistemas sean extremadamente difíciles de analizar. Diferentes enfoques de optimización se han utilizado como herramientas para dar soporte al proceso de toma de decisiones.
La Programación Estocástica (SP) ha sido muy utilizada para resolver modelos de optimización bajo incertidumbre (Prekopa1995). Bajo este enfoque, uno o más parámetros inciertos pueden ser modelados a través de variables aleatorias.  En un problema de dos etapas, las decisiones en la primera etapa son tomadas con los datos disponibles en el momento, antes de observar la realización de la variable aleatoria ("here and now"); en la segunda etapa, una vez que la información del parámetro incierto es revelado ("wait and see"), entonces se toman acciones correctivas. La SP aplicada a modelos de planificación energética, ha demostrado superioridad frente a los modelos determinísticos [Usher2012]. Sin embargo, las limitaciones presentes en la SP son: el conocimiento de la verdadera distribución de los parámetros y el problema de la dimensionalidad (incremento de etapas y escenarios). En muchos casos afrontar problemas reales sigue siendo imposible, pero gracias a los avances de la computación se han podido resolver problemas que habían sido impensables. Por otro lado,
la Optimización Robusta (RO) ha tomado mucho interés en los últimos años gracias al trabajo de (Ben-Tal1998), además, ésta no requiere una distribución específica para cada parámetro incierto, en su lugar, se asume que los datos inciertos varían en un conjunto de incertidumbre determinístico, el cual ha sido especificada por el usuario. La RO adopta un enfoque min-máx que aborda la incertidumbre, de manera que se pueda garantizar factibilidad ante cualquier realización de los parámetros inciertos dentro del conjunto de incertidumbre (Ben-Tal2015). Sin embargo, es conocido que RO puede generar soluciones muy conservadoras, el cual naturalmente incrementa el costo de las soluciones (Bertsimas2004). En este trabajo implementamos el enfoque de programación estocástica en dos etapas para un modelo de energía de gran escala y comparamos las soluciones estocásticas con soluciones robustas publicadas en la literatura (MORET2019). A continuación, introducimos el problema de energía y nuestra metodología. Luego, discutimos los resultados de la investigación y trabajo en desarrollo.

## Problema

Una formulación MILP para la planificación energética fue presentada por \cite{MORET2019}, donde se incorpora información de la demanda de uso final (electricidad, calefacción y transporte), la eficiencia y costo de las tecnologías, el costo de los recursos (importados y locales) y su disponibilidad; también se consideran unidades de almacenamiento. Éste es un modelo completo y una representación simplificada del sistema de energía nacional de Suiza, donde considera la evolución del sistema energético hasta el año 2035, con una formulación de un solo periodo y que toma en cuenta la estacionalidad del año por meses. Un esquema general del modelo de planificación energética, puede ser representado a través de la siguiente estructura:
\begin{subequations}\label{eq:MILP}
\begin{align}
\min_{x \in X} \quad c^T\textbf{x} + e^T\textbf{y} \\
\textrm{s.a.}\quad A\textbf{x} & \leq b \label{eq:1}\\
T\textbf{x} + W\textbf{y} & \geq d \label{eq:2}\\
\textbf{x} &\in X \\
\textbf{y} &\in Y 
\end{align}
\end{subequations}
