# Machine learning and Distributionaly Robust

---
abstract: |
    Este trabajo investiga cómo los enfoques estocásticos implementados en
    los problemas de planificación energética bajo incertidumbre y las
    hipótesis de distribuciones consideradas en los parámetros inciertos
    impactan las decisiones estratégicas de inversión. Basado en un modelo
    del sistema energético suizo, implementamos un enfoque de programación
    estocástica en dos etapas, asumiendo diferentes distribuciones para los
    parámetros inciertos más importantes según un análisis de sensibilidad
    previa. Luego, comparamos las distintas soluciones estocásticas
    calculadas con soluciones publicadas en la literatura que usan el
    enfoque de la optimización robusta. El impacto del enfoque y de la
    distribución es tal sobre la decisión de inversión que concluimos a la
    necesidad de usar las técnicas de optimización robusta en distribución
    en nuestro trabajo futuro.
author:
- 
title: |
    Planificación energética bajo incertidumbre: importancia de los enfoques
    e hipótesis estocásticos en las soluciones robustas
---

Introducción
============

La planificación de los sistemas de energía propone una estrategia
óptima de inversión en tecnologías eficientes y qué recursos serán
necesarios para satisfacer la demanda futura. Los modelos de
planificación son herramientas muy importantes para el análisis de los
sistemas energéticos ya que a través de ellos se puede garantizar el
suministro de energía de manera asequible, competitiva, segura,
sostenible y confiable para el crecimiento y desarrollo de un país o
comunidad. Una característica inherente en estos modelos es la carencia
de datos fiables y presencia de errores de medidas en los parámetros de
entrada, lo que ha hecho que estos sistemas sean extremadamente
difíciles de analizar. Diferentes enfoques de optimización se han
utilizado como herramientas para dar soporte al proceso de toma de
decisiones. La Programación Estocástica (SP) ha sido muy utilizada para
resolver modelos de optimización bajo incertidumbre [@Prekopa1995]. Bajo
este enfoque, uno o más parámetros inciertos pueden ser modelados a
través de variables aleatorias. En un problema de dos etapas, las
decisiones en la primera etapa son tomadas con los datos disponibles en
el momento, antes de observar la realización de la variable aleatoria
("here and now"); en la segunda etapa, una vez que la información del
parámetro incierto es revelado ("wait and see"), entonces se toman
acciones correctivas. La SP aplicada a modelos de planificación
energética, ha demostrado superioridad frente a los modelos
determinísticos [@Usher2012]. Sin embargo, las limitaciones presentes en
la SP son: el conocimiento de la verdadera distribución de los
parámetros y el problema de la dimensionalidad (incremento de etapas y
escenarios). En muchos casos afrontar problemas reales sigue siendo
imposible, pero gracias a los avances de la computación se han podido
resolver problemas que habían sido impensables. Por otro lado, la
Optimización Robusta (RO) ha tomado mucho interés en los últimos años
gracias al trabajo de [@Ben-Tal1998], además, ésta no requiere una
distribución específica para cada parámetro incierto, en su lugar, se
asume que los datos inciertos varían en un conjunto de incertidumbre
determinístico, el cual ha sido especificada por el usuario. La RO
adopta un enfoque min-máx que aborda la incertidumbre, de manera que se
pueda garantizar factibilidad ante cualquier realización de los
parámetros inciertos dentro del conjunto de incertidumbre
[@Ben-Tal2015]. Sin embargo, es conocido que RO puede generar soluciones
muy conservadoras, el cual naturalmente incrementa el costo de las
soluciones [@Bertsimas2004]. En este trabajo implementamos el enfoque de
programación estocástica en dos etapas para un modelo de energía de gran
escala y comparamos las soluciones estocásticas con soluciones robustas
publicadas en la literatura [@MORET2019]. A continuación, introducimos
el problema de energía y nuestra metodología. Luego, discutimos los
resultados de la investigación y trabajo en desarrollo.

Problema a estudiar
===================

Una formulación MILP para la planificación energética fue presentada por
[@MORET2019], donde se incorpora información de la demanda de uso final
(electricidad, calefacción y transporte), la eficiencia y costo de las
tecnologías, el costo de los recursos (importados y locales) y su
disponibilidad; también se consideran unidades de almacenamiento. Éste
es un modelo completo y una representación simplificada del sistema de
energía nacional de Suiza, donde considera la evolución del sistema
energético hasta el año 2035, con una formulación de un solo periodo y
que toma en cuenta la estacionalidad del año por meses. Un esquema
general del modelo de planificación energética, puede ser representado a
través de la siguiente estructura:

