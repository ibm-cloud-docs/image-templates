---

copyright:
  years: 2014, 2018
lastupdated: "2018-11-15"

subcollection: image-templates

---

{:shortdesc: .shortdesc}
{:tip: .tip}
{:new_window: target="_blank"}


# Criando um modelo de imagem

Com modelos de imagem, é possível replicar várias opções de configuração para o {{site.data.keyword.virtualmachinesshort}}.
{:shortdesc}

Em qualquer ponto durante a vida de um servidor virtual, é possível criar um modelo de imagem. Em seguida, é possível usá-lo para replicar rapidamente partes de sua configuração em outro servidor virtual. É possível criar modelos de imagem por meio de qualquer servidor virtual, independentemente de seu sistema operacional. Quando o seu modelo de imagem estiver concluído, será possível usá-lo para criar outro servidor virtual.

Complete as etapas a seguir para criar um modelo de imagem de um servidor virtual.

1. Acesse o [{{site.data.keyword.slportal_full}} ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://control.softlayer.com/){: new_window} usando suas credenciais exclusivas.
2. No menu **Dispositivos**, selecione **Lista de dispositivos**.
3. Clique no servidor virtual que você deseja usar para criar um modelo de imagem.

  Verifique a guia **Senhas** da página **Detalhes do dispositivo**. Assegure-se de que quaisquer senhas listadas na página **Detalhes do dispositivo** correspondam às senhas reais do sistema operacional e a quaisquer outras senhas de complemento de software. Se as senhas não corresponderem, os servidores virtuais que forem criados por meio desse modelo de imagem falharão.
  {:tip}

4. No menu **Ações**, selecione **Criar modelo de imagem**.
5. Insira o novo nome para a imagem no campo **Nome da imagem**.
6. Insira quaisquer notas necessárias para a imagem no campo **Nota**.
7. Selecione a caixa de seleção **Concordo** quando todas as informações forem inseridas.
8. Clique em **Criar modelo** para criar o modelo de imagem.

## Próximas etapas

Após o modelo de imagem ser criado, mais servidores virtuais poderão ser criados usando o modelo. Os novos
servidores virtuais têm as mesmas configurações que estão incluídas no modelo de imagem.
