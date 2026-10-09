# KNN interactivo

Ejemplo visual del algoritmo k vecinos más cercanos (KNN) en un único archivo HTML, sin dependencias externas.

## Cómo usarlo

1. Descarga `index.html` y ábrelo en cualquier navegador (también funciona publicado como demo, ver más abajo).
2. Elige una clase (A, B o C) y haz clic en el lienzo para añadir puntos.
3. Elige "Añadir k": basta con mover el ratón por el mapa, sin hacer clic, y la k se muestra donde esté el cursor, con las líneas a sus k vecinos, los votos y la predicción. Un clic fija la consulta y otro clic la vuelve a soltar.
4. Ajusta k con el deslizador o con la rueda del ratón y observa cómo cambia la frontera de decisión. Clic derecho borra el punto más cercano. La precisión leave-one-out de la k activa aparece en el panel.
5. Con una consulta marcada se dibuja el radio de la vecindad: un círculo punteado centrado en la consulta cuyo radio es la distancia al k-ésimo vecino; su valor aparece en el panel.
6. La casilla "Vista 3D" cambia a KNN en 3 dimensiones: cada punto tiene una altura z propia (las clases también se separan en altura) y la distancia se calcula con x, y y z. Arrastra para girar 360° (giro completo y vista desde arriba o desde abajo), la rueda cambia k, el doble clic restablece la vista y "Giro automático" rota la escena sola. En 3D la vecindad es una esfera. Los puntos se añaden en la vista 2D.
7. El deslizador "Altura z" ajusta la altura de la consulta en 3D y la de los puntos nuevos que añadas en 2D. A la altura de la consulta se dibuja un corte coloreado de la frontera de decisión.
8. Los botones de la sección Datos permiten cargar el caso real Iris, generar un dataset aleatorio, borrar puntos, quitar la consulta o reiniciar (vuelve al estado inicial: un dataset fijo con 3 zonas separadas en forma de Y y k=3).
9. La interfaz usa tres columnas (lienzo, controles e información) y se adapta al ancho de la ventana.

La precisión leave-one-out indica, para cada punto, si el algoritmo acierta usando los demás puntos del conjunto.

## Caso real: Iris

El botón **Caso real: Iris** carga 150 flores medidas por Fisher (1936), 50 de cada especie (Setosa, Versicolor, Virginica). Fuente: UCI / scikit-learn. Ejes: x = largo del pétalo (cm), y = ancho del pétalo (cm), z = largo del sépalo (cm, solo en 3D). Cada eje se normaliza a [0,1] (mínimo-máximo, con un margen del 9 % en el lienzo) para que pese igual en la distancia. Uso: estimar la especie de una flor a partir de sus medidas comparándola con las 150 conocidas.

Valores medidos con el código de la página; con el ratón las posiciones son aproximadas.

1. Pétalo 1,5 × 0,2 cm: **Setosa** (5 votos de 5 con k = 5).
2. Pétalo 4,6 × 1,4 cm: **Versicolor** (5 de 5). Pétalo 5,8 × 2,0 cm: **Virginica** (5 de 5).
3. Zona ambigua, pétalo 4,8 × 1,8 cm: k = 1 → Versicolor; k = 5 → Virginica (4 contra 1); k = 15 → Virginica (12 contra 3).
4. Precisión leave-one-out (150 flores): k = 1 → 96,0 %; k = 3 → 96,0 %; k = 5 → 96,7 %; k = 9 → 96,0 %; k = 15 → 96,0 %. La diferencia entre k es de una sola flor. Con k = 5 los errores son 0 en Setosa, 2 en Versicolor y 3 en Virginica.
5. En 3D (añade el sépalo), con k = 3 y la consulta en 4,8 × 1,8: sépalo 5,5 cm → Virginica (3 de 3); sépalo 7,0 cm → Versicolor (3 de 3). Precisión leave-one-out en 3D con k = 3: 96,7 %.

## Ejemplo de uso (dataset de demostración)

Valores medidos con el dataset inicial (botón Reiniciar) y el cursor en el punto indicado; en otro punto los números variarán ligeramente.

