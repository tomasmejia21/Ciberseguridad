<h1>Maquina PingPong</h1>

<h3>1. Corremos la maquina en Docker ejecutando los archivos auto_deploy.sh y pingpong.tar</h3>
<img width="555" height="375" alt="image" src="https://github.com/user-attachments/assets/0b845745-d3ca-4978-b67b-d65efbe89ef5" />

<h3>2. Hacemos prueba de conectividad con "ping 172.17.0.2"</h3>
<img width="486" height="137" alt="image" src="https://github.com/user-attachments/assets/b6ea3e57-3755-4b01-815b-153ef2e42039" />

<h3>3. Hacemos un escaneo de puertos con nmap agregandole los siguientes parametros:</h3>
<p>
  <ul>
    <li>-p- Para hacer escaneo a todos los puertos</li>
    <li>--open Para mostrar solo puertos abiertos</li>
    <li>-sS Para hacer un escaneo SYN</li>
    <li>--min-rate 5000 Para aumentar la velocidad del escaneo</li>
    <li>-n Para no realizar traduccion DNS</li>
    <li>-Pn Para asumir que los hosts estan activos</li>
    <li>-vvv Para mostrar informacion detallada de los puertos que va detectando</li>
  </ul>
</p>
<h3>Una vez hecho esto, encontramos los puertos 80/tcp(http), 443/tcp(https) y 5000/tcp(upnp) abiertos.</h3>
<img width="551" height="426" alt="image" src="https://github.com/user-attachments/assets/645aeb2f-9177-424a-b86c-37c918428ddf" />

<h3>4. Intentamos acceder a cada uno de los puertos de la URL, en el puerto 80 nos encontramos con la pagina por defecto de apache y no se observo nada en el codigo fuente, en el puerto 443 no hay nada, y en el puerto 5000 nos encontramos con una aplicacion que realiza pruebas de ping.</h3>
<img width="1114" height="548" alt="image" src="https://github.com/user-attachments/assets/8638279a-3806-4a54-8d8e-02bd9a274b4c" />
<img width="1115" height="543" alt="image" src="https://github.com/user-attachments/assets/c3edb2b3-c13d-42d3-8ad9-b614bac05293" />

<h3>5. hacemos fuzzing de las posibles rutas con gobuster con los parametros:</h3>
<p>
  <ul>
    <li>dir -u Para adjuntar la direccion obtenida</li>
    <li>-w para adjuntarle la ruta de posibles directorios ya establecidos en un wordlist</li>
    <li>-x para las extensiones que queremos buscar, en este caso son: php, html, txt, py, sh, txt</li>
  </ul>
</p>
<h3>Con el fuzzing, no encontramos nada util, por ende, nos concetraremos a ver si el puerto 5000 tiene algo interesante.</h3>
<img width="1092" height="376" alt="image" src="https://github.com/user-attachments/assets/d306c2cd-4649-4afa-b9d8-60b464280d08" />

<h3>5. Pruebo a ver si la pagina hace pruebas de conectividad poniendo su propia IP y vemos que funciona haciendo pruebas de conectividad.</h3>
<img width="434" height="394" alt="image" src="https://github.com/user-attachments/assets/88db5850-b152-411c-9f7f-ec3dc89d4ea3" />

<h3>6. Ahora insertando valores erroneos, vemos que la pagina nos muestra un mensaje de error directamente de una shell, por ende, vamos a probar si se puede hacer ejecucion remota de comandos.</h3>
<img width="434" height="390" alt="image" src="https://github.com/user-attachments/assets/b2a4802c-1435-4915-9f68-88aa61af6679" />

<h3>7. Probamos poniendo un ; para finalizar cualquier comando y poniendo el comando "whoami" y nos damos cuenta que hay ejecucion remota de comandos.</h3>
<img width="433" height="388" alt="image" src="https://github.com/user-attachments/assets/b83d3a5c-7696-43df-a906-85dc23e2d2b7" />

<h3>8. Sabiendo que hay ejecucion remota de comandos, probaremos a enviarnos una revershell (generada en la web revshells.com) para obtener acceso, poniendo a escuchar por el puerto 2000.</h3>
<img width="852" height="544" alt="image" src="https://github.com/user-attachments/assets/7ed7aa61-42d1-49bc-bfcf-1aa9ad4c029a" />
<img width="438" height="389" alt="image" src="https://github.com/user-attachments/assets/e7452914-ae03-4642-9ca4-69afbc1e883c" />
<img width="625" height="112" alt="image" src="https://github.com/user-attachments/assets/6bea0941-2e50-4389-884e-fbfe835bb165" />

<h3>9. Una vez con acceso a una shell, procedemos a buscar algo util en los directorios, pero no encontramos nada.</h3>
<img width="480" height="416" alt="image" src="https://github.com/user-attachments/assets/1e55861d-3ede-4d53-96b4-8873120c32cb" />
<h3>Tambien observamos que hay varios usuarios en la maquina a los cuales a lo mejor podriamos acceder.</h3>
<img width="544" height="191" alt="image" src="https://github.com/user-attachments/assets/dc512103-399b-4829-9c57-d85ce6ac216e" />
<img width="511" height="450" alt="image" src="https://github.com/user-attachments/assets/9e1d426e-fa16-46a6-96a5-bab8aa6e3767" />

