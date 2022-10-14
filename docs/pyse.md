# <FONT COLOR=#8B008B>Problema de funcionamiento de Pyserial</font>
El problema se detecta tras actualización de Ubuntu 20.04 LTS a 22.04 LTS que no reconoce la placa ESP32 STEAMakers desde ArduinoBlocks y no es posible programarla. Posteriormente se comprueba que pasa lo mismo en una instalación limpia de Ubuntu 22.04.

La primera sospecha recae en un demonio que incorpora la versión para tarjetas de lectura Braille, que se llama brltty. Se ejecuta desde una terminal:

~~~
sudo apt remove brltty
~~~

No se producen cambios referentes al problema.

Se piensa en esto porque  
~~~
ls /dev
~~~ 

no devuelve una entrada ttyUSB0 teniendo la placa conectada aunque *lsusb* si devuelve datos referentes al dispositivo y si se hace *lsmod* se comprueba que el dispositivo está cargado. Esto sucede por un conflicto entre las identificaciones de la placa ESP32 STEAMaker y el lector de pantalla Braille.

Mi situación de partida es:

~~~
python2 --version
Python 2.7.18
python3 --version
Python 3.10.6
~~~

Al intentar instalar el paquete pyserial recomendado por ArduinoBlocks para la placa con:

~~~
sudo dpkg -i python-pyserial_3.5-1_all.deb (pyserial de ArduinoBlocks)
~~~

La consola muestra un mensaje similar al siguiente:

~~~
Seleccionando el paquete python-pyserial previamente no seleccionado.
(Leyendo la base de datos ... 218964 ficheros o directorios instalados actualmente.)
Preparando para desempaquetar python-pyserial_3.5-1_all.deb ...
Desempaquetando python-pyserial (3.5-1) ...
dpkg: problemas de dependencias impiden la configuración de python-pyserial:
 python-pyserial depende de python (<< 2.8); sin embargo:
  El paquete `python' no está instalado.
 python-pyserial depende de python (>= 2.7); sin embargo:
  El paquete `python' no está instalado.
 python-pyserial depende de python:any (>= 2.6.6-7~).

dpkg: error al procesar el paquete python-pyserial (--install):
 problemas de dependencias - se deja sin configurar
Se encontraron errores al procesar:
 python-pyserial 
~~~

Indica que no está instalado el paquete python menor a la versión 2.8 y que tampoco lo está para una versión mayor a la 2.7 y que python-pyserial depende de los paquetes python.

<font color=#FF0000> En cambio al principio está clara la respuesta a las consultas y si que están instalados </font color>

Realizo un intento de hacer lo mismo con pyserial desintalado, haciendo:

~~~
sudo apt remove python3-serial
sudo apt autoclean && sudo apt autoremove
sudo apt update
sudo apt install python3-serial
~~~

Intento hacer lo siguiente:

~~~
sudo apt install python-serial python3-serial
python -m pip install pyserial
python3 -m pip install pyserial
~~~

Y la respuesta es que no puede localizar el paquete pyserial por lo que compruebo, ya que para poder usar pyserial hay que tener estos dos paquetes: python-serial y python3-serial

Siempre desde una terminal y con pyserial eliminado, aunque creo que esto da igual sigo la siguiente secuencia:

* Actualizar las alternativas de la lista de python con:

~~~
sudo update-alternatives --list python
update-alternatives: error: no hay alternativas para python y una lista ade directorios
~~~

* Pido listado de todo lo relacionado con python

~~~
ls /usr/bin/python*
/usr/bin/python2    /usr/bin/python3     /usr/bin/python3-futurize
/usr/bin/python2.7  /usr/bin/python3.10  /usr/bin/python3-pasteurize
~~~

Es decir que python está en el sistema.

* En estas condiciones hago:

~~~
sudo update-alternatives --install /usr/bin/python python /usr/bin/python2 2
sudo update-alternatives --install /usr/bin/python python /usr/bin/python3 1
sudo update-alternatives --config python
~~~

Y seleccionamos la alternativa que tenga python3

Si ahora hacemos:

~~~
python --version
~~~

la respuesta debe ser python 3.n.n

Ahora ya se puede instalar pyserial con pip3, reiniciar el equipo y que todo vuelva a funcionar sin problema.

~~~
pip3 install pyserial 
~~~

o bien

~~~
sudo dpkg -i python-pyserial_3.5-1_all.deb
~~~
