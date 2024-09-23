# Redes Generativas [ES]

Material relativo a la asignatura de [Temas Avanzados de Ciencia e Ingeniería de Datos](https://www.uniovi.es/en/estudia/grados/ingenieria/datos/-/fof/asignatura/GCINGD01-4-008)


## Alternativas de Software
Para ejecutar los modelos que veremos durante el desarrollo de la asignatura tenéis las siguientes opciones (no están ordenadas de acuerdo a ningún criterio):
- [Google Colab](https://colab.research.google.com/): Notebooks en la nube con posibilidad de subir archivos no persistentes o conectarse a los archivos de Google Drive.
  
  ⚠️Recordar cambiar el tipo de entorno de ejecución dependiendo de si se quiere utilizar CPU o GPU⚠️

  ![image](https://github.com/user-attachments/assets/6c24628b-3d31-46fd-b659-440feddbc893)

- Lightning AI - Studios: de los creadores de Pytorch. Ofrece una plataforma unificada donde se pueden crear "estudios", que son entornos de python aislados con VSCode accesibles tanto desde la propia página como por ssh.

  También ofrece la posibilidad de cambiar entre CPU/GPU
  
  ![image](https://github.com/user-attachments/assets/83cdeedb-8c55-4d85-bf62-70df51b5556d)

- Sistema de computación de altas prestaciones del Departamento de Informática: sistema con una GPU A100, utiliza un modo de trabajo análogo al que se utiliza en los centros de supercomputación.

  ![image](https://github.com/user-attachments/assets/660c498e-e252-4150-81d2-d05916d91d32)



  Este sistema dispone de varias colas (particiones en la nomenclatura de Slurm) a las que los usuarios pueden enviar sus trabajos:
  1.	cola-cpu → Para trabajos que requieren procesamiento en CPU
  2.	cola-gpu → Para trabajos que requieren procesamiento en GPU (mem. GPU=10GB)
  3.	cola-gpu-20 → Para trabajos que requieren procesamiento en GPU (mem. GPU=20GB)
 
  Los usuarios se conectarán por ssh al nodo de login para realizar todas las operaciones. Desde aquí enviarán sus trabajos a los nodos de computación mediante un sistema de colas llamado Slurm y podrán cargar los ficheros de datos que necesiten y extraer los ficheros de resultados.


  En la primera conexión se debe ejecutar lo siguiente para poder utilizar conda:

  ```
  eval "$(/soft/miniconda3/bin/conda shell.bash hook)"
  conda init
  conda config --set auto_activate_base false
  ```

  Luego, para crear un entorno:

  ```
  conda create --name generativos python=3.X # Sustituyendo X por la versión que se desee
  conda activate generativos
  ```

  Comandos de SLURM:
  ```
  # Lanzar trabajo
  sbatch --partition=cola-gpu --gres=gpu:A100 script.sh # Esto genera un fichero slurm-XXX.outscript
  
  # Comprobar ocupación de la cola
  squeue
  
  # Cancelar trabajo
  scancel <job_id>
  ```

  Ejemplo de script.sh
  ```
  #!/bin/bash
  host=`hostname`
  hora=`date`
  echo "Usuario logname: $LOGNAME"
  echo "Home: $HOME"
  echo "Hora ejecución: $hora"
  echo "Nodo de cálculo: $host"
  echo "HOLA"
  sleep 20
  echo "ADIOS"
  python -c "print('hello world')"
  ```

