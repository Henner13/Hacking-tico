Una vez hayamos hecho la preparación previa del laboratorio, lo primero será hacer un escaneo con NMAP.

```bash
sudo nmap -p- --open -sS -sV -sC --min-rate 5000 -n- -vvv -nP 10.129.204.96
```
*Nota :* La IP puede variar en cada caso.

En esta máquina podemos ver el puerto 23 abierto, que corre con el servicio telnet.
En este caso podemos simplemente entrar.
```
telnet 10.129.204.96
```
Esto nos llevará a una pequeña interfaz en la terminal, ahora lo que podemos hacer es probar a usar posibles usuarios sin contraseña como: admin, administrator o root.

En este caso nos podemos logear con ' root '.

![[Pasted image 20250613224239.png]](https://github.com/Henner13/Hacking-tico/blob/main/RedTeam/HackTheBox/Im%C3%A1genes/Pasted%20image%2020250613224239.png)
Aquí entraremos directamente como usuario root en una terminal Linux.
Una vez logueados usaremos
```bash
root@Meow:~# ls
flag.txt snap
root@Meow:~# cat flag.txt
*****************************
```


Así queda solucionado el primer Laboratorio.
