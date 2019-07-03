---

copyright:
  years: 2014, 2018
lastupdated: "2019-06-11"

keywords:

subcollection: image-templates

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:tip: .tip}

# Inicializando uma VSI de uma imagem
{: #booting-a-vsi-from-an-image}

O recurso Inicialização da imagem inicia uma instância do servidor virtual (VSI) usando um modelo ISO que é
importado de uma conta do Object Storage.
{:shortdesc}

A inicialização de um servidor virtual de uma imagem coloca o dispositivo on-line com segurança para que os problemas possam ser resolvidos. Na maioria dos casos, a inicialização do recurso de imagem permite que a resolução de problemas ocorra em um ambiente sem o risco de perda de dados significativos que pode ser experimentado em um recarregamento de S.O. Embora a perda significativa de dados seja menos provável do que com um recarregamento do S.O., recomenda-se que você faça um backup do dispositivo antes de iniciar a inicialização.

## Antes
de Começar
Primeiro, navegue até o menu do dispositivo e assegure-se de ter as permissões de conta corretas para concluir as tarefas.

* Navegue até o menu do dispositivo do console. Para obter mais informações, veja [Navegando até dispositivos](/docs/infrastructure/image-templates?topic=virtual-servers-navigating-devices).
* Assegure-se de ter quaisquer permissões de conta necessárias e de ter acesso ao dispositivo. Somente o proprietário da conta ou um usuário com a permissão de infraestrutura clássica **Gerenciar usuários** poderá ajustar as permissões.

Para obter mais informações sobre permissões, veja [Permissões de infraestrutura clássica](/docs/iam?topic=iam-infrapermission#infrapermission) e [Gerenciando o acesso ao dispositivo](/docs/vsi?topic=virtual-servers-managing-device-access).

## Inicializando uma VSI de uma imagem

Use as etapas a seguir para iniciar um servidor virtual de uma imagem.

1. Faça backup de todos os dados no dispositivo.
2. No menu **Dispositivos**, selecione **Lista de dispositivos**.
3. Na Lista de dispositivos, clique no servidor virtual que você deseja iniciar de um modelo ISO.
4. Na página Detalhes do dispositivo para o servidor virtual, selecione **Ações > Inicialização da imagem**.
5. Clique em **Inicializar por meio dessa imagem** para a imagem desejada

  Alterne entre imagens públicas e privadas usando o recurso drop-down acima da lista de imagens.
  {:tip}

6. Clique em **Inicialização da imagem** para inicializar o dispositivo usando a imagem selecionada. Clique em **Fechar** para cancelar a ação.

## Próximas etapas

Depois de iniciar o processo de inicialização, a imagem é desligada e, em seguida, iniciada usando a imagem selecionada. O tempo de inicialização varia, pois o tamanho e o tipo de
cada imagem variam. Após o servidor virtual ser iniciado usando a imagem selecionada, ele poderá ser acessado como um dispositivo inicializado regularmente, mas será configurado de acordo com a imagem. Reinicie o servidor virtual para desligar e retornar o dispositivo para seu estado original.
