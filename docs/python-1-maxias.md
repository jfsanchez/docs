# Maxias pitónicas

![images/python/ipython.png](images/python/ipython.png)

## E outras herbas

Ipython engade algo de funcionalidade na consola: autocopletado, maxias, etc. É un python interactivo. O jupyterlab precisa de ipython e ipykernel para funcionar correctamente.

Podemos lanzar unha consola interactiva de iPython con ese mesmo comando:

    (bigdata) PS C:\> ipython
    Python 3.8.18 (default, Sep 11 2023, 13:39:12) [MSC v.1916 64 bit (AMD64)]
    Type 'copyright', 'credits' or 'license' for more information
    IPython 8.12.0 -- An enhanced Interactive Python. Type '?' for help.

Podemos completar ou autocompletar e obter axuda con: TAB, Ctrl+TAB, Ctrl+Espacio...

Tamén temos dispoñibles unha serie de funcións listas para empregar nos notebooks que poden ser moi útiles:
<https://ipython.readthedocs.io/en/stable/interactive/magics.html>

A continuación veremos algunhas.

## %time

Permítenos medir canto tempo leva unha execución determinada.

**Exemplo:**


```python
import time
from random import randint

def tarefa_pitónica():
    tempo=randint(1, 10)
    print(f'Vaime levar: {tempo}')
    time.sleep(tempo)

%time tarefa_pitónica()
```

## %run

Executa scripts externos. Pode ser moi útil para preparar unha contorna cos datos, etc.



```python
%run holamundo.py
```

## %load

Amosa o código e despois o executa


```python
%load holamundo.py
```

## %edit

Abre o código para edición e logo permite executalo


```python
%edit holamundo.py
```

## Amosar axuda dunha maxia:

    %MAXIA??



```python
%edit??
```

## %pycat

Amosa o código


```python
%pycat holamundo.py
```

## %who

Amosa as variables en memoria


```python
%who
```

## %reset

Borra o estado da memoria


```python
%reset
```

## %save

Salva o estado da sesión, é dicir, as liñas que foron executadas.


```python
%save C:\\Users\\USUARIO\\sesion.py
```

## %lsmagic

Lista as diferentes maxias


```python
%lsmagic
```

## %env

Amosa as variables do sistema


```python
%env
```
