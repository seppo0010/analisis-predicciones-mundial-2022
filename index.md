Por [Sebastián Waisbrot](http://twitter.com/seppo0011)
y [Valeria Edelsztein](http://twitter.com/ValeArvejita)

¿Manija? ¿Quién está manija? Nadie está manija.

Somos solo un país (+ Bangladesh + India) de pie frente a la Scaloneta
pidiéndole que nos traiga la Copa.

Y como hay que sobrevivir a estas horas de incertidumbre y ansiedad de
alguna manera hasta que el reloj marque las 12 del mediodía del domingo
18 de diciembre, elegimos hacerlo con lo que mejor nos sale (?):
¡estadística!

# La pregunta

Se termina Catar 2022 y ~~con él las esperanzas, la alegría, el deseo~~
nos quedan los memes y los resultados. Pero también las predicciones.
Por ejemplo, cierto periodista deportivo pronosticó que Brasil le
ganaría la final a Alemania, mientras Argentina sería la decepción del
torneo (qué interesante, cuéntame más). También matemáticos predijeron
que Brasil le ganaría a Bélgica la final (pues no, mi ciela). Sin
embargo es difícil establecer cuánto se equivocaron, ya que son pocas
predicciones y muy tajantes.

Por suerte, también contamos con predicciones probabilísticas para cada
uno de los partidos de la Copa. Por ejemplo, en
[fivethirtyeight](https://projects.fivethirtyeight.com/2022-world-cup-predictions/)
tenemos cada partido que se disputó y la evolución de las probabilidades
a lo largo de los minutos de juego. Lo mismo con
[301060](https://301060.exactas.uba.ar/), de Exactas UBA,
que tiene predicciones de resultados para cada posible encuentro.

La pregunta, entonces, era obvia (?): ¿cuán buenos fueron los modelos
predictivos?

Lo que pasó a continuación no les sorprenderá (o sí).

Disclaimer: todos los cálculos y datos que se usarán a continuación
están disponibles en
[GitHub](https://github.com/seppo0010/analisis-predicciones-mundial-2022)
y ningún jugador de fútbol fue herido durante el proceso (quizás sí
algunas susceptibilidades, pero eso no es responsabilidad del estadista).

# Lo primero: estimar el fútbol es difícil

El fútbol es un deporte bastante difícil de predecir. No es que estemos
queriendo excusar a quienes hicieron las predicciones, simplemente es lo
que pasa. En este
[hilo](https://twitter.com/pgroisma/status/1602641250842189824)
del matemático Pablo Groisman hay una explicación que se resume en que en fútbol ser
"el doble mejor" que tu rival hace que tus probabilidades de ganar sean
aproximadamente 55%, es decir solamente un 10% por encima del oponente,
mucho menores que en el caso del tenis, por ejemplo.

En este Mundial, a lo largo de los 62 partidos jugados (esto está siendo escrito el sábado 17 de diciembre al mediodía) hubo 163 goles,
es decir, entre 2 y 3 goles por partido, y 37 de ellos (60%) se
resolvieron por una diferencia de un gol o hubo empate, es decir, que un
gol fue determinante en el resultado final.

# Lo segundo: ¿es la probabilidad la herramienta adecuada para esto?

Sí. La probabilidad es una medida de la certidumbre de que ocurra un
evento. El problema es que la probabilidad es, bueno, probabilística, y
peor aún, no es sencillo (¿SIEMPRE TODO ES MÁS COMPLEJO? Depende, es más
complejo). Si tiramos una moneda al aire y decimos que con certeza cae
cara, y efectivamente sale cara, ¿es que somos buenos prediciendo o es
que tuvimos suerte? Si no lo pudiéramos reintentar, nunca lo sabríamos.
Las tiradas de monedas las podemos repetir. Los partidos de fútbol, no.

## Cómo evaluar las predicciones

En este caso, tenemos hechos que no son repetibles. Lamentablemente no
podemos pedirle a Messi que se ponga a pasear a Gvardiol 100 partidos
seguidos a ver en cuántos pierde Croacia. Jackie Siéramos. Pero lo que
sí tenemos son muchos partidos.

Para cada partido, antes de que empiece, podemos estimar la probabilidad
de que gane un equipo, el otro o haya empate. Por ejemplo, en
Catar-Ecuador, fivethirtyeight daba un 24% de chances para los locales,
26% para empate y 50% para Ecuador, mientras que 301060 decía 13%, 21% y
66% respectivamente. El resultado del partido fue 2 a 0 para el equipo
latinoamericano, por lo que la predicción de 301060 fue mejor que la de
fivethirtyeight en este caso, ya que *arriesgó* más hacia el resultado
correcto\*

\*Aclaración: cuanto más arriesgada es una predicción, más información
sobre el mundo nos da y más interesante es. Si cuando empezó la Copa
decíamos algo como "El Mundial lo va a ganar alguno de los 32
seleccionados que lo disputan" no estábamos siendo muy arriesgados que
digamos; en cambio, si decíamos "Catar va a ganar el Mundial" era una
predicción mucho más interesante (y un delirio).

### Puntaje de Brier

Para poder, entonces, comparar los poderes predictivos de cada modelo
necesitamos calcular cuánto mejor es uno frente a otro para cada partido
de alguna forma sistemática. Usemos el [calificador de
Brier](https://en.wikipedia.org/wiki/Brier_score) para
darle un número a cuánto mejor es una estimación sobre otra.

Este método es el que se utiliza más comúnmente para medir la precisión
de un pronóstico, especialmente cuando el resultado es binario. Por
ejemplo, supongamos que el pronóstico del estado del tiempo dice que
tenemos un 90% de probabilidades de lluvia y, al final, llueve. Podemos
calcular el puntaje de Brier para este pronóstico como (f -- o)² donde
f es la probabilidad pronosticada (0,9) y o es el resultado (1 si el
evento ocurre, 0 si no ocurre). En este caso: (0,9 -- 1)² = (-0,1)² =
0,01.

Por la manera en la que se calcula, como una estimación del error, el
puntaje de Brier puede tomar valores entre 0 y 1. Cuanto menor sea el
puntaje, menor error y más preciso será el pronóstico: un pronóstico
perfecto obtiene un puntaje de 0. Si queremos calcular el puntaje de
Brier para un conjunto de pronósticos tendremos que hacer el promedio de
los puntajes de Brier para cada pronóstico individual.

Para el caso del partido Catar-Ecuador, el puntaje de Brier es 0.1766
((0.66-1)²+0.13²+0.21²) para 301060 y 0.3752
((1-0.50)²+0.24²+0.26²) para fivethirtyeight, es decir, mucho
mejor para el modelo de Exactas (AWANTE).

### Coeficiente

El puntaje de Brier hasta acá es una estimación que cuantifica cuánto
error hubo. Para sacar algo más "palpable" (y más intuitivo), vamos a
comparar este puntaje con el que obtendría un modelo base que en fase de
grupos predice un 33% para cada equipo o empate y en eliminatorias un
50% de que pasa cada uno (es decir, una situación completamente al
azar). Así, vamos a obtener lo que llamamos "coeficiente": el
coeficiente para una predicción perfecta quedaría en 1, predecir tan
bien como el azar es 0 y hasta podría ser negativo de ser peor.

## Un obstáculo: empates y eliminatorias

Algo muy importante: algunas predicciones consideraban que a partir de
octavos, los partidos eran eliminatorios y, por lo tanto, debía pasar
uno u otro equipo así que no consideraban los empates, mientras que
otras se basaban únicamente en los 90 minutos y sí los permitían. Para
que sea justo el sistema de puntaje, es necesario "medir" a todo con la
misma vara, es decir, pasar todos los modelos al mismo sistema. Optamos
por distribuir la probabilidad de empate entre los dos equipos. Esta
distribución puede ser en partes iguales o proporcionalmente (o de
cualquier otra forma, pero estas son las que más sentido tienen).
Decidimos hacerlo en partes iguales porque el tiempo extra es bastante
distinto a los primeros 90 minutos (los jugadores están más cansados, la
presión es mayor, hay menos "juego"), y los penales son literalmente
otro juego. Entonces, en el caso de Catar-Ecuador, si fueran a jugar
penales (aunque obviamente no aplica porque era un partido de fase de
grupos) las probabilidades de 24/26/50 serían 37/63 y las de 13/21/66
pasarían a 23.5/76.5.

Igual no nos convence del todo, pueden contarnos si se les ocurre una
idea mejor.

# Lo tercero: datos

Los datos usados fueron obtenidos de
[fivethirtyeight](https://projects.fivethirtyeight.com/2022-world-cup-predictions/),
[306010](https://301060.exactas.uba.ar/manoamano.htm),
[oddspedia](https://oddspedia.com/es/futbol/mundo/copa-mundial)
y [fbref](https://fbref.com).

## Apuestas y probabilidades

Una apuesta lleva implícita una probabilidad. Antes de la final
Argentina-Francia, bet365 paga 2.80 por una victoria de Francia, 3.00
por tiempo extra y 2.80 por Argentina, por lo que *a ojo* notamos que
las probabilidades están divididas en partes iguales.

Sin embargo, en BC.Game pagan 2.80/3.15/2.90, es decir que Argentina es
levemente preferido, pero la pregunta es por cuánto. La respuesta es
bastante sencilla: el inverso de cada uno, es decir 1/2.8 (36%), 1/3.15
(32%) y 1/2.9 (34%). El lector atento (ahre) notará que esto suma más de
100%. Lo que está por encima de eso es la comisión de la casa de
apuestas, así que hay que *normalizar*, es decir, calcular la proporción
sobre 100%, con lo que queda 35%, 31% y 34% (nadie se juega mucho, como
verán).

## ¡Aprender!

Las predicciones de 301060 estaban determinadas antes de que empiece la
competición, mientras que el resto pudo "aprender" sobre la marcha, ya
sea ver qué equipos estaban superando las expectativas, o qué jugadores
se lesionaron o fueron suspendidos. Esto les debería dar cierta ventaja.

El ganador es...

¡Ya comete la maldita naranja!

| 301060                             |0.1685|
| lilibet                            |0.0989|
| weltbet                            |0.0982|
| bodog                              |0.0952|
| 20bet                              |0.0940|
| ivibet                             |0.0938|
| bet365                             |0.0936|
| jackbit                            |0.0926|
| goldenbet                          |0.0926|
| 31bet                              |0.0926|
| mystake                            |0.0924|
| freshbet                           |0.0923|
| bc-game-sport                      |0.0922|
| fivethirtyeight                    |0.0858|

¡Felicitaciones a la gente de Exactas! Mate, dulce de leche, colectivo, birome y
modelos predictores de Copas del Mundo. Al resto: "Andá pa\' allá bobo".

## Nos calmemos (o "abriendo el paraguas")

¿Significa este resultado que las predicciones de Exactas son mejores?
Bueno, más o menos. El sistema de puntaje que elegimos determina cuánto
penalizamos el riesgo que no se concreta o premiamos la osadía exitosa,
pero no existe una respuesta unívoca de qué es lo que corresponde en
esta circunstancia.

Las casas de apuestas están predeciblemente muy cerca entre ellas ya
que, como funcionan como un mercado, se equilibran con la demanda.

De los 64 partidos, 301060 (el mejor) y fivethirtyeight (el peor)
eligieron al mismo ganador probable en 58. Las seis diferencias:

|               | fivethirtyeight  |       | 301060    |           | Resultado
|--|--|--|--|--|
| Marruecos-Croacia          | 32%      | 39%   | 43%       | 28%       | 0-0
| Gales-Iran    | 39%      | 30%   | 25%       | 47%       | 0-2
| Túnez-Australia            | 39%      | 29%   | 35%       | 37%       | 0-1
| Croacia-Bélgica            | 39%      | 33%   | 28%       | 48%       | 0-0
| Inglaterra-Francia         | 52%      | 48%   | 50%       | 50%       | 1-2
| Croacia-Marruecos (tercer puesto)          | 53%      | 47%   | 43%       | 57%

# El bonus-track nuestro de cada día

## Partidos más sorprendentes

Los resultados más inesperados del mundial (en orden descendiente):

-   Argentina 1 - Arabia Saudita 2 (fingiremos demencia)

-   Camerún 1 - Brasil 0

-   Australia 1 - Dinamarca 0

-   Croacia 1 (4) - Brasil 1 (2)

-   Japón 2 - España 1

## Comparación con Rusia 2018

Lamentablemente no contamos con todas las mismas predicciones de
mundiales pasados, pero, por suerte (con qué poco nos divertimos),
[fivethirtyeight](https://projects.fivethirtyeight.com/2018-world-cup-predictions/)
sí las tiene. En el mundial pasado, el mismo coeficiente que ahora le da
0.0858 le daba 0.1790 por lo que o bien empeoró mucho el modelo, o bien
hubieron muchas más sorpresas en Catar.

Las sorpresas de ese Mundial:

-   Corea del Sur 2 - Alemania 0

-   España 1 (3) - Rusia 1 (4)

-   Alemania 0 - México 1

-   España 2 - Marruecos 2

-   Argentina 1 - Islandia 1 (fingiremos demencia otra vez)

Y hasta acá llegamos, palpitando la final del Mundial, unidos en el
grito que no deja grieta alguna (y si la deja no queremos saberlo):
"Muchachos, ahora nos volvimos a ilusionar".
