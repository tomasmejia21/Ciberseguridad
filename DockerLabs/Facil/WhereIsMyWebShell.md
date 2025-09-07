<h1>Maquina WhereIsMyWebShell</h1>

<h3>1. Corremos la maquina WhereIsMyWebShell en docker con "./auto_deploy.sh whereismywebshell.tar"</h3>
<img width="521" height="147" alt="image" src="https://github.com/user-attachments/assets/191c595b-bb2a-4ca2-9fc0-6b8207b42595" />

<h3>2. Probamos conectividad haciendo ping con la maquina y vemos que esta activa.</h3>
<img width="485" height="125" alt="image" src="https://github.com/user-attachments/assets/37ebeb21-1a46-43c5-9ed4-9ca5fe3495ca" />

<h3>3. Hacemos escaneo de puertos con nmap y con los parametros:</h3>
<p><br>-p- para escanear todos los puertos <br>--open para mirar solo los que esten abiertos <br>--min-rate 5000 para que sea mas rapido el escaneo <br>-n para que nmap no intente buscar la traduccion dns y sea mas rapido <br>-Pn para asumir que el host esta activo <br>-vvv para que conforme encuentre un puerto lo muestre en pantalla <br><br>Acá nos damos cuenta que el puerto 80 es el unico abierto.</p>
<img width="764" height="365" alt="image" src="https://github.com/user-attachments/assets/7d93ca9b-0ed1-4518-a063-27920d549d85" />

<h3>4. Entramos a la pagina web y solo encontramos con que en /tmp puede haber algo interesante. Revisando el codigo fuente no hay nada interesante.</h3>
<img width="937" height="122" alt="image" src="https://github.com/user-attachments/assets/cdf4113f-1b65-4df0-be59-73d1b8b7ba07" />

<h3>5. Hacemos fuzzing de posibles rutas con feroxbuster y gobuster, con los parametros:</h3>
<p><br>-u http://172.17.0.2 para adjuntar la direccion de la web <br>-w para adjuntar el wordlist que va a servir para la busqueda, este se encuentra directamente en el SO Kali <br>-x para definirle las extensiones de los archivos que va a buscar, en este caso buscamos php, txt y html</p>
<img width="960" height="487" alt="image" src="https://github.com/user-attachments/assets/77e41a75-92c6-421f-9721-358153b54d78" />
<img width="930" height="353" alt="image" src="https://github.com/user-attachments/assets/21d14907-c0ac-49d6-b562-a4ac920c335c" />

<h3>6. Mirando las rutas encontradas, nos damos cuenta que hay varias rutas disponibles, /index.html que es la pagina que nos encontramos nada mas abrir la pagina, /shell.php que no tiene ningun contenido pero podria ser usada por medio de parametros en la url, y /warning.html que nos muestra un mensaje que dice que esta pagina ya ha sido vulnerada anteriormente con un parametro en la webshell.</h3>
<img width="1163" height="294" alt="image" src="https://github.com/user-attachments/assets/3c326bee-9b38-49c9-8b5f-04402655feca" />

<h3>7. Vamos a intentar con wfuzz encontrar el parametro para usar para ejecutar la shell en la pagina web, para eso, usaremos los siguientes comandos junto a wfuzz:</h3>
<p>-c para mostrar los resultados con colores <br>-hl=0 para filtrar los resultados que no funcionen <br> -w para adjuntar el wordlist que se usara para la busqueda, este se encuentra intalado por defecto en el SO Kali <br>-u para adjuntar la url a la que se le hara la busqueda (http://172.17.0.2/shell.php?FUZZ=id), en donde esta la palabra fuzz, sera reemplazada por los parametros a buscar en el wordlist.<br><br>Acá nos encontramos con que la palabra clave es parameter, y usandolo nos damos cuenta que si funciona.</be></p>
<img width="1138" height="345" alt="image" src="https://github.com/user-attachments/assets/ef3d103b-f9c0-46e1-8860-2af155524716" />
<img width="668" height="307" alt="image" src="https://github.com/user-attachments/assets/b2ceeb2e-9b24-491f-bed7-e0bd86929bbb" />

<h3>8. Luego de esto, vamos a conseguir una reverse shell (generada en la pagina revshells.com), una vez generada esta revshell, la encriptamos en cualquier url encoder (yo use meyerweb.com/eric/tools/dencoder) para ponerla asi en la URL como parametro, una vez hecho esto, ponemos en nuestra bash el comando "nc -nlvp 2000", esto para estar en escucha por este puerto y accedemos con la URL, obteniendo asi acceso a la maquina.</h3>
<img width="792" height="455" alt="image" src="https://github.com/user-attachments/assets/2d402ab0-40a5-4fba-a55d-8374599d7048" />
<img width="749" height="220" alt="image" src="https://github.com/user-attachments/assets/2dbd326a-ddf1-466e-ac21-28aad70cb83b" />
<img width="938" height="128" alt="image" src="https://github.com/user-attachments/assets/f9ebafa3-3923-469b-8b57-49c29624e3e6" />
<img width="623" height="158" alt="image" src="https://github.com/user-attachments/assets/9d859223-933d-4a7e-84ef-c721a0893c5a" />

<h3>9. Una vez obtenemos acceso a la maquina, entraremos al directorio /tmp que estaba en /index.html y nos damos cuenta que hay un archivo llamado secret.txt, y entrando dice que es la contraseña del usuario root.</h3>
<img width="428" height="238" alt="image" src="https://github.com/user-attachments/assets/e3506058-4345-473a-bc9a-568e4f37e494" />

<h3>10. Conociendo ya las posibles credenciales del usuario root, intentaremos acceder a este con "su root" y vemos que obtenemos acceso a la maquina como usuario root.</h3>
<img width="291" height="84" alt="image" src="https://github.com/user-attachments/assets/12920d09-8a11-4ac8-b22d-6e096839b324" />
