# rotor-antena
simple rotor de antena para TinyGS (tinygs.com)

# Introducción
Ver tinygs.com.  

El proyecto, ya operativo, consiste en un rotor controlado mediante un ESP8266, cuya misión es seguir al satelite que en esos momentos este
escuchando nuestra TinyGS  

Por ahora estoy utilizando un motor paso a paso de una vieja impresora, posteriormente lo cambiare por un motor paso a paso con desmultiplicador
100:1.  

Solo voy a mover el azimut de la antena, quedando para mas adelante la elevación de la misma.  

Para implementarlo, he modificado el programa de la GS, para que envie via GET (El ESP8266 tiene la IP 192.168.1.24 -> http://192.168.1.24/sat?sat=Norbi)  el nombre del satélite que esta escuchando al ESP8266 encargado de mover el rotor.  

Una vez conocido el satelite a seguir por el ESP8266, he utilizado la libreria https://github.com/Hopperpop/Sgp4-Library para obtener los datos de posición referentes al satelite escuchado.  

Los TLE´s de cada satelite, los actualizo automaticamente cada hora desde Celestrak: http://www.celestrak.com/satcat/tle.php?CATNR=46494 (por ejemplo, Norbi en este caso)  

# Vista del sistema 
![image](https://user-images.githubusercontent.com/48222471/117567192-4486f400-b0bb-11eb-8b01-1b8cee6842a2.png)

 
# Vista de la página web servida desde el ESP8266
![image](https://user-images.githubusercontent.com/48222471/117567409-8bc1b480-b0bc-11eb-89ca-9d89e79c7466.png)

