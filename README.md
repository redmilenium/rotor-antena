# Rotor antena
simple rotor de antena para TinyGS (tinygs.com)

# Introducción
Ver tinygs.com.  

El proyecto, ya operativo, consiste en un rotor controlado mediante un ESP8266, cuya misión es seguir al satelite que en esos momentos este
escuchando nuestra TinyGS.
He procurado "tocar" lo menos posible el soft de la TinyGS para no interferir en su trabajo y que el sistema encargado de rotar sea un ESP8266 que se encargue del trabajo de calcular el azimut y del movimiento del rotor.

# TINYGS HOMEMADE
![image](https://user-images.githubusercontent.com/48222471/117586079-2bf7f780-b116-11eb-84cd-9ce9a3a5349e.png)

# ESPECIFICACIONES ROTOR
Por ahora estoy utilizando un motor paso a paso de una vieja impresora, posteriormente lo cambiare por un motor paso a paso con desmultiplicador
100:1.  

Solo voy a mover el azimut de la antena, quedando para mas adelante la elevación de la misma.  

Para implementarlo, he modificado el programa de la GS, para que envie via GET (El ESP8266 tiene la IP 192.168.1.24 -> http://192.168.1.24/sat?sat=Norbi)  el nombre del satélite que esta escuchando al ESP8266 encargado de mover el rotor.  

Una vez conocido el satelite a seguir por el ESP8266, he utilizado la libreria https://github.com/Hopperpop/Sgp4-Library para obtener los datos de posición referentes al satelite escuchado.  

Los TLE´s de cada satelite, los actualizo automaticamente cada hora desde Celestrak: http://www.celestrak.com/satcat/tle.php?CATNR=46494 (por ejemplo, Norbi en este caso)  

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
[rotor.zip](https://github.com/redmilenium/rotor-antena/files/6460477/rotor.zip)

# Material necesario





Continuará...en breve subiré el programa gestor del rotor, además de las piezas necesarias fabricadas en la impresora 3D.

