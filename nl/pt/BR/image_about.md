---

copyright:
  years: 2014, 2017
lastupdated: "2017-10-03"

keywords:

subcollection: image-templates

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}

# Sobre modelos de imagem
{: #about-image-templates}

Modelos de imagem padrão fornecem uma opção de criação de imagens para todos os {{site.data.keyword.BluVirtServers_short}}, independentemente do sistema operacional.
{:shortdesc}

Com modelos de imagem padrão, é possível capturar uma imagem de um servidor virtual existente e criar uma nova baseada na imagem capturada. Modelos de imagem padrão não são compatíveis com servidores bare metal.

## Como funcionam os modelos de imagem
As duas ações principais para qualquer modelo de imagem são criar e implementar. Quando você solicita a criação de uma imagem, o sistema automatizado do {{site.data.keyword.BluSoftlayer_full}} coloca seu servidor off-line. Enquanto o servidor está off-line, um backup compactado de seus dados é criado, as informações de configuração são registradas e o modelo de imagem é armazenado na SAN do {{site.data.keyword.BluSoftlayer_notm}}. Durante o estágio de implementação do modelo de imagem, o sistema automatizado constrói um novo servidor que é baseado nos dados que são reunidos da imagem selecionada. O processo de implementação faz ajustes para o volume, restaura os dados copiados e, em seguida, faz mudanças de configuração final (por exemplo, configurações de rede) para o novo host.

## Imagens Privadas

Imagens privadas são imagens que são criadas por um usuário na conta ou imagens que são criadas em outra conta e compartilhadas. Por padrão, todas as imagens que são criadas são privadas.

## Imagens Públicas

Imagens públicas são servidores virtuais pré-configurados que são postados pelo {{site.data.keyword.BluSoftlayer_notm}} ou divulgados por clientes e estão disponíveis para uso por todos os clientes do {{site.data.keyword.BluSoftlayer_notm}}. Os servidores virtuais que são fornecidos por meio de modelos de imagem pública geralmente são configurados para um desempenho otimizado, com combinações específicas de especificações de configuração diferentes.
