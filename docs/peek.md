# <FONT COLOR=#8B008B>Peek no graba el cursor</font>
En Ubuntu 22.04 he estado teniendo un problema con la creación de gif animados mediante la aplicación Peek y es que, aunque la opción está claramente marcada en las preferencias, este realmente no aparece en las grabaciones.

En la imagen vemos las preferencias de Peek

<center>

![Preferencias de Peek](./img/peek/pref.png)  
*Preferencias de Peek*

</center>

La situación de referencia en cuanto a versiones de Gnome, Peek y tipo de XDG utilizado los vemos a continuación:

<center>

![Versiones y XDG](./img/peek/ver.png)  
*Versiones y XDG*

</center>

Los comandos utilizados son:

~~~shell
gnome-shell --version
peek -v
echo $XDG_SESSION_TYPE
~~~

La solución que he encontrado sin tocar casi nada de configuraciones es activar el magnificador. Para ello abrimos la Configuración y nos dirigimos a accesibilidad y activamos Ampliación:

<center>

![Opciones de accesibilidad](./img/peek/opc.png)  
*Opciones de accesibilidad*

</center>

Que para que no afecte a la visualización he configurado así:

<center>

![Opciones de Ampliación](./img/peek/ampl.png)  
*Opciones de Ampliación*

</center>