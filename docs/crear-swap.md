# 🧠 Crear SWAP

Creamos un arquivo para swap e o formateamos:

```bash 
dd if=/dev/zero of=/archivoswap bs=1024K count=4096
mkswap /arquivoswap
```

Editamos como root o ´/etc/fstab´

```bash
sudo nano /etc/fstab
```

Nese arquivo metemos:

```
/arquivoswap none swap sw 0 0
```

Gardamos o arquivo e logo executamos:

``` bash
swapon -a
```
