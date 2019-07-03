---

copyright:
  years: 2014, 2018
lastupdated: "2019-06-11"

keywords:

subcollection: image-templates

---

{:shortdesc: .shortdesc}
{:tip: .tip}
{:new_window: target="_blank"}


# Criando um modelo de imagem
{: #creating-an-image-template}

Com modelos de imagem, é possível replicar várias opções de configuração para o {{site.data.keyword.virtualmachinesshort}}.
{:shortdesc}

Em qualquer ponto durante a vida de um servidor virtual, é possível criar um modelo de imagem. Em seguida, é possível usá-lo para replicar rapidamente partes de sua configuração em outro servidor virtual. É possível criar modelos de imagem por meio de qualquer servidor virtual, independentemente de seu sistema operacional. Quando o seu modelo de imagem estiver concluído, será possível usá-lo para criar outro servidor virtual.

## Antes
de Começar
Primeiro, navegue até o menu do dispositivo e assegure-se de ter as permissões de conta corretas para concluir as tarefas.

* Navegue até o menu do dispositivo do console. Para obter mais informações, veja [Navegando até dispositivos](/docs/infrastructure/image-templates?topic=virtual-servers-navigating-devices).
* Assegure-se de ter quaisquer permissões de conta necessárias e de ter acesso ao dispositivo. Somente o proprietário da conta ou um usuário com a permissão de infraestrutura clássica **Gerenciar usuários** poderá ajustar as permissões.

Para obter mais informações sobre permissões, veja [Permissões de infraestrutura clássica](/docs/iam?topic=iam-infrapermission#infrapermission) e [Gerenciando o acesso ao dispositivo](/docs/vsi?topic=virtual-servers-managing-device-access).

## Criando um modelo de imagem

Complete as etapas a seguir para criar um modelo de imagem de um servidor virtual.

1. No menu **Dispositivos**, selecione **Lista de dispositivos**.
2. Clique no servidor virtual que você deseja usar para criar um modelo de imagem.

  Verifique a guia **Senhas** da página **Detalhes do dispositivo**. Assegure-se de que quaisquer senhas listadas na página **Detalhes do dispositivo** correspondam às senhas reais do sistema operacional e a quaisquer outras senhas de complemento de software. Se as senhas não corresponderem, os servidores virtuais que forem criados por meio desse modelo de imagem falharão.
  {:tip}

3. No menu **Ações**, selecione **Criar modelo de imagem**.
4. Insira o novo nome para a imagem no campo **Nome da imagem**.
5. Insira quaisquer notas necessárias para a imagem no campo **Nota**.
6. Selecione a caixa de seleção **Concordo** quando todas as informações forem inseridas.
7. Clique em **Criar modelo** para criar o modelo de imagem.

## Próximas etapas

Após o modelo de imagem ser criado, mais servidores virtuais poderão ser criados usando o modelo. Os novos
servidores virtuais têm as mesmas configurações que estão incluídas no modelo de imagem.
