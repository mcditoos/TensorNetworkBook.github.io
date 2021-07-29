Capítulo 1
============================




```{epigraph}
**"Being ignorant is not so much a shame, as being unwilling to learn."**<br />

 -- Benjamin Franklin

```



## Introduction

  

The concept of tensor networks is a relatively young one that has its origin in the graph notation proposed by Penrose, which began to be applied in practical problems in 1971 {cite}`penrose_1971`. However, its stayed otside of the popular topics of conversation in science until Deutsch used a diagrammatic notation derived from it to formulate what he called "Quantum Computational Networks" that are now known as quantum circuits {cite}`D1989`. That's right! The circuits you build in [Qiskit](qiskit.org) are but a special case of tensor networks. A fun fact is that while the most popular model was made by Deutsch, Feynman had also formulated his own diagrammatic model related to the former just a few years earlier {cite}`Feynman1986`. But even after all this, tensor networks were rarely a topic of conversation. 

The interest resurfaced thanks to the use of the theory of tensor networks in numerical computations, with the rise of algorithms like MPS, TT, TTN, MERA and PEPS. Some of them have their origin in condensed matter and were meant to be used problems in quantum mechanics, somewhat inspired in the density matrix renormalization group. However, these numerical techniques also found use in machine learning, due to the fact that just like in condensed matter, the numerical problems are high-dimensional. This even drove companies like Google into investing in research in the area, as well as the development of libraries used for numerical computations of these techniques like [TensorNetwork](https://github.com/google/TensorNetwork).

A tensor network is a countable array of tensors connected by contractions (sums over indices) between them and not only has it helped in the theorical comprehension of wave functions in quantum mechanics, but they also form the basis for many powerful approaches to numerical simulation. So... ¿What is this book about? The following texts is mainly concerned in what is commonly called "tensor network methods" whiche is the set of tools associated with the computation and simplification of tensor networks. These methods, on top of being used in the aforementioned areas of physics of condensed matter {cite}`1308.3318` and machine learning {cite}`NIPS2015_6855456e`, they are also present in quantum information theory {cite}`PhysRevLett.125.190402`, open quantum systems {cite}`PhysRevLett.121.227401`, quantum gravity {cite}`Chirco_2018` and many other areas of science.

In this chapter, we will introduce some of the basic concepts that are necessary to understand what is a tensor network and how the techniques used in this theory can be implemented in numerical computations. We begin with the most basic thing we need to understand, what is a tensor.



## ¿Que es un Tensor?



A tensor is a multidimensional array of complex numbers; This mathematical concept generalizes the idea of multilinear maps, i.e. functions of many parameters that are linear with respect to each other. Now, if this somewhat formal definition doesn't give you a clear idea of what tensors are, let's try with a more intuitive explanation.

A tensor is a series of numbers denoted with $N$ indices, where $N$ is what we call the "order" of the tensor. We've all dealt with tensors in our lives, for example:

* $5$ is a tensor of order $0$, it is a series of numbers for which we do not need indices to refer to an element. This is the same for any other scalar.

* The vector $\vec{T}=\begin{pmatrix} 0  \\ 1 \end{pmatrix}$, is a tensor of order $1$, we need a single index to refer to one of its elements (i.e. $i=1,2$)

```{image} images/vector.png
:alt: vector
:width: 200px
:align: center
```


* A matrix $\vec{T}=\begin{pmatrix} 0 & 0 \\1 & 1 \end{pmatrix}$ is a tensor of range $2$, we need two indices to identify unequivocally every single one of its elements


```{image} images/matriz.png
:alt: matrix
:width: 200px
:align: center
```


A tensor of order $3$ is a mathematical object that most people are not very familiar with, but let's imagine a set of numbers ordered in a Rubik's cube. To identify each of the numbers, we would need $3$ indices.


```{image} images/tensor.png
:alt: Tensor of order 3
:width: 200px
:align: center
```


|      Name       |             Example             | Order |              Notation             |
|:---------------:|:-------------------------------:|:-----:|:---------------------------------:|
|     Escalar     |             Constant            |   0   |             $\lambda$             |
|      Vector     |          Wave function          |   1   |             $\psi_{i}$            |
|      Matrix     |             Operator            |   2   |             $A_{i.j}$             |
|       : :       |               : :               |  : :  |                : :                |
|  N-order Tensor |       N-body Wave function      |   N   | $\psi_{\alpha_{1},...\alpha_{N}}$ |










The way that tensors are commonly used in physics and mathematics, is through what is called the index notation and the Einstein sum convention, which we will introduce in the next section.

<<<<<<< HEAD
### Representacion gráfica de redes tensoriales
=======
## Representación gráfica de redes tensoriales  
>>>>>>> ab3240fc3d5a0e570b63d074e8d67108b12d434e

Utilizamos una notación de diagramas para los tensores, donde cada tensor es dibujado como una figura sólida con un número de conexiones correspondientes a su orden. A continuación mostramos unos ejemplos:

**El Vector** $A_{i}$ se representa como:

```{image} images/vector_tensor.png
:alt: Un vector en su representación gráfica
:width: 140px
:align: center
```

**La matriz** $B_{ij}$ se representa como:

```{image} images/matrix_tensor.png
:alt: Una matríz en su representación gráfica
:width: 140px
:align: center
```

**Un tensor de orden 3** $C_{ijk}$ se representa como:

```{image} images/tensor_orden_3.png
:alt: Un tensor de orden 3 en su representación gráfica
:width: 140px
:align: center
```

Podemos formar redes de múltiples tensores donde un índice compartido por dos tensores denota una suma o contracción sobre el índice.

```{image} images/contraccion.png
:alt: Representación gráfica de una contracción entre dos tensores 
:width: 270px
:align: center
```

La multiplicación de dos matrices $A$ y $B$ se puede detonar en forma de suma como:
$C_{ik} = \sum_{j}A_{ij}B_{jk} $. Su equivalente en diagramas de tensores es:


```{image} images/ejemplo1.png
:alt: Representación gráfica de una contracción entre dos tensores, denotando una multiplicación de matrices 
:width: 270px
:align: center
```
De similar manera, podemos pensar en cómo representar otras operaciones comunes con matrices, como el producto interno. Si ahora consideramos dos vectores $A$ y $B$, su producto interno $\sum_{i}A_{i}\cdot B_{i}$ va a tener un equivalente en diagramas de tensores de forma:

```{image} images/prod_interno1.png
:alt: Representación gráfica de un producto interno con vectores
:width: 270px
:align: center
```

Con dos tensores $D$ y $E$ de rango 4 por ejemplo su producto interno $\sum_{ijkl}D_{ijkl}\cdot E_{ijkl}$ es equivalente al diagrama:

```{image} images/prod_interno2.png
:alt: Representación gráfica de un producto interno con tensores rango-4
:width: 270px
:align: center
```

Finalmente, también podemos representar la traza de un arreglo. Considerando una matriz $A$, su traza $\sum_{i}A_{ii}$ tiene un diagrama equivalente con la forma:

```{image} images/traza.png
:alt: Representación gráfica de la traza de una matriz
:width: 270px
:align: center
```

Podemos ver el poder de la notación con diagramas de tensores en la contracción de 3 tensores, donde en notación de sumatoria se expresa como: 
$D_{ijk} = \sum_{lmn}A_{ljm}B_{iln}C_{nmk} $. En cambio la notacion con diagramas es mucho mas sencilla de interpretar:

```{image} images/ejemplo2.png
:alt: Representación gráfica de una contracción entre tres tensores
:width: 270px
:align: center
```

En muchas aplicaciones, la meta es aproximar un tensor de alto orden, como el siguiente:

```{image} images/tensor_N.png
:alt: Tensor de orden N
:width: 270px
:align: center
```
A un red tensorial compuesta de muchos tensores de bajo orden:

```{image} images/tensor_N_2.png
:alt: Red tensorial de N tensores de bajo orden
:width: 270px
:align: center
```

La representación de redes tensoriales es muy útil por varias razones:

1. Pueden ofrecer una representación más comprimida de estructuras de datos grandes
2. Permite una caracterización de la estructura de los datos. Además de que la notación de diagramas permite una intuición visual más clara. 
3. Ya que la descomposición de los datos es robusta, se pueden utilizar para trabajar con ruido o con datos faltantes.
4. Permite tener un Framework unificado para manipular grandes datos. Como evaluar información estadística utilizando una colección pequeña de redes tensoriales sin la necesitada de tener conocimiento específico sobre la estructura de todo el sistema o de lo que representa.


La **teoría de redes tensoriales** se enfoca en entender como esta representación funciona y en que situaciones es más óptima utilizarla. En cambio, los **algoritmos de redes tensoriales** se enfocan en métodos para obtener , manipular y extraer información de estas representaciones.

## Redes tensoriales para sistemas cuánticos de muchos cuerpos

### Redes Tensoriales MPS

También conocidas como Tensor Train (TT), las redes tensoriales **Matrix Product State** son una manera de representar un tensor de rango-$N$ como una cadena de tensores de rango-$3$. Esta representación es el objetivo de muchos algoritmos debido a que provee una buena aproximación de las redes tensoriales y es más eficiente computacionalmente.

```{image} images/MPS.png
:alt: Ejecución de MPS como un recorte del Tensor objetivo en tensores más pequeños
:width: 470px
:align: center
```

Viendo está imagen puede ser difícil imaginarnos de donde viene dicha eficiencia, pero vayamos más a fondo. Recordemos que el estado cuántico $|\psi\rangle=\sum_{ij\dots k}\psi_{ij\dots k}|u_{ij\dots k}\rangle$ requiere de $2^n$ coeficientes independientes para ser descrito completamente, es decir, crece exponencialmente con el tamaño de n. Con MPS queremos poder describir al estado cómo:

$$|\psi\rangle = \sum_{ij\dots k}Tr(A_{i}^{[1]}A_{j}^{[2]}\dots A_{k}^{[n]})|u_{ijkl}\rangle$$

De esta manera los recursos necesarios para describirlo solo crecen linealmente con un factor de escalamiento de $nd\chi^{2}$ donde $d$ es la dimensión del subsistema y $\chi$ por $\chi$ es el limite superior de las matrices. Podemos observar en esta ecuación que conocer los componentes de $\psi$ solo es cosa de calcular el producto entre las matrices. Es de aquí de donde viene el nombre del método.

La capacidad de MPS de reducir los recursos viene de una herramienta llamada **Singular Value Decomposition** que se aplica de manera recursiva al tensor inicial $T$ para obtener las matrices que cumplen $T=U\Sigma V$, donde $U$ y $V$ son unitarios y $\Sigma$ es real, diagonal y no-negativa. 

Si volvemos al ejemplo de la imagen, podemos notar que los tensores de los extremos solo tienen dos conexiones, por lo que el producto va a dar como resultado un escalar entonces la traza no es necesaria. Y su ecuación tendria la forma:
$$|\psi\rangle = \sum_{ijkl}A_{i}^{[1]}A_{j}^{[2]}A_{k}^{[3]}A_{l}^{[4]}|u_{ij\dots k}\rangle$$



```{bibliography}
```
<!--- 



## Notacion indicial y convenio de la suma de Einstein 


Esta introduccion a la notacion indicial y la suma de einstein sera un poco distinta de la usual ya que estaremos revisando tres formas de ver estas manipulaciones, una que llamaremos algebraica (usada en relatividad), la diagramatica que aun no tiene estandar pero que llamaremos de Penrose siguiendo [articulo de biamonte] y la de dirac (usualmente usada en cuantica). Esto nos dara una perspectiva mas amplia al poder apreciar las ventajas y desventajas de cada una de ellas, y resaltar lo intuitiva que es la notacion grafica.








