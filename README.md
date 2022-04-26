# Rotor antena
simple rotor de antena para TinyGS (tinygs.com)

# Introducción
Ver tinygs.com.  

El proyecto, ya operativo, consiste en un rotor controlado mediante un ESP8266, cuya misión es seguir al satelite que en esos momentos este
escuchando nuestra TinyGS.
He procurado "tocar" lo menos posible el soft de la TinyGS para no interferir en su trabajo y que el sistema encargado de rotar sea un ESP8266 que se encargue del trabajo de calcular el azimut y del movimiento del rotor.


# TINYGS HOMEMADE
![image](https://user-images.githubusercontent.com/48222471/117852716-e9eec300-b287-11eb-8d8b-245ba6786273.png)

# ESPECIFICACIONES ROTOR
Por ahora estoy utilizando un motor paso a paso de una vieja impresora, posteriormente lo cambiare por un motor paso a paso con desmultiplicador
100:1.  

Solo voy a mover el azimut de la antena, quedando para mas adelante la elevación de la misma.  

Para implementarlo, he modificado el programa de la GS, para que envie via GET (El ESP8266 tiene la IP 192.168.1.24 -> http://192.168.1.24/sat?sat=Norbi)  el nombre del satélite que esta escuchando al ESP8266 encargado de mover el rotor.  

Una vez conocido el satelite a seguir por el ESP8266, he utilizado la libreria https://github.com/Hopperpop/Sgp4-Library para obtener los datos de posición referentes al satelite escuchado.  

Los TLE´s de cada satelite, los actualizo automaticamente cada hora desde Celestrak: http://www.celestrak.com/satcat/tle.php?CATNR=46494 (por ejemplo, Norbi en este caso)  

![image](https://user-images.githubusercontent.com/48222471/117855623-f1fc3200-b28a-11eb-84a6-347b1d3e238a.png)


# Vista del sistema 
![image](https://user-images.githubusercontent.com/48222471/117701870-0b887580-b1c8-11eb-90a5-3a4babf89063.png)

![image](https://user-images.githubusercontent.com/48222471/117582414-34dece00-b102-11eb-8a1d-5b62fcdbd1a9.png)
 
# Vista de la página web servida desde el ESP8266
![image](https://user-images.githubusercontent.com/48222471/117567409-8bc1b480-b0bc-11eb-89ca-9d89e79c7466.png)


Desde aquí puedes seleccionar que el rotor siga automaticamente al satelite que este seleccionado en la TinyGS.


El programa esta configurado para que el seguimiento se haga cuando el satelite este a menos de 3000 Km., por encima de esa distancia es muy complicado que lleguemos a recibir algo...
Una vez que el satelite este dentro de este margen de 3000 Km. el ESP8266 calcula el azimut del mismo con respecto a nuestra posición y comenzara a orientar la antena. La posición se actualiza cada segundo. Es muy importante que durante la instalación, orientemos perfectamente los 0 grados del rotor con el Norte.

También podemos ponerlo en manual, y mover el rotor a la posición que nos interese.

# Modificación a incluir en el soft de TINYGS  https://github.com/G4lile0/tinyGS 


[añadidotinygs.txt](https://github.com/redmilenium/rotor-antena/files/6453774/anadidotinygs.txt)

![image](https://user-images.githubusercontent.com/48222471/117697320-9f574300-b1c2-11eb-9f0f-eeb7b1928cb5.png)

Se envia el nombre del satelite escuchado una vez por minuto.

![image](https://user-images.githubusercontent.com/48222471/117697616-f3fabe00-b1c2-11eb-801f-5e43a9c3276c.png)


# Esquema

pinMode(D5, OUTPUT);       //DIRECCION GIRO 

pinMode(D6, OUTPUT);       //PULSO PARA DAR UN PASO

pinMode(D7, OUTPUT);       //ENABLE

pinMode(D2, INPUT);       //DETECTOR

![image](https://user-images.githubusercontent.com/48222471/117826333-5eb60300-b270-11eb-928d-8bab5ac784fd.png)

# Archivos 3D
[rotor3D_printer.zip](https://github.com/redmilenium/rotor-antena/files/6461855/rotor3D_printer.zip)

# Material necesario
- Una TinyGS. Puedes hacerlo tu mismo o puedes adquirirla hecha.
- Cajas para conexiones electricas. Son necesarias 2: una para la TinyGS y otra para el rotor y asociados.
- Motor paso a paso. He utilizado uno reciclado de una impresora y que gira directamente el eje del rotor. 
  Dado que el controlador de motor paso a paso lo tengo configurado a 1600 pasos por vuelta, obtengo una relación de 4.44 pulsos por grado. 
  Estoy a la espera de recibir un nema17 con un gearbox con una relación 100:1:
  ![image](https://user-images.githubusercontent.com/48222471/117849127-464fe380-b284-11eb-8574-328bca941831.png)
  
  Es decir por cada 100 vueltas del motor paso a paso, el rotor dará solo 1. 
  En este caso configuraré el controlador de motor p.p. a 200 pulsos por vuelta. 
  Esto son 20.000 pulsos de motor pap por vuelta de rotor: 55.55 pulsos por grado.
- Un acoplador que una el motor paso a paso con el eje del rotor:

  ![image](https://user-images.githubusercontent.com/48222471/117849594-c0806800-b284-11eb-8fcb-0d31d48d27bb.png)
  
  Las medidas del acoplador serán acordes al diametro del eje del motor paso a paso y del eje del rotor.
  
  Yo he utilizado un eje de aluminio para el rotor de 10 mm. de diametro que se puede adquirir en  Leroy Merlin, al igual que los herrajes necesarios, las cajas electricas,etc.
- Una placa de CI para prototipos. En ella instalaré el ESP8266, el convertidor DC/DC y la resistencia para el detector HALL.
- Tornilleria varia, ademas de brocas, remaches, cables, regletas para conexiones electricas y una fuente de 24 voltios de c.c.

# Platformio

Recomiendo disponer de platformio para la carga del programa en el ESP8266. 
Para instalarlo puedes utilizar cualquiera de los tutoriales existentes en Youtube y que lo hacen francamente bien.
Importante, utilizar las siguientes versiones;
PLATFORM: Espressif 8266 (2.6.2) > WeMos D1 R2 and mini
HARDWARE: ESP8266 240MHz, 80KB RAM, 4MB Flash
PACKAGES:
 - framework-arduinoespressif8266 3.20704.0 (2.7.4)    
 - tool-esptool 1.413.0 (4.13)
 - tool-esptoolpy 1.20800.0 (2.8.0)
 - toolchain-xtensa 2.40802.200502 (4.8.2)

Respecto al programa, estoy modificandolo un poco, ya que yo lo tengo integrado en el control domotico de mi casa y presenta ciertas particularidades.

Estoy seguro de que quedan bugs por solucionar, pero presenta un comportamiento bastante aceptable.

Y aunque el sistema lo tengo instalado en un tejado, no tengo visión de 360 grados sin obstaculos y aun asi y en la parte "mala" obtengo buenos resultados:
///////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////
- 14:03:47 success!
- 14:03:47 [SX12x8] Starting to listen to Norbi
- 14:03:47 success!
- 14:16:08 [SX12x8] RSSI:		-121.500000 dBm
- [SX12x8] SNR:		-8.500000 dB
- [SX12x8] Frequency error:	-8415.871094 Hz
- 14:16:08 Packet (143 bytes):
- 14:16:08 - 8effffffff0a0601c9cd2700000000f10f000047cd0b322d2842524b204d57205645523a30325f31320000000000000e0000ec0700000002100000e60a8600f003000082202001609efec6ffd6606f01cb0de60106322d2819001000e8ff38000000000000000904040f0f0f0f0f0f000702bffcb2600875300e0c000c0000
- 14:16:08 a5102523f30c060904006010e21f4d15
- 14:18:08 [SX12x8] RSSI:		-119.000000 dBm
- [SX12x8] SNR:		-8.000000 dB
- [SX12x8] Frequency error:	-1784.676392 Hz
- 14:18:08 Packet (143 bytes):
- 14:18:08 8effffffff0a0601c9cd2900000000f10f000049cd83322d2842524b204d57205645523a30325f31320000000000000e0000ec0700000002100000e50a8600f003000082202001609efec6ffd6606f01cb0de6017f322d281500feff18002f000000000000000a04040f0f0f0f0f0f000a03bffcb2600a91300e0c000c0000
- 14:18:08 c4233825ed0c070a05006010f91f1572
- 14:20:08 [SX12x8] RSSI:		-126.500000 dBm
- [SX12x8] SNR:		-14.500000 dB
- [SX12x8] Frequency error:	6616.514648 Hz
- 14:20:08 [SX12x8] CRC error! Data cannot be retrieved
- 15:52:17 [SX12x8] RSSI:		-123.000000 dBm
- [SX12x8] SNR:		-10.000000 dB
- [SX12x8] Frequency error:	-3978.297363 Hz
- 15:52:17 Packet (143 bytes):
- 15:52:17 8effffffff0a0601c9cd8700000000f10f0000a7cd95482d2842524b204d57205645523a30325f31320000000000000e0000ec0700000002110000e60a86001804000082202000609efec6ffd6606f01cb0de60190482d281e00e0ffeaff38000000000000000904040f0f0f0f0f0f000107bffcb260087f300e0c000c0000
- 15:52:17 d7f47007df0c060804006010b01fe157
- 15:54:18 [SX12x8] RSSI:		-125.000000 dBm
- [SX12x8] SNR:		-11.000000 dB
- [SX12x8] Frequency error:	3542.089844 Hz
-15:54:18 Packet (143 bytes):
- 15:54:18 8effffffff0a0601c9cd8900000000f10f0000a9cd0d492d2842524b204d57205645523a30325f31320000000000000e0000ec0700000002140000e30a86001804000082202001609efec6ffd6606f01cb0de60108492d2822000800d8ff33000000000000000904040f0f0f0f0f0f000407bffcb2600a93300e0c000c0000
- 15:54:18 e917d223af0d070a05006010f01f0a63
- 15:56:18 [SX12x8] RSSI:		-122.000000 dBm
- [SX12x8] SNR:		-10.000000 dB
- [SX12x8] Frequency error:	8042.578125 Hz
- 15:56:18 Packet (143 bytes):
- 15:56:18 8effffffff0a0601c9cd8b00000000f10f0000abcd85492d2842524b204d57205645523a30325f31320000000000000e0000ec0700000002120000e00a86001804000082202001609efec6ffd6606f01cb0de60180492d281b000700120031000000000000000a04040f0f0f0f0f0f000209bffcb2600aa3300e0c000c0000
- 15:56:18 40311e36ee0c070a060060101e202c43
/////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////

# Configurando el programa
- Repito: recomiendo utilizar platformio para su carga.
  Por un lado hay que cargar el software y por otro la imagen que se ve de fondo en la página web que sirve. Todo esto se hace desde platformio
- Debes configurar en el programa tu SSID y tu password. Tienes la posibilidad de incluir 2 AP´s.
- Tambien debes poner la latitud, longitud y altura de donde se va a situar el rotor.
- Yo he utilizado la IP 192.168.1.24 para el rotor.  Si necesitas cambiarla, debes tener en cuenta que hay que hacerlo en varios puntos de programa.
  Nada que alguien como tu no pueda hacer!!!
  Verifica que la IP a utilizar ( en mi caso 192.168.1.24) este fuera del rango de IP´s que el DHCP de tu router este asignando, para evitar conflictos...
  Al acceder a la web (192.168.1.24 en mi caso) te solicita usuario y password:  admin  y abracadabra. Estan en el programa y si no te gustan tambien puedes cambiarlos.
- Tanto la TinyGS como el ESP8266 del rotor, deben estar en la misma red... 192.168.1.xx en mi caso
- Por ahora solo he incluido 4 satelites: Norbi, SDSat, FEES y Sri Shakthi Sat. 
  Yo solo he llegado a recibir datos de los 3 primeros y conociendo el número NORAD es muy facil incluir cualquier otro: http://www.celestrak.com/satcat/tle.php?CATNR=46494
  Este, por ejemplo, nos permite conseguir los TLE's actualizado de Norbi
- Dado que despues de su instalación en un tejado, por ejemplo, no tendras facil acceso al ESP8266, tambien esta activa la posibilidad de enviar via OTA nuevas versiones del software. Para ello deberás editar el archivo platformio.ini, comentar la linea de COMX y descomentar las referentes a OTA:


  ![image](https://user-images.githubusercontent.com/48222471/117863090-ae59f600-b293-11eb-95e9-90c4ce90b0e6.png)
  
- En el directorio dejo el programa para quien quiera usarlo: rotor.zip (descomprimirlo y desde platformio: open folder y grabar en el ESP8266).

  11/06/2021: ya esta instalado el nuevo motor paso a paso con desmultiplicación 100:1
    
![100_1](https://user-images.githubusercontent.com/48222471/122085710-4606b300-ce03-11eb-9a80-0f9f67350f73.jpeg)

  Ya esta operativo, pero todavía en pruebas. 
  
  Ahora puede mover antenas de mayor peso y con un movimiento mas suave. 
  
  Por ahora, los saltos son de 1 grado...
  
 - 17/02/2022 
 - Se limpia, fija y da esplendor al soft. Ahora los saltos son de centesimas de grado. Recuerda orientar correctamente a 0 grados la antena.
 - He cambiado el sensor de cero: antes era un detector de efecto Hall y ahora es un SW que tiene un funcionamiento mucho mas adecuado.
 - En breve subiré fotos.
  
  
![mpp sw1](https://user-images.githubusercontent.com/48222471/165341632-bf61b2e4-3fcc-4b06-80e6-baefd147bd20.jpeg)


