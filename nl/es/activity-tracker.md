---

copyright:
  years: 2018
lastupdated: "2018-08-09"

keywords:

subcollection: image-templates

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:table: .aria-labeledby="caption"}

# Auditoría de sucesos de servidor virtual con Activity Tracker
{: #auditing-virtual-server-events-with-activity-tracker}

Puede auditar una instancia de servidor virtual a través de su ciclo de vida mediante [Activity Tracker](/docs/services/cloud-activity-tracker?topic=cloud-activity-tracker-activity_tracker_ov). Debe tener una instancia de Activity Tracker con el plan de servicio premium suministrado en EE.UU. sur. El plan premium le proporciona acceso al panel de control de Kibana que tiene más opciones para filtrar y buscar registros de auditoría.

Los registros son visibles en la sección de registros de cuenta del propietario de la cuenta de {{site.data.keyword.cloud}}. El propietario de la cuenta puede ver los registros siguientes:
* Sucesos desencadenados por el propietario de la cuenta.
* Sucesos desencadenados por los usuarios que están enlazados con la cuenta.

Puede auditar los siguientes sucesos de instancia de servidor virtual a través de Activity Tracker:
* Suministro de una instancia de servidor virtual
* Inhabilitación de un puerto para una interfaz privada o pública
* Habilitación de un puerto para una interfaz privada o pública
* Configuración de la velocidad de puerto
* Creación una plantilla de imagen
* Apagado de una instancia de servidor virtual
* Encendido de una instancia de servidor virtual
* Rearranque de una instancia de servidor virtual
* Redenominación de una instancia de servidor virtual
* Cómo entrar en modalidad de rescate para una instancia de servidor virtual
* Adición de un grupo de seguridad a una interfaz pública o privada de una instancia de servidor virtual
* Eliminación de un grupo de seguridad de una interfaz pública o privada de una instancia de servidor virtual
* Carga de una instancia de servidor virtual a partir de una imagen
* Arranque de una instancia de servidor virtual a partir de una imagen
* Cancelación de un dispositivo de instancia de servidor virtual

Para los sucesos asociados a una interfaz de red, el registro de auditoría no distingue si el suceso se ha producido en una interfaz pública o en una interfaz privada.
{: tip}
