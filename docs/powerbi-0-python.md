# 🧐 PowerBi y Python

## Crear un entorno de conda para Microsoft PowerBi

~~~~
conda create -n powerbi python=3.11
conda activate powerbi
conda install -c conda-forge matplotlib pandas mkl-service
~~~~

## Configurar un entorno conda en Microsoft PowerBI

  - Ir a: Archivo -> Opciones y configuración -> Opciones.
  - En la parte izquiera seleccionar: Creación de scripts de Python.
  - En establezca un directorio raíz para Python, examinar y seleccionar la carpeta del entorno de conda:
~~~~
C:\Users\usuario\AppData\Local\miniconda3\envs\powerbi
~~~~

## Usar un código en Python como origen de datos

  - Hacer click en Inicio -> Obtener datos -> Más...
  - Buscar: Script de Python.
  - Introducir este código de ejemplo:

~~~~
import pandas as pd
datos_estudantes = ({
    'Nomes':["Fulano", "Mengana", "Zutano", "Perengana"],
    'Pesos' :[83, 56, 90, 60],
    'Notas':[9, 8, 7, 6]
        })
df = pd.DataFrame(datos_estudantes)
~~~~
