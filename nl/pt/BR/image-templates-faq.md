---

copyright:
  years: 2014, 2018
lastupdated: "2017-10-04"

---


{:new_window: target="_blank"}
{:tip: .tip}

# FAQs: modelos de imagem

## O que é um modelo de imagem padrão?

Um modelo de imagem padrão é a opção de criação de imagem do {{site.data.keyword.virtualmachineslong}} para o {{site.data.keyword.BluSoftlayer_full}}.
Modelos de imagem padrão permitem que os usuários capturem uma imagem de um servidor virtual existente, independentemente de seu sistema operacional e criem
um novo servidor virtual baseado na imagem.

## O que é um modelo ISO?

O Modelo ISO é um tipo de modelo que é especificamente reservado para ISOs que podem ser usados para inicializar uma VSI. Os modelos ISO estão disponíveis em duas versões: público e privado. Os modelos ISO públicos são modelos pré-configurados fornecidos pelo {{site.data.keyword.BluSoftlayer_notm}} e podem ser usados por qualquer cliente. Os modelos ISO privados são criados importando uma imagem de um ISO armazenado em uma Conta do {{site.data.keyword.objectstorageshort}}. Para que um ISO seja importado para a tela Modelos de imagem, o ISO deve ser inicializável.

Apenas Sistemas Operacionais Suportados pelo {{site.data.keyword.BluSoftlayer_notm}} podem ser usados para carregar um modelo ISO em uma VSI. Para obter mais informações, consulte [Sistemas operacionais suportados ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](http://www.softlayer.com/services/software/).
{:tip}

## Qual é a diferença entre uma imagem pública e uma imagem privada?

Uma imagem pública é uma imagem que pode ser visualizada e aplicada em um novo servidor virtual por qualquer usuário do {{site.data.keyword.BluSoftlayer_notm}}. O {{site.data.keyword.BluSoftlayer_notm}}
atualmente cria imagens públicas como uma solução para opções de configuração em diferentes dispositivos. Os clientes também podem tornar imagens públicas e disponíveis para qualquer usuário. Uma imagem privada é uma imagem que pode ser
visualizada apenas por usuários autorizados. Usuários autorizados são padronizados para qualquer usuário em sua conta; no entanto, as imagens também podem ser compartilhadas entre múltiplas
contas atualizando as opções de compartilhamento no {{site.data.keyword.slportal_full}}.

## Posso capturar e implementar servidores virtuais com o hypervisor autogerenciado?

Somente servidores que são provisionados por {{site.data.keyword.BluSoftlayer_notm}} podem ser capturados e implementados. Os servidores virtuais individuais que você criou manualmente em dispositivos pessoais não podem ser capturados, provisionados ou implementados.

## Posso compartilhar meu Modelo ISO privado com outros clientes?

Os únicos Modelos ISO que são disponibilizados para todos os clientes são modelos gerados pelo {{site.data.keyword.BluSoftlayer_notm}}. Os modelos ISO privados são específicos da conta e não podem ser compartilhados entre clientes por meio do {{site.data.keyword.slportal}}.

## Meu modelo ISO precisa estar no mesmo data center que meu servidor virtual para concluir uma inicialização de imagem?

Sim, os modelos ISO devem estar no mesmo data center que um servidor virtual para inicializar da imagem. Se ele não estiver no mesmo data center,
o modelo ISO deverá ser copiado no data center apropriado para concluir a ação. Se essa situação ocorrer, aparecerá
um aviso relacionado à transação. Quando um modelo ISO é copiado em um data center diferente, uma pequena taxa é cobrada para a conta da
mesma maneira que as taxas são aplicadas para copiar outros tipos de modelos de imagem.

## Qual é a diferença entre os dados físicos e o tamanho do volume?

O volume é o espaço em disco que está disponível para armazenar arquivos, enquanto os dados físicos consistem nos arquivos reais que são armazenados no disco.

## O que é o recurso de importação/exportação de imagem?

O recurso de importação/exportação de imagem que está localizado na página Modelos de imagem no {{site.data.keyword.slportal}} permite que VHDs e ISOs armazenados em uma conta do {{site.data.keyword.objectstorageshort}} sejam convertidos em modelos de imagem e vice-versa. Ao importar uma imagem, um arquivo específico (VHD ou ISO) é originado de um Contêiner da Conta especificado [{{site.data.keyword.objectstorageshort}}] e é convertido em um modelo de imagem. O modelo de imagem pode, então, ser usado para inicializar ou carregar um dispositivo. Quando você exportar uma imagem, o modelo de imagem será convertido em um arquivo (ou vários arquivos se o modelo tiver múltiplos discos) que será armazenado em um local especificado em um Contêiner da Conta do {{site.data.keyword.objectstorageshort}}. 


