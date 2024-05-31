---
layout: post
title: Taller Teórico-Práctico de Metabarcoding 2024
date: 2024-05-17 00:00:00
description: Material para el Taller Teórico-Práctico de Metabarcoding
tags: metabarcoding microeco bioinformatics
categories: workshop
featured: true
---
# Tutorial de Metabarcoding

## Tabla de Contenidos
1. Introducción
2. Sobre los datos que vamos a manejar
3. Paso por paso
4. Extracción de ADN
5. PCR y Electróforesis
6. Secuenciación
7. Análisis de Datos
8. Conclusiones

## Introducción
El metabarcoding es una técnica de secuenciación de ADN que permite identificar y caracterizar la biodiversidad presente en una muestra ambiental. Utiliza secuencias de ADN seleccionadas para determinar rápidamente las especies que componen una comunidad biológica. Estas secuencias normalmente son marcadores taxonómicos con características específicas ya seleccionadas. Para el caso del taller de hoy, ese marcador será el 16S, por características que estaremos definiendo después.

Asumimos que los ácidos nucléicos y las proteínas cambian con el tiempo. Es por ello que pueden considerarse como registros de la historia evolutiva. Teoretizando que estos cambios se dan al azar y que aumentan de manera lineal, se deduce que estos cambios pueden reflejar la distancia evolutiva existente entre ellas. 

Los ribosomas son orgánulos que utilizan los organismos para la síntesis de proteínas. El ribosoma bacteriano tiene un índice de sedimentación de 70S (coeficiente de Svedberg), razón por la cuál se presenta usualmente con esta unidad. Puede disociarse en dos subunidades: La subunidad grande 50S y la subunidad pequeña 30S.

