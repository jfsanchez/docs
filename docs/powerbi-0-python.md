# 🧐 PowerBi e Python

## Crear un novo contorno de conda para Microsoft PowerBi

~~~~
conda create -n powerbi python=3.11
conda activate powerbi
conda install -c conda-forge matplotlib pandas mkl-service
~~~~

## Configurar un contorno conda en Microsoft PowerBI

  - Ir a: *"Archivo -> Opciones y configuración -> Opciones"*.
  - Na parte esquiera seleccionar: *"Creación de scripts de Python"*.
  - En "Establezca un directorio raíz para Python" preme en "Examinar" e selecciona o cartafol do contorno de conda:
~~~~
C:\Users\usuario\AppData\Local\miniconda3\envs\powerbi
~~~~

## Empregar código en Python como orixe de datos

  - Facer click en *"Inicio -> Obtener datos -> Más..."*
  - Buscar: *"Script de Python"*.
  - Podes empregar este código como exemplo:

~~~~
import pandas as pd
datos_estudantes = ({
    'Nomes':["Fulano", "Mengana", "Zutano", "Perengana"],
    'Pesos' :[83, 56, 90, 60],
    'Notas':[9, 8, 7, 6]
        })
df = pd.DataFrame(datos_estudantes)
~~~~

Tamén pode resultarche interesante:

  - **Expresiones DAX**: <https://learn.microsoft.com/es-es/dax/>