[\[eq:MILP\]]{#eq:MILP label="eq:MILP"} $$\begin{aligned}
\min_{x \in X} \quad c^T\textbf{x} + e^T\textbf{y} \\
\textrm{s.a.}\quad A\textbf{x} & \leq b \label{eq:1}\\
T\textbf{x} + W\textbf{y} & \geq d \label{eq:2}\\
\textbf{x} &\in X \\
\textbf{y} &\in Y \end{aligned}$$

Donde la variable **x** representa las decisiones de inversión, el
conjunto
$X \subseteq  \mathbb{R}_{+}^{n_1 - p} \times \mathbb{Z}_{+}^{p}$ impone
restricciones relacionadas a la naturaleza de las variables (continuas y
enteras). La variable **y** representa las decisiones de operación, y el
conjunto $Y \subseteq \mathbb{R}_{+}^{n_2}$ son restricciones de no
negatividad. Además, $n_1$, $n_2$ y $p$ son enteros positivos con
$p < n_1$.\
El objetivo del problema es minimizar el costo total de inversión y
operación. El primer término de la función objetivo define la inversión
anualizada y costo de mantenimiento para cada tecnología y el segundo
término define el costo de operación de los recursos en un periodo. La
restricción ([\[eq:1\]](#eq:1){reference-type="ref" reference="eq:1"})
representa de manera simplificada varias restricciones del sistema que
no dependen de la variable de operación, como: la capacidad existente,
la máxima capacidad que puede ser instalada para cada tecnología y
adicionales especificaciones del sistema tales como redes eléctricas y
de calefacción descentralizada. La restricción
([\[eq:2\]](#eq:2){reference-type="ref" reference="eq:2"}) representa
todas aquellas restricciones que dependen de la variable de operación,
entre ellas podemos mencionar el factor de capacidad anual y mensual, la
disponibilidad de los recursos importados y locales, el balance de la
generación y consumo, y también la operación de las unidades de
almacenamiento. En el problema
([\[eq:MILP\]](#eq:MILP){reference-type="ref" reference="eq:MILP"}), la
incertidumbre fue considerada en: el vector *c*, el cual contiene el
costo de inversión, el costo de mantenimiento, la tasa de descuento y el
tiempo de vida de cada tecnología; el vector *e* donde están los costos
de los recursos y en el vector *d* donde se encuentra la demanda de uso
final. Por último, el parámetro de eficiencia de la tecnología de
conversión de energía es también considerada como incierta y se
encuentra dentro de la matriz *W*. Para una mejor comprensión de los
parámetros referimos al lector ver [@MORET2017a].

Programación estocástica en dos etapas
--------------------------------------

El problema ([\[eq:MILP\]](#eq:MILP){reference-type="ref"
reference="eq:MILP"}) en su versión de programación estocástica en dos
etapas, selecciona alguna decisión inicial (**x**) que minimice el costo
actual más el costo esperado de la variable recurso (**y**). Una
estándar forma del modelo estocástico en dos etapas es el siguiente:

$$\label{eq:Stoch}
\begin{array}{rrclcl}
\displaystyle \min_{x \in X} & \multicolumn{3}{l}{c^T\textbf{x} + \displaystyle \mathbb{E}[Q(\textbf{x},\xi)]} \\
\textrm{s.a.} & A \textbf{x} & \leq & b \\
\end{array}$$

y $Q(\textbf{x},\xi)$ es la función recurso:

$$Q(\textbf{x},\xi) := \min_y \{e(\xi)^T\textbf{y}: W(\xi)\textbf{y}\geq d(\xi)-T(\xi)\textbf{x}, \textbf{y}\in \mathit{Y}\}$$

donde $\mathbb{E}$ es la esperanza, y $\xi$ es un vector aleatorio en
$\mathbb{R}^k$ que denota un escenario o posible resultado con respecto
al espacio de probabilidad $(\Omega,\mathcal{F},\mathbb{P})$. El
problema en la primera etapa compromete a
$A \in \mathbb{R}^{m_1\times n_1}$, $b \in \mathbb{R}^{m_1}$, y
$c \in \mathbb{R}^{n_1}$. En la segunda etapa se tiene a la matriz
tecnológica $T(\xi) \in \mathbb{R}^{m_2\times n_1}$, la matriz de
recurso $W(\xi) \in \mathbb{R}^{m_2 \times n_2}$, el vector
$d(\xi) \in \mathbb{R}^{m_2}$, y el vector de costo
$e(\xi) \in \mathbb{R}^{n_2}$. Además, la matriz $W$ es de recurso
parcial, es decir, no siempre las decisiones de la segunda etapa son
factibles para las decisiones de la primera etapa.

Método de muestreo
------------------

El enfoque de "Sample Average Approximation" (SAA) puede ser utilizado
como un método de muestreo para resolver problemas de optimización
estocástica a través de simulación de Monte Carlo. Esta técnica es
utilizada por [@Shapiro1998], donde el costo esperado de la función
recurso es aproximado por un promedio de muestras derivadas de muestras
aleatorias. Sea $(\xi_i)_{i=1}^N$ un conjunto de $N$ muestras generadas
de $\Omega$ de acuerdo con la probabilidad $\mathbb{P}$, y sea
$Q(x,\xi_i)$ el costo de la función recurso para la realización $\xi_i$,
entonces el costo del valor esperado es aproximado por el promedio de
las realizaciones:

$$\mathbb{E}[Q(\textbf{x},\xi)] \approx \frac{1}{N} \sum_{i=1}^NQ(\textbf{x},\xi)$$

De modo que, para un $N$ muy grande la solución SAA será una buena
aproximación de la solución óptima verdadera. Además, la estructura en
forma de L es explotada, al descomponer el problema completo en
subproblemas más pequeños a través de descomposición de Benders.

Avances de la investigación
===========================

Partiendo del modelo ([\[eq:MILP\]](#eq:MILP){reference-type="ref"
reference="eq:MILP"}) presentado en la sección
[2](#sec:MILP){reference-type="ref" reference="sec:MILP"}, hemos
implementado un modelo de programación estocástica en dos etapas,
tomando en cuenta la incertidumbre en casi todos los parámetros
inciertos propuestos en [@MORET2017a], y resolvemos vía descomposición
de Benders. En esta sección, iniciaremos explicando las distribuciones
asumidas sobre los parámetros, y después, el análisis de los resultados
obtenidos hasta ahora.

Distribuciones asumidas
-----------------------

Para aplicar la SP, se necesita el conocimiento de la verdadera
distribución de los parámetros inciertos, la cual es difícil de obtener
en practica. Pero gracias al trabajo de [@MORET2017a], tenemos
información de los rangos y el valor nominal de los parámetros
inciertos, sobre los cuales podemos elegir una distribución que cumpla
con las características estadísticas dadas. Sin embargo, deseando
observar el impacto de la distribución asumida, se propone hacer el
cambio de la distribución para aquellos parámetros más influyentes del
modelo. Gracias a previo análisis de sensibilidad, se concluye que los
parámetros inciertos más importantes del modelo se encuentran en los
costos de segunda etapa (vector **e**), y especialmente el precio del
gas natural. Por lo tanto, distinguimos para los costos de los recursos
distribuciones asociados a los siguientes supuestos:

-   Cuando el valor nominal (valor más probable) es igual a la mediana,
    entonces se asume que los parámetros se distribuyen de manera
    uniforme.

-   Cuando el valor nominal es menor que la mediana, en este caso, se
    sugiere el uso de la lognormal como la distribución de los
    parámetros.

Resultados hasta ahora
----------------------

El modelo ([\[eq:Stoch\]](#eq:Stoch){reference-type="ref"
reference="eq:Stoch"}) es transformado a un "Big-MILP" por medio de SAA,
con N = 1500 escenarios, y es resuelto vía descomposición de Benders. A
continuación, mencionaremos 5 criterios utilizados para obtener los
resultados:

-   *Deterministic*, el cual no considera la incertidumbre y asume el
    valor nominal para todos los parámetros.

-   *Worst-case*, asume que todos los parámetros inciertos tomarán su
    valor de peor caso, como en [@Soyster1973].

-   *Robust*, es la solución definida por aplicar el enfoque robusto de
    [@MORET2019].

-   *Stochastic*, donde se asume que todos los parámetros inciertos
    tienen una distribución uniforme.

-   *Stochastic\**, se asume que solo los costos de los recursos tienen
    una distribución lognomal.

Impacto de la distribución en la capacidad instalada
----------------------------------------------------

La capacidad instalada ($\tilde{x}$) representa la producción máxima de
electricidad que un generador puede producir en condiciones ideales. A
continuación, proporcionamos a través de cincos soluciones: (1)
determinística, (2) robustas y (2) estocásticas, como está configurada
la generación de electricidad del sistema.

\centering
![Capacidades instaladas para el suministro del sector
eléctrico.[]{label="Figura:1"}](output1.png){#Figura:1 width="2.5in"}

En los resultados notamos, que la solución *Stochastic* suple la
producción de energía por medio de fuentes renovables y combustibles
fósiles, mientas que la solución *Stochastic\** es totalmente dominada
por uso de gas natural y además es muy parecida a la solución
*Deterministic*. Esto se debe a que la distribución lognormal da mayor
probabilidad a costos bajos. Este es un ejemplo claro de que la
distribución aproximada puede no representar verdaderamente la
incertidumbre subyacente, generando soluciones suboptimas y posiblemente
indeseables. El primer estudio que hicimos, nos permite concluir que la
distribución elegida tiene un impacto muy importante en la solución del
problema, lo que motiva a utilizar técnicas de optimización robusta en
distribución.

Trabajo en desarrollo
=====================

En el problema estudiado, la distribución elegida para los costos de los
recursos tiene un gran impacto de la solución estocástica. Para superar
la necesidad de comprometer los parámetros inciertos a una estimada
distribución, se propone usar las técnicas de optimización robusta en
distribución. El enfoque fue planteado por [@Scarf1958], donde después
de definir un conjunto $\mathcal{D}$ de distribuciones de probabilidad
que se supone que incluye la distribución real ($\mathbb{P}$), la
función objetivo de ([\[eq:Stoch\]](#eq:Stoch){reference-type="ref"
reference="eq:Stoch"}) se modifica de tal manera que se obtenga la peor
distribución en el conjunto de distribuciones. Por lo tanto, esto lleva
a resolver un programa estocástico y robusto en distribución (DRO):
$$\begin{array}{rrclcl}
\displaystyle \min_{x \in X} & \multicolumn{3}{l}{c^T\textbf{x} + \displaystyle \max_{\mathbb{P} \in \mathcal{D}}\mathbb{E}_{\mathbb{P}}[Q(\textbf{x},\xi)]} \\
\textrm{s.a.} & A \textbf{x} & \leq & b \\
\end{array}$$

donde $Q(\textbf{x},\xi)$ en la función recurso, $\xi$ es un vector
aleatorio en $\mathbb{R}^k$, $\mathbb{P}$ es la distribución de $\xi$,
$\mathcal{D}$ es el conjunto de incertidumbre para esta distribución, y
$X$ es la región factible de la variable de decisión $x$. Este enfoque
propone proteger las decisiones tomadas del riesgo y la ambigüedad de la
distribución subyacente de la incertidumbre. En esta presentación,
mostraremos los resultados preliminares obtenidos con DRO.

Agradecimientos {#agradecimientos .unnumbered .unnumbered}
===============

Este trabajo ha sido financiado en parte por el proyecto FONDECYT
1171145.

1

\bibitem{Prekopa1995}
Prékopa, András. *Stochastic Programming*. Springer Netherlands, 1995.

\bibitem{Usher2012}
Will Usher y Neil Strachan. *Critical mid-term uncertainties in
long-term decarbonisation pathways*. Energy Policy, 41:433--444, 2012.

\bibitem{Ben-Tal1998}
Ben-Tal, A. and Nemirovski, A. *Robust Convex Optimization*. Mathematics
of Operations Research, 23(4):769-805 1998.

\bibitem{Ben-Tal2015}
Ben-Tal, Aharon y den Hertog, Dick y Vial, Jean-Philippe. *Deriving
robust counterparts of nonlinear uncertain inequalities*. Mathematical
Programming, 149(1-2):265-299, 2015.

\bibitem{Bertsimas2004}
Bertsimas, Dimitris y Sim, Melvyn. *The Price of Robustness*. Operations
Research, 52(1):35-53, 2004.

\bibitem{MORET2019}
Stefano Moret y Frédéric Babonneau y Michel Bierlaire y François
Maréchal. *Decision support for strategic energy planning: A robust
optimization framework*. European Journal of Operational Research,
280(2):539-554, 2020.

\bibitem{MORET2017a}
Stefano Moret y Víctor Codina Gironès y Michel Bierlaire y François
Maréchal. *Characterization of input uncertainties in strategic energy
planning models*. Applied Energy, 202:597-617, 2017.

\bibitem{Shapiro1998}
Shapiro, Alexander y Homem-de-Mello, Tito. *A simulation-based approach
to two-stage stochastic programming with recourse*. Mathematical
Programming, 81(3):301-325, 1998.

\bibitem{Soyster1973}
Soyster, A. L. *Convex Programming with Set-Inclusive Constraints and
Applications to Inexact Linear Programming*. Operations Research,
21(5):1154-1157, 1973.

\bibitem{Scarf1958}
Scarf, H. *A Min-Max Solution of an Inventory Problem*. In: Studies in
the Mathematical Theory of Inventory and Production, Stanford University
Press, Redwood City, CA, 201-209, 1958.
