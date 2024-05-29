# Proyecto de Redes Adversariales Generativas (GAN y CGAN)

Este repositorio contiene el desarrollo de un proyecto utilizando Redes Adversariales Generativas (GAN) y Redes Adversariales Generativas Condicionales (CGAN) aplicadas a los conjuntos de datos MNIST y Fashion MNIST.

## Instalación

1. **Clonar el Repositorio**: Primero, clona el repositorio en tu máquina local usando Git:

    ```bash
    git clone https://github.com/yourusername/your-repository.git
    ```

2. **Navegar al Directorio del Proyecto**: Muévete al directorio del proyecto:

    ```bash
    cd your-repository
    ```

3. **Crear un Entorno Virtual (Opcional pero Recomendado)**: Es una buena práctica crear un entorno virtual para gestionar las dependencias de tu proyecto. Puedes crear un entorno virtual usando `virtualenv` o `conda`. Aquí tienes un ejemplo usando `virtualenv`:

    ```bash
    # Instalar virtualenv si no lo has hecho antes
    pip install virtualenv
    
    # Crear un entorno virtual
    virtualenv venv
    
    # Activar el entorno virtual
    source venv/bin/activate
    ```

4. **Instalar Dependencias**: Instala los paquetes Python requeridos usando pip:

    ```bash
    pip install -r requirements.txt
    ```

5. **Ejecutar el Cuaderno de Jupyter**: Una vez que las dependencias estén instaladas, puedes ejecutar el cuaderno de Jupyter que contiene el código de tu proyecto:

    ```bash
    jupyter notebook MachineLearningModel.ipynb
    ```

6. **Explorar el Proyecto**: Abre el cuaderno de Jupyter en tu navegador y sigue las instrucciones dentro del cuaderno para explorar el proyecto y ejecutar las celdas de código.


## Desarrollo del Proyecto

### Parte 1: Exploración y Preprocesamiento de Datos

#### 1.1 MNIST
El dataset de MNIST forma parte de la librería de Pytorch y consiste en un conjunto de entrenamiento de 60,000 imágenes y un conjunto de prueba de 10,000 imágenes de números escritos a mano. Las clases en este dataset son números que van del 0 al 9. Además, la distribución de los datos entre las distintas clases en el conjunto de entrenamiento es relativamente balanceada.

#### 1.2 Fashion MNIST
Fashion-MNIST consiste en 70,000 imágenes en escala de grises de productos de moda clasificadas en 10 categorías. Dentro de cada categoría, hay 7,000 imágenes. El conjunto de datos está dividido en un conjunto de entrenamiento de 60,000 imágenes y un conjunto de prueba de 10,000 imágenes, al igual que MNIST. La distribución de clases es completamente equitativa.

### Preprocesamiento
Se diseñó una secuencia de transformación que se aplicará al conjunto de datos. Esta consiste en la transformación a tensores de Pytorch y la normalización de los valores de los píxeles de cada imagen para que tengan una media de 0.5 y una desviación estándar de 0.5.

En las siguientes líneas de código, se cargan los conjuntos de datos MNIST y Fashion-MNIST, aplicando las transformaciones definidas previamente para el preprocesamiento de las imágenes.

La librería de PyTorch utiliza loaders los cuales retornan los datos durante el entrenamiento. En este caso, los lotes consisten en 64 imágenes cada uno y además estos datos serán “shuffled” para que el orden de las fotos no afecte el entrenamiento.

## Parte 2: Diseño y Entrenamiento de GAN

### Arquitecturas Utilizadas

#### Red Adversarial Generativa (GAN)
Una GAN consta de dos elementos principales: un generador (G) y un discriminador (D). El generador tiene como objetivo replicar la distribución de datos del conjunto de entrenamiento produciendo muestras artificiales. En este caso, el generador va a crear imágenes de números escritos para el dataset MNIST e imágenes de artículos de ropa para el dataset Fashion MNIST. El discriminador actúa como un juez que determina si una muestra es real o falsa.

#### Red Adversarial Generativa Condicional (CGAN)
La CGAN es similar a la GAN regular pero incluye un aspecto condicional, donde el generador debe cumplir con un parámetro específico o se le añade una etiqueta para tener más control sobre el tipo de dato generado. En este proyecto, la CGAN se utilizó para crear resultados basados en la etiqueta.

### Análisis de la Interacción Generador-Discriminador

#### 2.1 Modelo Generador
El generador tiene como función principal la generación de datos. Inicialmente, estos datos tienden a ser aleatorios. Con cada época, el modelo aprende cómo se distribuyen los datos y crea imágenes más realistas. Utiliza capas convolucionales transpuestas, capas lineales y capas de activación para crear imágenes a partir de vectores de ruido.

#### 2.2 Modelo Discriminador
El discriminador tiene la función de distinguir entre datos reales y falsos. Recibe muestras de datos, que pueden ser reales o generadas, y su salida es un valor escalar entre 0 y 1, que representa la probabilidad de que la muestra de entrada sea real. Utiliza capas convolucionales, capas de normalización de lotes y funciones de activación como Leaky ReLU.

#### 2.3 Relación Generador-Discriminador
Según Brownlee (2019), esta relación adversarial se asemeja a un juego donde el generador actúa como un falsificador y el discriminador como la aplicación de la ley. Ambos modelos ajustan sus estrategias continuamente, lo que conduce a un escenario competitivo donde las mejoras de un modelo se logran a expensas del otro. Este proceso permite que las GANs alcancen un equilibrio donde el generador puede crear muestras indistinguibles de los datos reales.

## Parte 3: Búsqueda Hiperparámetros
En cuanto a la optimización de los parámetros, se decidió hacer un primer entrenamiento al inicio para GAN y CGAN de ambos datasets y al final utilizar Optuna para calcular los mejores hiperparámetros. Esta librería evaluó los parámetros para  lr (tasa de aprendizaje), batch_size y betas_first para la aplicación de GAN en ambos datasets. 

## Parte 4: Análisis Resultados
### Resultados del Entrenamiento

#### MNIST

##### GAN
- Precisión Promedio del Generador: 0.8575
- Precisión Promedio del Discriminador: 0.8160

##### CGAN
- Precisión Promedio del Generador: 0.4846
- Precisión Promedio del Discriminador: 0.3987

#### Fashion MNIST

##### GAN
- Precisión Promedio del Generador: 0.7205
- Precisión Promedio del Discriminador: 0.6842

##### CGAN
- Precisión Promedio del Generador: 0.6106
- Precisión Promedio del Discriminador: 0.5862

## Contacto
Creado por Raquel Mora - raquel.mora@ulead.ac.cr, David Corrales - david.corrales@ulead.ac.cr, Alexa Coll - alexa.coll@ulead.ac.cr


## Referencias

- Brownlee, J. (2019, July 19). "A gentle introduction to generative adversarial networks (Gans)." MachineLearningMastery.com. [Enlace](https://machinelearningmastery.com/what-are-generative-adversarial-networks-gans/)
- Busquet, M. (2023, September 5). "Understanding image generation: A beginner’s guide to generative adversarial networks." OVHcloud Blog. [Enlace](https://blog.ovhcloud.com/understanding-image-generation-beginner-guide-generative-adversarial-networks-gan/)
- Pra, M. D. (2023, October 30). "Generative Adversarial Networks." Medium. [Enlace](https://medium.com/@marcodelpra/generative-adversarial-networks-dba10e1b4424)
- Sharma, A. (2024, January 10). "Conditional gan (cgan) in pytorch and tensorflow." LearnOpenCV. [Enlace](https://learnopencv.com/conditional-gan-cgan-in-pytorch-and-tensorflow/)


