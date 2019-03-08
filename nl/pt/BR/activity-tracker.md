---

copyright:
  years: 2018
lastupdated: "2018-08-09"

subcollection: image-templates

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:table: .aria-labeledby="caption"}

# Auditando eventos do servidor virtual com o Activity Tracker

É possível auditar uma instância de servidor virtual ao longo de seu ciclo de vida usando o [Activity Tracker](/docs/services/cloud-activity-tracker?topic=cloud-activity-tracker-activity_tracker_ov). Deve-se ter uma instância do Activity Tracker com o plano de serviço premium provisionado no Sul dos EUA. O plano premium fornece a você o acesso ao painel do Kibana que tem mais opções para filtragem e procura de logs de auditoria.

Os logs são visíveis na seção de logs da conta para o proprietário da conta do {{site.data.keyword.cloud}}. O proprietário da conta pode visualizar os logs a seguir:
* Eventos acionados pelo proprietário da conta.
* Eventos acionados por usuários que estão vinculados à conta.

É possível auditar os eventos de instância de servidor virtual a seguir por meio do Activity Tracker:
* Provisionando uma instância de servidor virtual
* Desativando uma porta para uma interface privada ou pública
* Ativando uma porta para uma interface privada ou pública
* Configurando a velocidade da porta
* Criando um modelo de imagem
* Desligando uma instância de servidor virtual
* Ligando uma instância de servidor virtual
* Reinicializando uma instância de servidor virtual
* Renomeando uma instância de servidor virtual
* Inserindo o modo de resgate para uma instância de servidor virtual
* Incluindo um grupo de segurança em uma interface pública ou privada de uma instância de servidor virtual
* Removendo um grupo de segurança de uma interface pública ou privada de uma instância de servidor virtual
* Carregando uma instância de servidor virtual por meio de uma imagem
* Inicializando uma instância de servidor virtual por meio de uma imagem
* Cancelando um dispositivo de instância de servidor virtual

Para eventos associados a uma interface de rede, o log de auditoria não distingue se o evento ocorreu em uma interface pública ou em uma interface privada.
{: tip}
