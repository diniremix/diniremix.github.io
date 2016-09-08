---
title: "Instalar Kazam en Korora 24"
tags: [korora]
---


<a data-flickr-embed="true" data-context="true"  href="https://www.flickr.com/photos/47496520@N05/28908063724/in/dateposted-public/" title="kazam1"><img src="https://c1.staticflickr.com/9/8372/28908063724_160041bce3_o.png" width="481" height="335" alt="kazam1"></a>


[**Kazam**](https://launchpad.net/kazam) proporciona una interfaz bien diseñada y muy fácil de usar para grabar y hacer capturas de pantalla.

Se puede grabar vídeo y audio del escritorio en múltiples canales simultáneamente. Cuenta con un control de los niveles de audio y permite tambien seleccionar una región de la pantalla para ser capturada.

El Soporte para códecs H264 y VP8 está integrado por defecto.

Aunque existen numerosos programas para realizar esta labor en Gnu/Linux, Kazam brilla por su simplicidad y potencia a la hora de hacer un screencast de tu escritorio.

<br>
<br>

### Instalando Kazam en Korora 24:

Agregamos el repositorio de [**home:Kenzy:packages/kazam**](http://software.opensuse.org/download.html?project=home%3AKenzy%3Apackages&package=kazam) para fedora 24
{% highlight python %}
dnf config-manager --add-repo http://download.opensuse.org/repositories/home:Kenzy:packages/Fedora_24/home:Kenzy:packages.repo
{% endhighlight %}

Realizamos la instalación:

{% highlight python %}
dnf install kazam
{% endhighlight %}

A la hora de escribir estas líneas, Kazam se encuentra en su [versión **1.4.5**](https://launchpad.net/~kazam-team/+archive/ubuntu/stable-series)

listo!.

<a data-flickr-embed="true" data-context="true"  href="https://www.flickr.com/photos/47496520@N05/29533150345/in/dateposted-public/" title="kazamkorora"><img src="https://c2.staticflickr.com/9/8101/29533150345_e975c9bdf2_z.jpg" width="640" height="360" alt="kazamkorora"></a>

<br>
<br>

### **Usando Kazam en Korora 24**
<iframe width="560" height="420" src="http://www.youtube.com/embed/3DRzO9zz5xE?color=white&theme=light"></iframe>

<br>
<br>


### Problemas comunes

En el caso de Korora 24, Kazam puede no funcionar bien y lanzar un error al iniciar, como se aprecia en la siguiente imagen.

<a data-flickr-embed="true" data-context="true"  href="https://www.flickr.com/photos/47496520@N05/29499414116/in/dateposted-public/" title="kazam-error"><img src="https://c5.staticflickr.com/9/8692/29499414116_68767cc930_z.jpg" width="640" height="360" alt="kazam-error"></a>

Esto es debido posiblemente a un cambio en la versión de python usada (**3.5.1** para este caso), para solucionar esto, basta con editar los archivos: 

- **/usr/bin/kazam** y seguidamente
- **/usr/lib/python3.5/site-packages/kazam/backend/config.py**

Con tu editor favorito editamos primeramente, el archivo **/usr/bin/kazam** y agregar las siguientes líneas antes de:

{% highlight python %}
...
from gi.repository import Gtk
...
{% endhighlight %}


Quedando de la siguiente manera:

{% highlight python %}
...
import gi
gi.require_version("Gtk", "3.0")
from gi.repository import Gtk
...
{% endhighlight %}

Guardamos los cambios y a continuación editamos el archivo **/usr/lib/python3.5/site-packages/kazam/backend/config.py** 

Ubicamos el metodo get y lo reescribiremos.

**Antes:**
{% highlight python %}
def get(self, section, key):
    try:
        return ConfigParser.get(self, section, key)
{% endhighlight %}

**Después:**
{% highlight python %}
def get(self, section, key, raw=True, fallback='rest'):
    try:
        return super(KazamConfig,self).get(section, key, raw=raw, fallback=fallback)
{% endhighlight %}

Estos cambios funcionan bien en Korora 24

El fallo expuesto aquí, fué [reportado y solucionado](https://bugs.launchpad.net/ubuntu/+source/kazam/+bug/1500083) en la sección bugs del proyecto, estamos atentos a una actualización proximamente.
