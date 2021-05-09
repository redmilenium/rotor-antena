# rotor-antena
simple rotor de antena para TinyGS (tinygs.com)

# Introducción
Ver tinygs.com.  

El proyecto, ya operativo, consiste en un rotor controlado mediante un ESP8266, cuya misión es seguir al satelite que en esos momentos este
escuchando nuestra TinyGS  

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
![image](https://user-images.githubusercontent.com/48222471/117582400-255f8500-b102-11eb-92a3-65a39cbca0f6.png)
![image](https://user-images.githubusercontent.com/48222471/117582414-34dece00-b102-11eb-8a1d-5b62fcdbd1a9.png)
 
# Vista de la página web servida desde el ESP8266
![image](https://user-images.githubusercontent.com/48222471/117567409-8bc1b480-b0bc-11eb-89ca-9d89e79c7466.png)


Desde aquí puedes seleccionar que el rotor siga automaticamente al satelite que este seleccionado en la TinyGS.


El programa esta configurado para que el seguimiento se haga cuando el satelite este a menos de 3000 Km., por encima de esa distancia es muy complicado que lleguemos a recibir algo...

También podemos ponerlo en manual, y mover el rotor a la posición que nos interese.

