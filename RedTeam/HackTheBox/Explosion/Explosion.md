En esta máquina empezaremos como siempre con un escaneo con nmap.

```bash
sudo nmap -p- --open -sC -sV -sS --min-rate 5000 -n -vvv -Pn 10.129.171.120
```

Aquí encontraremos varios puertos abiertos. Pero vamos a prestar mayor atención en este caso al puerto 3389 ya que corre el servicio de asistencia remota de Windows o también llamado `RDP`.

Así que vamos a usar `xfreerdp`.

**Nota**: En caso de no tenerlo se puede descargar de la siguiente forma:
```bash
sudo apt install freerdp2-x11
```
 Una vez sepamos si lo tenemos, lo usaremos.
```bash
xfreerpd /v:10.129.171.120
```

Nos pedirá un nombre de `Dominio` así que podemos ir probando con los más comunes como `user`, `admin` o `Administrator`. En este caso es `Administrator`.
Pero nos pide una contraseña, cosa que no sabemos. Así que vamos a probar a entrar diciéndole que ignore la certificación ( la contraseña).
```bash
xfreerpd /v:10.129.171.120 /cert:ignore /u:Administrator
```
Nos volverá a pedir que introduzcamos la contraseña pero al darle `Enter` nos dejará entrar.
Se nos abrirá directamente en la pantalla la pantalla de un Windows Server, y en el escritorio tenemos el archivo `flag` para resolver este laboratorio.
![[Pasted image 20250622193221.png]](https://github.com/Henner13/Hacking-tico/blob/main/RedTeam/HackTheBox/Im%C3%A1genes/Pasted%20image%2020250622193221.png)
