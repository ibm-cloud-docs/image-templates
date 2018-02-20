---
copyright:
  years: 2014, 2018
lastupdated: "2017-10-27"
---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:tip: .tip}

# Arranque de un VSI desde una imagen

La característica de arranque de imagen inicia una instancia de servidor virtual (VSI) mediante una plantilla ISO que se importa desde una cuenta de almacenamiento de objetos. {:shortdesc}

El arranque un servidor virtual desde una imagen lleva el dispositivo en línea de forma segura, y así pueden solucionarse los problemas. En la mayoría de los casos, la característica de arranque de imagen permite la resolución de problemas en un entorno sin arriesgar la pérdida de datos importantes que pueden ser experimentados en una recarga del SO. A pesar de que una pérdida de datos significativos es menos probable que una recarga del SO, se recomienda que realice copias de seguridad del dispositivo antes de iniciar el arranque. 

Realice los pasos siguientes para iniciar un servidor virtual desde una imagen.

1. Realice una copia de seguridad de todos los datos del dispositivo.
2. Desde el menú **Dispositivos** del [Portal de cliente ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://control.softlayer.com/), seleccione **Lista de dispositivos**. 
3. En la lista de dispositivos, pulse el servidor virtual que desea iniciar desde una plantilla ISO.
4. En la página de detalles del dispositivo para el servidor virtual, seleccione **Acciones > Iniciar desde imagen**.
5. Pulse **Iniciar desde esta imagen** para seleccionar la imagen deseada

  Alterne entre imágenes públicas y privadas mediante la función desplegable encima de la lista de imágenes.   {:tip}

6. Pulse **Arrancar desde imagen** para arrancar el dispositivo utilizando la imagen seleccionada. Pulse **Cerrar** para cancelar la acción.

## Siguientes pasos

Después de iniciar el proceso de arranque, la imagen se apaga y luego inicia mediante la imagen seleccionada. El tiempo de arranque varía, según el tamaño y el tipo de cada imagen varía. Después de que se inicie el servidor virtual utilizando la imagen seleccionada, se puede acceder como un dispositivo arrancado, pero se configura según la imagen. Reinicie el servidor virtual para apagar y devolver el dispositivo a su estado original.
