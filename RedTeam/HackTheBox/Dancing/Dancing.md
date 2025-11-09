Comenzaremos esta máquina con nuestro escaneo habitual
```bash
nmap -p- -sS -sC -sV --min-rate=5000 -n- -vvv -Pn 10.129.1.12
```
En este caso vemos que esta abierto el puerto 445 que en Windows da servicio SMB.

Para explotar esto vamos a usar ' smbclient '
```
smbclient -L 10.129.1.12
```
Nos preguntaran por una contraseña pero no la sabemos. Pero nos da opciones 
![[Pasted image 20250613233914.png]]
Sabiendo esto,  es ir probando con cada opción de `Sharename`.
```
smbclient \\\\10.129.1.12\\ADMIN$
smbclient \\\\10.129.1.12\\C$
smbclient \\\\10.129.1.12\\WorkShares
```

Al final conseguimos entrar y nos encontramos con dos posibles usuarios.

![[Pasted image 20250613234446.png]]

Vamos navegando por la terminal hasta conseguir que buscamos. 
![[Pasted image 20250613235451.png]]

Para descargar cada archivo.txt usaremos el siguiente comando:
``` bash
smb: \usuario\> get archivo.txt
```
