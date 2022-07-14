## Tutorial para el analisis de genoma

Para realizar el analisis de secuencias de Mycobacterium Tuberculosis. vamos a realizar los siguentes pasos

Seguiremos esta diagrama de flujo.



Primero  verificamos la instalación correcta de Docker en nuestro equipo.

docker run --name conda-ubuntu_WGS -d  -v TU_CARPETA_EN_WINDOWS:/home/CARPETA_QUE_DESEAS_EN_LINUX/  continuumio/miniconda3  tail -f /dev/null 


docker exec -it conda-ubuntu_WGS   /bin/bash


Procedemos a instalar y actualizar Anaconda y bioconda.

conda config --add channels defaults
conda config --add channels bioconda
conda config --add channels conda-forge

Despues nos ubicamos en nuestra carpeta "wgs"  

Creamos una carpeta llamada "Curso_TB_Delfin"

Dentro de esta carpeta ireamos creando nuevas carpetas conforme se necesite, y descargando los archivos que se vayan indicando.


Al recibir la secuencia primero debemos valorar su calidad. Para esto utilizaremos el programa Fastqc.

URl:  https://github.com/s-andrews/FastQC

Este software se pude instalar  y correr desde windows, solo se requiere tener una version de java instalada.


# Ensamble

## Uso de A5- miseq

Instalación
 
 conda install -c bioconda a5-miseq

El programa A5 Produce ensamblajes de genoma microbiano de alta calidad en una computadora portátil sin ningún ajuste de parámetros.
A5 es un programa que automatiza todos los pasos para generar conjuntos de genoma bacteriano a partir de datos brutos de Illumina.
Consiste en cinco pasos:

(1) leer la limpieza;

(2) ensamble de cóntigo;

(3) andamio crudo;

(4) corrección de ensamblaje incorrecto;

(5) andamiaje final.

Una vez instalado el programa,  descargamos los archivos con los que  trabajaremos

raw : 

Para utilizar A5

Debemos colocarlos dentro de la carpeta del programa desde la terminal.
utilizando el comando "cd" nos colocamos en a5_miseq_macOS_20160825/bin

Utilizaremos esta secuencia para practicar el ensamble


Los ubicamos dentro de nuestra carpeta de trabajo /Curso_TB_Epimol/secuencias

en terminial  indicamos el siguente comando para descomprimir los archivos de secuencias.
  $ gunz -d 15-4493_S1_L001_R1_001.fastq.gz
  $ gunz -d  15-4493_S1_L001_R2_001.fastq.gz
  $ ls
  $ pwd

Nos mostrara una lista de los archivos ya descomprimidos sin el .gz
y la ubicación donde se encuentran nuestros archivos
/Curso_TB_Epimol/secuencias/15-4493_S1_L001_R1_001.fastq
/Curso_TB_Epimol/secuencias/15-4493_S1_L001_R2_001.fastq

nos movemos a la carpeta con el programa el archvo " a5_pipeline.pl" dentro de la carpeta .../a5_miseq_macOS_20160825/bin

En terminal colocamos

    $a5_pipeline.pl /Curso_TB_Epimol/secuencias/15-4493_S1_L001_R1_001.fastq /Curso_TB_Epimol/secuencias/15-4493_S1_L001_R2_001.fastq Ensamble_15_4493_ --threads 2

La logica de este comando es :
   perl = al lenguaje en el que esta codificado el script  " a5a5_pipeline"

  ./a5a5_pipeline =  ". " indica que el archivo esta en la misma carpta donde estamos ubicados en terminal,

  /Curso_TB_Epimol/secuencias/15-4493_S1_L001_R1_001.fastq = input_Forward

  /Curso_TB_Epimol/secuencias/15-4493_S1_L001_R1_001.fastq = input_Reverse

  Ensamble_15_4493_ = Nombre que llevaran todos los archivos temporales y de salida  (output)


  --threads 2  = Numero de trabajos que permitiremos que corran simultaneamente en nuestro procesador, esto permite que este programa se pueda correr en equipos portatiles sin dificultad.

Al termino de 1 hora proximadamente terminara el proceso generando una carpeta con el nombre que nosotros le especificamos.
El resultado de nuestro ensamble estara en el archivo:





evaluar ENSAMBLE

QUAST evaluates genome assemblies.
conda install -c bioconda quast


### Ordenamiendo

Ya con en ensamble relizado

Lo que procede es ordenar el genoma, el ensamble une los fragmentos por solapamiento, pero esto no garantiza la continuidad.

Por lo que es necesario comparar con un genoma de referencia para tener una cotinuidad en la posicion de genes y se pueda garantizar una mayor covertura en el proceso de anotación.

Para esto intalamos el programa CONTGuator
https://sourceforge.net/projects/contiguator/?source=typ_redirect

Una vez dentro de este la carpeta de este programa ubicamos el archivo CONTIGuator.py

Descargamos el genoma de referencia de Mycobacterium que deseamos comparar en este caso es para M. Bovis
  en la pagina de NCBI ftp://ftp.ncbi.nlm.nih.gov/genomes/all/GCF/000/195/835/GCF_000195835.2_ASM19583v2/GCF_000195835.2_ASM19583v2_genomic.fna.gz
  En el drobox de este curso https://www.dropbox.com/s/mlunzcpdiy5p1r1/GCF_000195835.2_ASM19583v2_genomic.fna?dl=0

  En terminal nos colocamos dentro de la carpeta de CONTGuator


  $ pwd



  $ python ./CONTIGuator.py -r ../Curso_TB_Epimol/GenomaRef/GCF.fasta -c  ../Curso_TB_Epimol/Ensamble_15_4493/ensamble15_4493_.contigs.

  O se puede usar la version web del ordenador

  http://combo.dbe.unifi.it/contiguator


  Podemos observar los resultados del ordenamiento en este link

  https://www.dropbox.com/sh/8izjwke83tuwn2y/AABcZcxJTyVMovej0sGcpVM5a?dl=0


  #### Anotación

  ingresamos a Rast

  http://rast.nmpdr.org/

  Y Creamos una cuenta








  ### Tarea

  Realizar el mismo procedimiento  para
secuencia  1
 https://www.dropbox.com/s/pwfg0vhn72efnqo/15-4494_S2_L001_R1_001.fastq.gz?dl=0
 https://www.dropbox.com/s/z6xdmqqciar6yfg/15-4494_S2_L001_R2_001.fastq.gz?dl=0

Secuencia 2

https://www.dropbox.com/s/2m8pkpiqtw0yejq/15-4495_S4_L001_R1_001.fastq.gz?dl=0
https://www.dropbox.com/s/91yijg90ucfpv0c/15-4495_S4_L001_R2_001.fastq.gz?dl=0



El Link para la presentación https://www.dropbox.com/s/f58d9s8ivcldx45/Secuenciaci%C3%B3n.pptx?dl=0







.




x
