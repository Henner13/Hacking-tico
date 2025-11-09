En esta máquina empezaremos como siempre  haciendo un escaneo con nmap.
```bash
sudo nmap -p- --open -sC -sS -sV --min-rate 5000 -n -vvv -Pn 10.129.89.155 
```

En este escaneo únicamente encontramos el puerto 6379 abierto con un servicio **`redis`**. Sabiendo este lo único que vamos a comprobar es el nombre del host.
```bash
redis-cli -h 10.129.89.155
```

![[Pasted image 20250622184704.png]]
![[Pasted image 20250622184808.png]]
Al final de toda la información vemos que hay 4 keys y expires 0, por lo que vamos a seleccionar ese 0.
```bash
10.129.89.155:6379> select 0
```

Ahora seleccionamos todas las keys con `*`

```bash
10.129.89.155:6379> keys * 
```

![[Pasted image 20250622185242.png]]
Nos mostrara las siguientes keys y para ver su interior solo tendremos que usar el comando `get`.

En la solución de este laboratorio lo que buscamos es el contenido de **`flag`**.
 Y así se soluciona este laboratorio.


---
## 📖CURIOSIDADES📖

¿Qué es Redis?

**Redis** es una base de datos en memoria de código abierto, muy rápida y versátil, que se utiliza principalmente como:

- **Base de datos clave-valor**
- **Sistema de caché**
- **Broker de mensajes (cola de tareas)**

 ¿Para qué se usa Redis?

1. **Caché de datos**: Acelera aplicaciones web guardando resultados de consultas frecuentes.
2. **Contadores y rankings en tiempo real**: Ideal para sistemas de puntuación o estadísticas.
3. **Colas de tareas**: Muy usado en sistemas distribuidos para manejar trabajos en segundo plano.
4. **Sesiones de usuario**: Guarda sesiones de login de forma rápida y temporal.
5. **Pub/Sub (publicar/suscribirse)**: Para enviar mensajes entre servicios o microservicios.

Características clave:

- Soporta estructuras como: `strings`, `lists`, `sets`, `hashes`, `sorted sets`, `bitmaps`, y más.
- Persistencia opcional (puede guardar en disco si lo deseas).
- Muy usado con lenguajes como Python, Node.js, Java, y .NET.
- Compatible con Docker y Kubernetes.