1. Pulsa **Reiniciar** (60 puntos, 20 por clase, k = 3) y selecciona **Añadir k**.
2. Mueve el ratón por la esquina superior derecha (80 % del ancho, 25 % del alto): predicción **B** con 3 votos de B y radio ≈ 0,12. Con k = 15 sigue siendo B, con los 15 votos.
3. Mueve el ratón al centro, donde se juntan las tres zonas (48 %, 52 %): con k = 1 gana **A**; con k = 3 gana **C** (A = 1, C = 2); con k = 9 gana **A** (A = 4, B = 2, C = 3). Cerca de una frontera la predicción cambia con k: usa la rueda del ratón.
4. Mira "Precisión leave-one-out" en el panel al cambiar k: k = 1 → 86,7 %; k = 3 → 91,7 %; k = 5 → 90,0 %; k = 9 → 88,3 %; k = 15 → 86,7 %. En este dataset k = 3 es la mejor de las probadas.
5. Haz clic para fijar la consulta en el centro y activa **Vista 3D**. Arrastra para girar 360° y mueve **Altura z**: con z = 0,20 la predicción es **A** (3 votos de A); con z = 0,50 es **C** (3 votos de C). Mismas x e y, otra clase: solo cambia la altura. En 3D la precisión leave-one-out con k = 3 es 98,3 %.

## Despliegue en Vercel

Es un sitio estático: un único `index.html`, sin build ni dependencias. Al importar el repositorio en Vercel, deja el preset en "Other" y sin comando de build; la demo queda servida en la raíz.

## Algoritmo

1. Calcular la distancia euclídea entre la consulta y cada punto de entrenamiento: d = sqrt((x1-x2)^2 + (y1-y2)^2). En vista 3D se añade la altura: d = sqrt((x1-x2)^2 + (y1-y2)^2 + (z1-z2)^2).
2. Seleccionar los k puntos con menor distancia.
3. Asignar la clase más frecuente entre esos k vecinos (votación mayoritaria). En caso de empate, gana la clase del vecino más cercano.

## Modelos

Durante la creación se cambió de modelo. Las últimas iteraciones (mejora de la vista 3D y radio de k) se han hecho con Claude Sonnet 5.5.

## Prompts utilizados

Prompts en crudo tal como se escribieron durante la creación del ejemplo:

1. quiero un ejemplo de algoritmo de knn. Que sea sobretodo interactivo, lo tengo q subir a github y con descargar el html se pueda ver el ejemplo de forma visual
2. añade una opcion de reiniciar los pts
3. q el reiniciar cree como los 3 grupos, q no sea completamente blaco. Aparte necesito un readme con los promts en crudo q se han utilizado para crear el ejemplo
4. ahora el reiniciar es lo mismo q un dataset aleatorio
5. q el reinicio sea algo de este tipo, una division simple (con un boceto a mano de un cuadrado dividido en tres zonas desde un centro, con puntos verdes en la zona superior derecha)
6. quiero q me comentes el codigo y me digas cosas a mejorar en este ejemplo
7. añade esos cambios, y aparte q te parece si añadimos la opcion de con el raton poder mover la k para q sea mas visual y aparte añadir mas de una k a la vez
8. añade la opcion de vista 3d
9. y tambien mejora la parte visual de colores
10. mejora la vista 3d y explicame q es esto (con una captura del panel "Ks a comparar": botones del 1 al 15, marcados el 3 y el 5)
11. en el readme añade q ahora he cambiado de modelo
12. metele tambien un radio visual de la cantidad de k seleccionada
13. el modelo 3d tiene q dejar vision 360, en el 3d q no todos los puntos esten en la misma altura, entonces al aplicar el 3d q se puede ajustar la altura de la k
14. luego tambien q no haga falta ir clikando para poner la k, q cambie segun arrastras por el mapa
15. elimina esto (captura del panel "Ks a comparar", con el botón 3 marcado), añade la opcion de al seleccionar añadir la k, q se muestra con solo arrastras por el mapa. Y por ultimo un ejemplo de uso
16. por ultimo cambio los colores de la pagina, un estilo mas oscuro
17. dspues subelo a github y lo conectare a vercel para una demo
18. hazmelo mas conpacto, no podemos dejar ese espacio a la derecha. Lo has relacionado con un ejemplo de uso real? (con una captura de la interfaz oscura con la parte derecha vacía)