<h3>10. Luego, con "sudo -l" buscamos que puede ejecutar el usuario actual con permisos sudo y nos encontramos con que hay un binario llamado dpkg el cual podemos ejecutar con el usuario bobby.</h3>
<img width="751" height="152" alt="image" src="https://github.com/user-attachments/assets/0f92f009-b0d5-4e4e-927c-d4242ecc2af0" />
<h3>Buscando en la pagina "gtfobins.github.io" vemos que con este binario pondemos obtener acceso al usuario que lo ejecuta haciendo: </h3
<img width="854" height="227" alt="image" src="https://github.com/user-attachments/assets/1f324032-743d-4a9f-9d41-e221c28dfa74" />
<h3>Intentamos escalar privilegios ejecutando los comandos anteriores y obtenemos acceso al usuario bobby. <br><br>(Cabe recalcar que previamente debemos hacer un tratamiento de la shell para que podamos usar esta mas comodamente.)</h3>
<img width="652" height="453" alt="image" src="https://github.com/user-attachments/assets/a15c6635-4e9a-4e74-95a6-fa5bbd6d7f59" />

<h3>11. Una vez tenemos acceso al usuario bobby, ejecutamos nuevamente el "sudo -l" dandonos cuenta que el usuario gladys puede ejecutar un binario llamado php como sudo.</h3>
<img width="755" height="142" alt="image" src="https://github.com/user-attachments/assets/a7b770ef-6d2d-4aea-994d-1a5d04087ecd" />
<h3>Buscando en la misma pagina anteriormente mencionada, nos encontramos con que debemos ejecutar los siguientes comandos para escalar privilegios.</h3>
<img width="828" height="168" alt="image" src="https://github.com/user-attachments/assets/5901e203-c6d7-450d-91fa-9633df833510" />
<h3>Intentamos nuevamente escalar privilegios ejecutando los comandos y obtenemos acceso a gladys.</h3>
<img width="581" height="37" alt="image" src="https://github.com/user-attachments/assets/4278628f-6cd6-414f-81f2-95895f629d56" />
<img width="56" height="33" alt="image" src="https://github.com/user-attachments/assets/f919d6aa-978a-4943-956e-20eada294d2b" />

<h3>11. Ahora, con acceso a gladys, volvemos a mirar que puede ejecutar este usuario con "sudo -l", dandonos cuenta que hay un binario llamado "cut" que puede ejecutar el usuario chocolatito con permisos sudo. Este binario sirve para extraer informacion de documentos de texto.</h3>
<img width="1016" height="104" alt="image" src="https://github.com/user-attachments/assets/a8377032-d294-4f53-8a32-8ee98502a03d" />
<h3>Nuevamente buscando en la pagina anterior, vemos que el binario cut se puede usar de esta forma.</h3>
<img width="842" height="189" alt="image" src="https://github.com/user-attachments/assets/70377c27-0209-48d5-868f-8d2224c1beaa" />
<h3>Buscando en los directorios de gladys, nos encontramos con el archivo chocolatitocontraseña.txt, pero vemos que este no es accesible, unicamente por el usuario chocolatito. Recordando que anteriormente habiamos visto que podiamos ejecutar cut con chocolatito entonces vamos a leer el contenido de este archivo.</h3>
<img width="629" height="136" alt="image" src="https://github.com/user-attachments/assets/f7003a6d-fbee-4722-be8f-a2c190d54a10" />
<img width="420" height="55" alt="image" src="https://github.com/user-attachments/assets/c7f53e23-39de-4668-a3dc-8176a5599120" />
<h3>Una vez tenemos la contraseña, ingresamos las credenciales del usuario chocolatito y obtenemos acceso.</h3>
<img width="275" height="59" alt="image" src="https://github.com/user-attachments/assets/23e46554-6cfa-431d-8490-164e1a400cc7" />

<h3>12. Volvemos a ejecutar el "sudo -l" y nos encontramos con que el usuario theboss puede ejecutar el binario awk.</h3>
<img width="1012" height="102" alt="image" src="https://github.com/user-attachments/assets/18ccd883-b434-44be-9c4f-a079ab80fa68" />
<h3>Buscando nuevamente en la pagina anterior, vemos que con el el binario awk podemos escalar permisos nuevamente.</h3>
<img width="846" height="186" alt="image" src="https://github.com/user-attachments/assets/e2b323a9-662b-43da-b62c-ffcbf605cde1" />
<img width="701" height="56" alt="image" src="https://github.com/user-attachments/assets/44674b75-9788-459a-a754-0994d1249d99" />

<h3>13. Ahora, con acceso al usuario theboss, volvemos a mirar que binarios se pueden ejecutar con "sudo -l" y vemos que hay un binario llamado sed.</h3>
<img width="750" height="136" alt="image" src="https://github.com/user-attachments/assets/667531b6-44b6-479b-8e2c-6362f0d08d26" />
<h3>Buscando en la pagina anterioremente mencionada, el binario sed se puede usar de la siguiente manera para obtener acceso al usuario root.</h3>
<img width="836" height="208" alt="image" src="https://github.com/user-attachments/assets/4de22e82-d747-4baa-9577-81322766f4cc" />
<h3>Usamos el comando anterior y observamos que ahora somos usuario root en la maquina!</h3>
<img width="677" height="52" alt="image" src="https://github.com/user-attachments/assets/e97141ce-d8ba-42ac-ab94-bc18d0e53020" />
