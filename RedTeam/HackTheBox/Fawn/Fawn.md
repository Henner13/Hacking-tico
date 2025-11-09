En este laboratorio empezaremos con el escaneo de puertos

```bash
nmap -p- --open  -sC -sV -sS -n -vvv -nP 10.129.154.174
```

![[Pasted image 20250613225756.png]] 

En este caso como no conocemos ni usuario ni contraseña probaremos usando el usuario ' anonymous ' que usara por defecto la contraseña 'anon123'.
*Nota :* Otra opción es usar Metasploit.

Bueno, una vez dentro del sistema FTP solo hay que buscar el documento/s que deseemos y descargarlos.
```
ftp > get documento.txt
```
Esto se nos descargará en nuestra máquina y con el comando 'cat ' lo podemos ver.


---
## FTP con Metasploit.



Vamos a ver como sería con Metasploit. Entramos en Metasploit.

![[Pasted image 20250613231836.png]]

![[Pasted image 20250613231924.png]]
Rellenamos los campos de RHOSTS, USERNAME y PASS_FILE. 

```
> set rhosts 10.129.154.174
> set username anonymous
> set pass_file /usr/share/wordlists/rockyou.txt
```
Usaremos el nombre de anonymous ya que no sabemos ningún nombre de usuario.
Para el pass_file usaremos la ruta de nuestro documento rockyou.txt

![[Pasted image 20250613232141.png]]

Nos da la contraseña ' 123456 '.
Ahora intentamos entrar como habíamos hecho antes.

![[Pasted image 20250613232342.png]]
Y entramos con éxito.


