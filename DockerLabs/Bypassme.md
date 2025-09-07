<h1>Maquina BypassMe</h1>

<h3>1. Corremos la maquina en Docker con "./auto_deploy.sh bypassme.tar"</h3>
<img width="499" height="344" alt="image" src="https://github.com/user-attachments/assets/7da18cb2-f932-4029-8c3b-198e7ebcc9e8" />

<h3>2. Hacemos prueba de conectividad con "ping 172.17.0.2"</h3>
<img width="510" height="177" alt="image" src="https://github.com/user-attachments/assets/29fee3af-4845-40dd-a121-239c9e9b44c0" />

<h3>3. Escaneamos los puertos de la URL y nos damos cuenta que el puerto 22 y el puerto 80 estan abiertos.</h3>
<img width="527" height="208" alt="image" src="https://github.com/user-attachments/assets/261883da-b62d-46cc-9b22-9ae7c2f71d5e" />

<h3>4. Al entrar al puerto 80 (http) nos encontramos con un panel de login, revisando el codigo fuente no encontramos nada interesante</h3>
<img width="471" height="392" alt="image" src="https://github.com/user-attachments/assets/3954de9d-b5f7-4f27-8c4e-1caac3dc9fa7" />

<h3>5. Intentamos acceder haciendo una inyeccion SQL poniendo los siguientes parametros: <br>User: admin <br>Password: admin OR '1'='1' -- -</h3>
<img width="680" height="427" alt="image" src="https://github.com/user-attachments/assets/7483d3e7-fbdf-4e93-a2c6-ef20b6bec9ba" />

<h3>6. Una vez dentro, nos damos cuenta que la carpeta logs esta publica (lo dice la informacion en pantalla), y en el codigo fuente nos dicen que aun no se ha asegurado el archivo "logs.txt", por ende vamos a obtener acceso a esta carpeta por medio de cambiar la informacion del link.</h3>
<img width="278" height="21" alt="image" src="https://github.com/user-attachments/assets/b31f3145-e10e-4a6d-87af-a0029604107d" />
<img width="490" height="290" alt="image" src="https://github.com/user-attachments/assets/46387d43-fd63-47ac-9e04-0172f3d89456" />

<h3>7. Revisando en el archivo "logs.txt", nos encontramos con que hay un inicio de sesion por medio de ssh con el usuario "albert" y su respectiva contraseña.</h3>
<img width="647" height="386" alt="image" src="https://github.com/user-attachments/assets/4d4482f1-b0e7-45f3-8327-fb81d7a42239" />

<h3>8. Luego, intentamos hacer una conexion ssh con las credenciales obtenidas obteniendo asi acceso al usuario albert.</h3>
<img width="589" height="243" alt="image" src="https://github.com/user-attachments/assets/38e987c9-2c51-4c56-b60d-ac739019f2e1" />

<h3>9. Buscando archivos dentro del directorio del usuario y no encontramos nada interesante. Tambien nos damos cuenta que el usuario albert no puede usar sudo.</h3>
<img width="486" height="189" alt="image" src="https://github.com/user-attachments/assets/747bdd80-10ae-4411-ba26-6abdd2b5f625" />

<h3>10. Revisando los procesos activos con "ps aux | cat", nos encontramos con que hay un proceso del usuario conx que ejecuta un comando socat que devuelve una bash, por ende, intentaremos acceder a este cambiando los parametros del comando (listen por connect).</h3>
<img width="1038" height="320" alt="image" src="https://github.com/user-attachments/assets/55286aba-5f25-41b2-8666-f16c2118d658" />

<h3>11. Una vez hecho esto, obtenemos acceso al usuario conx en una bash.</h3>
<img width="544" height="51" alt="image" src="https://github.com/user-attachments/assets/b8922988-1fba-412f-abd3-38406fdd2d10" />

<h3>12. Volvemos a listar los procesos activos y nos encontramos con que root (a donde queremos acceder) esta ejecutando un proceso llamado cron que sirve para gestionar automanizaciones en Linux.</h3>
<img width="1033" height="371" alt="image" src="https://github.com/user-attachments/assets/960315cd-b642-4ad2-baf6-8d6788f812f1" />

<h3>13. Accediendo a los procesos activos que tiene cron en el directorio nos encontramos con que hay un archivo llamado backup-cron que se ejecuta periodicamente. Al hacerle cat, nos muestra la ubicacion del script y que se ejecuta con root."</h3>
<img width="432" height="327" alt="image" src="https://github.com/user-attachments/assets/a3e80a0e-a83a-4d79-9fae-8a4e08632b2d" />


<h3>14. Reemplazamos el script de backup-cron por un script que nos de permisos SUID a "/bin/bash"</h3>
<img width="568" height="305" alt="image" src="https://github.com/user-attachments/assets/3453d00a-be82-4173-8c32-48c5ab004b32" />

<h3>15. Luego de esto, una vez se ejecute el script con cron, podremos ver que ahora somos root en bash y elevando privilegios de root con "bash -p" nos convertimos en el usuario root.</h3>
<img width="430" height="86" alt="image" src="https://github.com/user-attachments/assets/f7892117-1cfc-4a40-ab6a-d2b58d13e953" />

<h3>Conclusiones: Debemos evitar los procesos cron con privilegios de ejecucion y debemos proteger siempre el login para evitar inyecciones SQL.</h3>
