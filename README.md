# Redes Generativas [ES]

Material relativo a la asignatura de [Temas Avanzados de Ciencia e Ingeniería de Datos](https://www.uniovi.es/en/estudia/grados/ingenieria/datos/-/fof/asignatura/GCINGD01-4-008)

## Prácticas
Cada sesión de prácticas se corresponde con un Notebook que está en la carpeta correspondiente en este mismo repositorio. Debes ir leyendo y completando el notebook, puesto que hay celdas de código en las que se ha eliminado una parte y se ha sustituido por un comentario que comienza con #TODO, en el que se especifica qué se espera que haga el código que debes implementar.

## Alternativas de Software
Para ejecutar los modelos que veremos durante el desarrollo de la asignatura tenéis las siguientes opciones. No están ordenadas de acuerdo a ningún criterio, aunque a día de hoy ⚠️**recomendaría utilizar Lightning AI**⚠️.

### Lightning AI - Studios
De los creadores de Pytorch. Ofrece una plataforma unificada donde se pueden crear "estudios". Los estudios son entornos de python aislados con VSCode accesibles tanto desde la propia página como por ssh.
  
  ![imagen](https://github.com/user-attachments/assets/07371699-16cd-4831-8ded-93c40548b983)


  👍🏻 Ventajas:
  
  - Posibilidad de cambiar entre CPU/GPU
  - No hay que crear entornos, cada estudio tiene el suyo propio
  - Almacenamiento persistente
  
  👎🏻 Desventajas:

  -  Computación y almacenamiento limitado

  ⚠️Es aconsejable crear un estudio por cada práctica porque se van a necesitar diferentes versiones de paquetes⚠️

  Tu cuenta de lightning se debería ver tal que así:
  
  ![imagen](https://github.com/user-attachments/assets/8e694ab5-be12-4ad8-bb34-d97c4b42c244)


---

### Google Colab
Notebooks en la nube.
  
  ⚠️Recordad cambiar el tipo de entorno de ejecución dependiendo de si se quiere utilizar CPU o GPU⚠️

  ![image](https://github.com/user-attachments/assets/6c24628b-3d31-46fd-b659-440feddbc893)

  👍🏻 Ventajas:
  
  - Posibilidad de cambiar entre CPU/GPU
  - No hay que crear entornos
  
  👎🏻 Desventajas:

  -  Almacenamiento no persistente a menos que se utilice Google Drive
  -  Hay que instalar los paquetes necesarios cada vez que se inicia el cuaderno
  -  Computación y almacenamiento limitado

---

### Sistema de computación de altas prestaciones del Departamento de Informática
Sistema con una GPU A100, utiliza un modo de trabajo análogo al que se utiliza en los centros de supercomputación.

  ![image](https://github.com/user-attachments/assets/660c498e-e252-4150-81d2-d05916d91d32)

  👍🏻 Ventajas:
  
  - Es lo que se utiliza en la mayoría de empresas/centros que trabajan con grandes modelos
  - Almacenamiento persistente
  
  👎🏻 Desventajas:

  -  No se pueden ejecutar notebooks con GPU (en realidad sí, pero es incompatible con lo del siguiente punto)
  -  Se comparte con otras asignaturas por lo que la ocupación de la cola es arbitraria
  -  Hay que crear los entornos "a mano"

⚠️ **En cualquier caso, estaría bien que lo utilizarais al menos una vez para ver cómo funciona.** ⚠️

Este sistema dispone de varias colas (particiones en la nomenclatura de Slurm) a las que los usuarios pueden enviar sus trabajos:
  1.	cola-cpu → Para trabajos que requieren procesamiento en CPU
  2.	cola-gpu → Para trabajos que requieren procesamiento en GPU (mem. GPU=10GB)
  3.	cola-gpu-20 → Para trabajos que requieren procesamiento en GPU (mem. GPU=20GB)
 
Los usuarios se conectan por ssh al nodo de login para realizar todas las operaciones. Desde aquí envian sus trabajos a los nodos de computación mediante un sistema de colas llamado Slurm y pueden cargar los ficheros de datos que necesiten y extraer los ficheros de resultados.

  Para entrar:
  -  Desde la terminal: ssh login-hpcinformatica.edv.uniovi.es
  -  Desde VSCode con la extensión "Remote-SSH": Darle al "+" e introducir "ssh login-hpcinformatica.edv.uniovi.es"

      <img width="298" alt="image" src="https://github.com/user-attachments/assets/d3199c52-bbc4-46a1-997b-0889f5676575">

  El usuario es vuestro UO.


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

