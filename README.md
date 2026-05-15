# Simulación de ataque Ataque-Evil-Twin-contra-WPA2-Enterprise 
El fin de esta práctica es simular un ataque Evil Twin contra una red WPA2 Enterprise dentro de un entorno corporativo controlado, con el objetivo de:

- Demostrar vulnerabilidades comunes presentes en redes WiFi empresariales mal configuradas.
- Entrenar a analistas forenses y personal técnico en la detección y mitigación de ataques inalámbricos.
- Analizar técnicas de suplantación de puntos de acceso (Evil Twin) y captura de credenciales WPA2 Enterprise mediante MSCHAPv2.
- Comprender el funcionamiento de protocolos de autenticación EAP y servidores RADIUS en entornos corporativos.

## Prerequisitos
- OS: Ubuntu
  - Herramienta: Hostapd

**INSTALACIÓN**

  Instalamos hostapd-wpe en kali Linux con el comando “sudo apt install hostapd-wpe”.

  <img width="517" height="497" alt="image" src="https://github.com/user-attachments/assets/f1a7fc11-a8c7-4cbb-abba-2c08b2e58c9f" />


**CONFIGURACIÓN**

  Usamos el comando iwconfig para ver qué interfaces inalámbricas reconoce el sistema. Al ejecutarlo, podemos identificar fácilmente wlan0, interfaz asociada a la tarjeta Wi‑Fi.

  <img width="592" height="190" alt="image" src="https://github.com/user-attachments/assets/743717b1-54c9-456a-bc91-28bbf7940a3a" />


  Luego editamos su fichero de configuración etc/hostapd-wpe/hostapd-wpe.conf

  <img width="406" height="40" alt="image" src="https://github.com/user-attachments/assets/b3eb525d-0cb3-4d64-85bf-a2a1cc30fc22" />


  En el archivo /etc/hostapd-wpe/hostapd-wpe.conf modificamos la configuración para que el SSID pase a llamarse WifiGuillermo, establecemos el canal en 1 y definimos wlan0 como la interfaz que utilizará la tarjeta de red.

  <img width="597" height="550" alt="image" src="https://github.com/user-attachments/assets/c902410f-f69e-42f2-adb7-0a6dbd56c495" />


  Creamos el archivo evil_twin.conf, donde definimos la configuración del punto de acceso falso llamado "WIFIGuillermo". Este punto de acceso opera en el canal 1 y utiliza el protocolo WPA‑EAP (Enterprise) para redirigir   las solicitudes de autenticación hacia un servidor RADIUS local en el puerto 1812, con el objetivo de capturar las credenciales que los clientes intenten enviar al conectarse.

  <img width="552" height="200" alt="image" src="https://github.com/user-attachments/assets/0e5df980-ace3-487b-a3e4-73942d50ef6c" />



  Ejecutamos el ataque Evil Twin utilizando hostapd‑wpe
  La herramienta ha capturado la identidad del usuario “guillermo”, enviada por un dispositivo que intentó conectarse al punto de acceso falso.También se han registrado el desafío y la respuesta del protocolo MSCHAPv2,      generando automáticamente hashes compatibles con herramientas de descifrado como John the Ripper y Hashcat.

<img width="485" height="425" alt="image" src="https://github.com/user-attachments/assets/28ad5733-0aa3-4f26-ae26-53edaa9c4126" />


  Volcamos el hash capturado en el archivo hasht.txt, el cual actúa como el "objetivo" que Hashcat procesará posteriormente. 
  
  <img width="605" height="37" alt="image" src="https://github.com/user-attachments/assets/0d55188b-458b-4a57-84ea-9416892639bf" />


  Creamos el archivo que funcionará como diccionario de palabras, donde incluimos una lista de posibles contraseñas. Este fichero será utilizado por Hashcat durante el ataque de diccionario para comparar cada una de esas   palabras con el hash capturado y determinar si alguna coincide.

  <img width="596" height="172" alt="image" src="https://github.com/user-attachments/assets/4daab869-c91b-463c-8f19-22b4c6606d76" />


  Procedemos a crackear la contraseña capturada previamente utilizando la herramienta Hashcat. Para ello, Hashcat emplea el archivo pruebas.txt, que contiene una lista de posibles contraseñas, y las compara con el hash      almacenado en hasht.txt.
  Tras completar el proceso, verificamos que la contraseña ha sido descifrada correctamente: “guillermo”. Esto confirma que una de las palabras del diccionario coincidía con el hash obtenido durante el ataque.
  
  <img width="455" height="377" alt="image" src="https://github.com/user-attachments/assets/b9cf8c0a-315f-49b8-9d35-1bc5424d373a" />


  Bibliografía:
  
  https://elbinario.net/2019/06/20/evil-twin-attack-wpa2-enterprise/

  https://myhackingnotes.com/wifi/wpa2_enterprise/


  Documento:[Ataque Evil Twin contra WPA2 Enterprise_Guillermo_Hervás.docx.pdf](https://github.com/user-attachments/files/27762070/Ataque.Evil.Twin.contra.WPA2.Enterprise_Guillermo_Hervas.docx.pdf)

