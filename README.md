# edr_vehicular
##Proyecto de Trabajo Terminal realizado por: José Acosta, Sergio Paniagua y Carlos Parra.

**El proyecto es un prototipo de sistema de IOT en el que, un SenseHat conectado a una Raspberry sensa datos de aceleración del vehículo en el que se encuentra, para posteriormente ser analizados y filtrados, de manera que cuando se detecte una anomalía, esta información se envíe de manera cifrada utilizando el protocolo HTTPS a una máquina virtual en Azure, capaz de recepcionar, descifrar y calcular la probabilidad de que el archivo de datos recibido pertenezca a un incidente o a un movimiento normal del vehículo.**
Este está implimentado por segmentos, de forma que cada una de las partes utilizadas puedan ser implementadas en diferentes proyectos para su utilidad.

*imagen*

El prototipo se compone principalmente de tres etapas:

## Etapa de Sensado

Esta etapa se compone de hardware y software, donde se encuentra una Raspberry Pi 4 con el sistema operativo Raspberry Pi OS e integrando un módulo de sensado SenseHAT.
Dentro de esta, se diseñaron dos scripts en Python, el primero...

### Sensado.py

Este script cuenta con todas las bibliotecas necesarias para utilizar el módulo SenseHAT adecuadamente para realizar las lecturas de los sensores, además de funcionar como un primer filtro de la información del vehículo.
Este script monitorea constantemente las lecturas de los sensores y en el momento en el que se sobrepasan ciertos umbrales establecidos, la información es cifrada utilizando AES-256 y se almacena en un archivo dentro de la Raspberry.

*imagen*

## Etapa de Transferencia

Esta etapa se compone de dos scripts realizados en Python y que permiten realizar una conexión segura entre la Raspberry y la máquina virtual en Azure, verificando que el contenido llegue de forma íntegra y adecuada.

### cliente.py

Este script se encuentra dentro de la Raspberry y se encarga de crear un socket seguro para posteriormente, realizar la conexión al servidor de la máquina virtual. Para lograr esto, previamente se crearon certificados auto-firmados que permitan verificar las conexiones establecidas, para poder proceder a enviar el archivo cifrado en bloques de datos hasta verificar que el archivo haya llegado completo.

### servidor.py

Este script se encuentra dentro de la máquina virtual en Azure y permite que esta esté en constante búsqueda de conexiones seguras, verificando los certificados de los intentos de conexión. Una vez establecida, se recibe el archivo en bloques junto con su nombre y este se descifra y almacena en una carpeta específica de la máquina virtual.

*imagen*

## Etapa de Análisis

Esta etapa se realiza únicamente en la máquina virtual, utilizando un modelo de IA, el cual es un Clasificador Binario Bayesiano, que a través de aprendizaje supervisado, se entrena para poder estar listo para analizar correctamente la información de los archivos a recibir.

Al realizar diferentes cálculos de los valores de cada parámetro registrado en el vehículo, se obtienen dos probabilidades, una indicando la probabilidad de que el archivo analizado sea una **colisión** y la segunda, indicando la probabilidad de que el archivo haya sido un **comportamiento normal** del vehículo.

*imagen*

##Importancia

Este proyecto fue de gran utilidad para poner en práctica todas las habilidades que se han adquirido a lo largo de la carrera, permitiéndonos realizar análisis de datos recibidos de diferentes sensores, uso de hardware y software de manera conjunta, desarrollo de scripts y modelos que permitan filtrar y analizar los datos recibidos, desarrollo de una arquitectura en la nube, preparación de diferentes sistemas operativos para poder correr diferentes scripts de manera conjunta, crear conexiones entre distintos dispositivos y despertar nuestra creatividad para encontrar una solución a un problema de visualización de los datos de un vehículo para poder observar lo que pasa durante un incidente vehicular.

*imagen*