![El ribosoma bacteriano](https://static.elsevier.es/multimedia/0213005X/0000002200000004/v0_201607121142/13059055/v0_201607121143/es/main.assets/28v22n04-13059055tab01.gif?idApp=UINPBA00004N "El ribosoma bacteriano")

Los genes que codifican los ARN ribosomales están organizados en operones (conjunto de genes que se transcriben a partir de la misma región promotora). Cada operón cuenta con regiones rrl (que codifica para 23S), rrs (16S) y 5S (rrf). Las regiones promotoras P1 y P2 se sitúanen la región anterior a rrs. Las regiones se encuentran separadas por regiones intergénicoas. Al momento que la ARNasaIII procesa el producto del operón mediante cortes, separa las tres clases de ARNr y las dos IG ([1][paper-1]).

![El operón del ribosoma bacteriano](https://static.elsevier.es/multimedia/0213005X/0000002200000004/v0_201607121142/13059055/v0_201607121143/es/main.assets/28v22n04-13059055tab02.gif?idApp=UINPBA00004N "El operón del ribosoma bacteriano")

### ¿Por qué 16S?

El gen de la secuencia ribosomal 16S presenta ciertas características únicas que lo hacen ideal para el trabajo ([2][paper-2]):
1. Presente en todos los organismos vivos.
2. No presenta recombinación, solo es una copia.
3. Altamente conservado y con regiones que son altamente variables (raro, pero básicamente lo primero indica que su función se mantiene porque es vital, mientras que sus regiones varían)
4. Gran base de datos de referencia.

![Variabilidad entre regiones del gen 16S](https://training.galaxyproject.org/training-material/topics/microbiome/images/16S_variableregions.jpg "Variabilidad entre regiones del gen 16S")

## Sobre los datos que vamos a manejar

Para este tutorial, estaremos realizando lo que se describe en este tutorial de Galaxy. El procedimiento será similar, pero estaré presentando esta información en español y con actualizaciones respecto a las imagenes utilizadas. [Así que todos los créditos van a ellos, realmente](https://training.galaxyproject.org/training-material/topics/microbiome/tutorials/mothur-miseq-sop-short/tutorial.html). Mayor detalle puede ser observado en su página. 

Estaremos utilizando data del laboratorio Schloss. Estos describen su experimento de la siguiente manera:

"El laboratorio Schloss está interesado en comprender el efecto de la variación normal en el microbioma intestinal en la salud del huésped. Con ese fin, recolectamos heces frescas de ratones diariamente durante 365 días después del destete. Durante los primeros 150 días después del destete (dpw), no se hizo nada con nuestros ratones, excepto permitirles comer, engordar y estar felices. Nos preguntábamos si el cambio rápido de peso observado durante los primeros 10 dpw afectó la estabilidad del microbioma en comparación con el microbioma observado entre los días 140 y 150. Analizaremos un solo ratón en 20 puntos de tiempo diferentes (10 temprano, 10 tardío). Para evaluar la tasa de error del análisis y la configuración experimental, el laboratorio Schloss también secuenció una comunidad simulada con una composición conocida (ADN genómico de 21 cepas bacterianas)."


## Paso por paso

### Crear cuenta en Galaxy e importar secuencias

Primero es necesario generar una cuenta en Galaxy. Utilizaremos esta plataforma puesto que no requiere ninguna instalación complicada y nos permite centrarnos en los pasos. Para ello requieres hacer lo siguiente:

Luego de este paso, nos toca importar la data a utilizar. Para ello utilizaremos una forma de importación especial. Requeriremos URLs de nuestras muestras. Aquí van:

```bash
https://zenodo.org/record/800651/files/F3D0_R1.fastq
https://zenodo.org/record/800651/files/F3D0_R2.fastq
https://zenodo.org/record/800651/files/F3D141_R1.fastq
https://zenodo.org/record/800651/files/F3D141_R2.fastq
https://zenodo.org/record/800651/files/F3D142_R1.fastq
https://zenodo.org/record/800651/files/F3D142_R2.fastq
https://zenodo.org/record/800651/files/F3D143_R1.fastq
https://zenodo.org/record/800651/files/F3D143_R2.fastq
https://zenodo.org/record/800651/files/F3D144_R1.fastq
https://zenodo.org/record/800651/files/F3D144_R2.fastq
https://zenodo.org/record/800651/files/F3D145_R1.fastq
https://zenodo.org/record/800651/files/F3D145_R2.fastq
https://zenodo.org/record/800651/files/F3D146_R1.fastq
https://zenodo.org/record/800651/files/F3D146_R2.fastq
https://zenodo.org/record/800651/files/F3D147_R1.fastq
https://zenodo.org/record/800651/files/F3D147_R2.fastq
https://zenodo.org/record/800651/files/F3D148_R1.fastq
https://zenodo.org/record/800651/files/F3D148_R2.fastq
https://zenodo.org/record/800651/files/F3D149_R1.fastq
https://zenodo.org/record/800651/files/F3D149_R2.fastq
https://zenodo.org/record/800651/files/F3D150_R1.fastq
https://zenodo.org/record/800651/files/F3D150_R2.fastq
https://zenodo.org/record/800651/files/F3D1_R1.fastq
https://zenodo.org/record/800651/files/F3D1_R2.fastq
https://zenodo.org/record/800651/files/F3D2_R1.fastq
https://zenodo.org/record/800651/files/F3D2_R2.fastq
https://zenodo.org/record/800651/files/F3D3_R1.fastq
https://zenodo.org/record/800651/files/F3D3_R2.fastq
https://zenodo.org/record/800651/files/F3D5_R1.fastq
https://zenodo.org/record/800651/files/F3D5_R2.fastq
https://zenodo.org/record/800651/files/F3D6_R1.fastq
https://zenodo.org/record/800651/files/F3D6_R2.fastq
https://zenodo.org/record/800651/files/F3D7_R1.fastq
https://zenodo.org/record/800651/files/F3D7_R2.fastq
https://zenodo.org/record/800651/files/F3D8_R1.fastq
https://zenodo.org/record/800651/files/F3D8_R2.fastq
https://zenodo.org/record/800651/files/F3D9_R1.fastq
https://zenodo.org/record/800651/files/F3D9_R2.fastq
https://zenodo.org/record/800651/files/Mock_R1.fastq
https://zenodo.org/record/800651/files/Mock_R2.fastq
```
A continuación, importamos también la data complementaria:

```bash
https://zenodo.org/record/800651/files/HMP_MOCK.v35.fasta
https://zenodo.org/record/800651/files/silva.v4.fasta
https://zenodo.org/record/800651/files/trainset9_032012.pds.fasta
https://zenodo.org/record/800651/files/trainset9_032012.pds.tax
https://zenodo.org/record/800651/files/mouse.dpw.metadata
```
Proseguiremos creando un dataset. Para ello, seleccionaremos todas nuestras secuencias con un check y le daremos a "Build dataset pairs list". Automáticamente, Galaxy deducirá que tienes secuencias forward (R1) y reverse (R2). De no ser así, se le puede agregar ese filtro de todos modos. Vemos que Galaxy también le ha asignado un nombre a cada uno de los archivos pareados, de acuerdo al nombre que queda detrás del guión bajo.

Por último, agrega el nombre de tu preferencia en la parte de abajo.

### Crear cuenta en Galaxy e importar secuencias


## Extracción de ADN
Describe el proceso de extracción de ADN de las muestras recogidas.

```bash
# Ejemplo de código para la extracción de ADN
echo "Aquí va el código relacionado con la extracción de ADN"
```

[paper-1]: https://www.elsevier.es/es-revista-enfermedades-infecciosas-microbiologia-clinica-28-articulo-identificacion-bacteriana-mediante-secuenciacion-del-13059055
[paper-2]: https://es.slideshare.net/slideshow/ccbc-tutorial-beiko/62069404
 
