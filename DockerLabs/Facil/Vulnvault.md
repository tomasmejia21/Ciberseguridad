<h1>Maquina Vulnvault</h1>

<h3>1. Ejecutamos la maquina descargada con "./auto_deploy.sh vulnvault.tar"</h3>
<img width="514" height="410" alt="image" src="https://github.com/user-attachments/assets/82f4b7e2-a311-4442-a6cc-4294720e7dda" />

<h3>2. Hacemos ping a la direccion que nos proporcionó docker para saber si esta activa la maquina.</h3>
<img width="479" height="122" alt="image" src="https://github.com/user-attachments/assets/913159d4-df56-46e3-a506-e2b5872dabea" />

<h3>3. Escaneamos los puertos abiertos con nmap con los parametros: </h3>
<p>-p- para escanear todos los puertos <br>--open para escanear solo los abiertos <br>--min-rate 5000 <br>-n para no buscar traduccion DNS <br>-Pn para asumir que el host esta activo <br>-vvv para que conforme encuentre puertos lo vaya mostrando en pantalla<br><br>Con esto nos damos cuenta que el puerto 22 (SSH) y el puerto 80(http) estan abiertos.</p>
<img width="761" height="396" alt="image" src="https://github.com/user-attachments/assets/82b77ff4-c2df-40dd-aadc-6596bdd60ccc" />

<h3>4. Procedemos a hacer fuzzing de rutas con gobuster en la direccion obtenida, todo esto siguiendo los parametros:</h3>
<p>"dir -u" para adjuntar la direccion <br>-w para adjuntar la ruta de wordlists con contenido ya establecido <br>-x con las extensiones php,txt,html<br><br> Luego de correr el comando, encontramos las siguientes rutas disponibles:</p>
<img width="1059" height="415" alt="image" src="https://github.com/user-attachments/assets/f55fdbc5-0e4f-4496-8617-228d6af9a687" />

<h3>5. Revisando las rutas obtenidas, nos encontramos con 4 rutas: 
  <ul>
    <li>/index.php que es la pagina por defecto apenas abrimos la pagina, que revisando su codigo fuente no obtenemos nada interesante</li>
    <li>/old que nos lleva a una version antigua de la pagina index.php sin css pero no tiene tampoco nada interesante</li>
    <li>/upload.html que nos lleva al apartado de subir archivos a la pagina</li>
    <li>/upload.php que nos muestra el estado de la subida de un archivo</li>
  </ul> 
</h3>
<img width="1162" height="539" alt="image" src="https://github.com/user-attachments/assets/206b84f3-e90d-4a97-85f5-403ba8def823" />
<img width="1164" height="540" alt="image" src="https://github.com/user-attachments/assets/f67b0760-f48e-4c5b-80dd-23c0f6d0beee" />
<img width="684" height="546" alt="image" src="https://github.com/user-attachments/assets/04e09dec-fe01-44fa-86d7-175027dc2d65" />
<img width="495" height="223" alt="image" src="https://github.com/user-attachments/assets/eae2050f-3a62-4dd6-82b5-2e06ec10a595" />

<h3>6. Intentamos subir un archivo a la pagina para ver si podemos sacar alguna informacion, pero no podemos obtener nada ni en la ruta /upload.html ni en /upload.php</h3>
<img width="880" height="418" alt="image" src="https://github.com/user-attachments/assets/3fd4958d-5694-4a39-ad48-f6e2093240b9" />
<img width="453" height="131" alt="image" src="https://github.com/user-attachments/assets/f056aee4-358b-43cc-b448-358226379bfc" />

<h3>7. Ahora, revisamos si a lo mejor se le puede hacer algun tipo de inyeccion a la pagina para realizar los formularios. Una forma comun es poniendo ; para finalizar el comando anterior y acompañarlo de algun otro comando para probar si tenemos algun acceso, y asi comprobamos que tenemos ejecucion de comandos remota.</h3>
<img width="747" height="327" alt="image" src="https://github.com/user-attachments/assets/eb1f7031-f08f-446d-9379-9ed65be4d5b2" />
<img width="525" height="354" alt="image" src="https://github.com/user-attachments/assets/da185d20-1917-4d7c-8c35-a0023578e2eb" />

<h3>8. Con la informacion anterior, vamos a intentar ejecutar una reverse shell (generada con revshells.com) poniendo previamente en escucha una shell con "nc -nlvp 433", pero nos damos cuenta que no es posible.</h3>
<img width="780" height="414" alt="image" src="https://github.com/user-attachments/assets/10551695-26f9-46ea-a567-4a4078f5b0d6" />
<img width="446" height="198" alt="image" src="https://github.com/user-attachments/assets/793b8974-b58d-418a-b0c5-0ab07b80cbc7" />
<img width="500" height="113" alt="image" src="https://github.com/user-attachments/assets/3e16909e-135c-4a99-9177-f3bd3533fedd" />

<h3>9. Ahora, intentamos obtener informacion de algun directorio o archivo que podamos acceder. Buscando nos damos cuenta que podemos acceder al directorio /etc/passwd, y aca nos damos cuenta que hay un usuario "samara" con el cual podemos buscar si tiene algo util.</h3>
<img width="586" height="470" alt="image" src="https://github.com/user-attachments/assets/31651674-1534-472b-9056-b41028f46aa9" />
<img width="565" height="448" alt="image" src="https://github.com/user-attachments/assets/6317ec53-287c-4f04-bc57-edaa06541679" />

<h3>10. Indagando en la ruta home del usuario samara, nos damos cuenta que hay un directorio /.ssh, y como previamente supimos que el puerto de SSH esta abierto, buscamos si hay una clave  SSH del usuario samara encontrandonos con este.</h3>
<img width="401" height="228" alt="image" src="https://github.com/user-attachments/assets/8ce9dd98-84ac-4739-846c-5386280e4ee7" />
<img width="398" height="150" alt="image" src="https://github.com/user-attachments/assets/81197223-3fd5-432f-b8ad-0f7a6fafee20" />
<img width="177" height="159" alt="image" src="https://github.com/user-attachments/assets/a879b555-0687-4f22-bbfe-842305f3dda6" />
<img width="447" height="469" alt="image" src="https://github.com/user-attachments/assets/134d6a1b-f362-4876-9c2d-e254f6d4d439" />

<h3>11. Intentamos acceder por medio de SSH a la maquina con el usuario samara y la clave SSH anteriormente encontrada, la cual almacenamos en un archivo llamado id_rsa creado previamente en la maquina usando emacs y dandole los respectivos permisos para que pueda ser accedido, y obtenemos acceso a la maquina.</h3>
<img width="1162" height="576" alt="image" src="https://github.com/user-attachments/assets/30c7e42c-3a68-4ed1-af33-0b7145c810f7" />
<img width="653" height="270" alt="image" src="https://github.com/user-attachments/assets/b7e29428-e992-483a-9f2d-b33876e67c1f" />

<h3>12. Intentamos buscar algo interesante en los directorios de tamara pero no encontramos nada, tambien vemos que no tiene permisos sudo.</h3>
<img width="490" height="344" alt="image" src="https://github.com/user-attachments/assets/01c92a37-977e-4fb4-a807-64640ce04111" />

<h3>13. Miramos en los procesos actuales ejecutandose con el comando "ps aux | cat" y nos encontramos con que el usuario root ejecuta un script llamado echo.sh en la ruta "/usr/local/bin/" periodicamente, el cual creaba el archivo message.txt que vimos anteriormente, le vamos añadir una linea al final de este para que bash tenga permisos SUID y podamos ejecutar comandos root en este.</h3>
<img width="1141" height="357" alt="image" src="https://github.com/user-attachments/assets/54e4a4cd-6c38-4622-8b69-34dc5af3ac68" />
<img width="548" height="89" alt="image" src="https://github.com/user-attachments/assets/d02bfae7-7b71-4b24-b3e5-1b5c04a3dc42" />
<img width="576" height="103" alt="image" src="https://github.com/user-attachments/assets/51f61edf-68ae-47f1-b522-79e39ddc74c9" />

<h3>14. Ahora escalamos permisos con "bash -p" y observamos que somos root en la maquina.</h3>
<img width="351" height="70" alt="image" src="https://github.com/user-attachments/assets/12a11533-25df-4aee-9cec-cb0722857eef" />
