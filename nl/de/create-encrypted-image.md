---

copyright:
  years: 2019
lastupdated: "2019-03-27"

keywords: VHD image file, encryption, encrypted image, image

subcollection: image-templates

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:tip: .tip}
{:important: .important}


# VHD-Images verschlüsseln 
{: #create-encrypted-image}

Um das Feature zur E2E-Verschlüsselung zu nutzen, müssen Sie Ihr VHD-Image vor dem Import in die Imagevorlagen zur Bereitstellung verschlüsselter Instanzen mit dem Tool 'vhd-util' verschlüsseln. Es werden zwei AES-Verschlüsselungsstufen unterstützt: AES 256-Bit und AES 512-Bit.
{:shortdesc}

## Voraussetzungen für verschlüsselte VHD-Images
{: #encrypted-image-reqs}

Verschlüsselte VHD-Images müssen die folgenden Voraussetzungen erfüllen:

* Sie müssen im VHD-Format vorliegen.
* Sie müssen mit der Infrastrukturumgebung der {{site.data.keyword.cloud}}-Konsole kompatibel sein.
* Sie müssen mit einem [unterstützten Betriebssystem](/docs/infrastructure/image-templates/?topic=image-templates-preparing-and-importing-images#preparing-and-importing-images) bereitgestellt werden.
* Sie müssen cloud-init-fähig sein.
* Sie müssen mit dem [Tool 'vhd-util'](/docs/infrastructure/image-templates?topic=image-templates-create-encrypted-image#vhd-util-tool) verschlüsselt worden sein.

## Eigenes VHD-Image verschlüsseln
{: #vhd-util-tool}

Führen Sie die folgenden Schritte aus, um Ihr verschlüsseltes VHD-Image zu erstellen:

1. Wählen Sie ein CentOS-System mit Version 7 oder höher aus, um Ihre VHD-Datei für {{site.data.keyword.cloud_notm}} zu verschlüsseln. Falls Sie keinen Zugriff auf die physische Hardware mit installiertem CentOS haben, können Sie eine virtuelle Serverinstanz mit CentOS 7 innerhalb von {{site.data.keyword.cloud_notm}} bereitstellen; verwenden Sie dabei entweder einen öffentlichen oder einen dedizierten Host. Das für die Verschlüsselung der VHD-Dateien verwendete CentOS-System muss selbst verschlüsselt werden.

2. Melden Sie sich bei Ihrem CentOS-System an und stellen Sie eine Verbindung zu Ihrem Kunden-VPN her; [rufen Sie anschließend die SoftLayer-Download-Site auf ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](http://downloads.service.softlayer.com/citrix/xen/){: new_window} und wählen Sie die RPM-Paketdatei mit dem Tool 'vhd-util' aus: vhd-util-standalone-3.5.0-xs.2+1.0_71.2.2.x86_64.rpm   

   Wenn Sie die RPM-Paketdatei nicht direkt in Ihr CentOS-System herunterladen können, laden Sie sie auf die Workstation herunter, mit der Sie derzeit arbeiten. Anschließend können Sie sie mithilfe des Befehls zum sicheren Kopieren (scp) in Ihr CentOS-System hochladen. Bei der Verwendung einer virtuellen Serverinstanz in {{site.data.keyword.cloud_notm}} nutzen Sie für diesen Upload die öffentliche IP-Adresse des Systems in Verbindung mit dem folgenden Befehl.

   ```
   scp vhd-util-standalone-3.5.0-xs.2+1.0_71.2.2.x86_64.rpm root@<vsi_public_ip>:
   ```
   {: pre}

3. Installieren Sie den RPM mithilfe des folgenden Befehls:

   ```
   rpm -iv vhd-util-standalone-3.5.0-xs.2+1.0_71.2.2.x86_64.rpm
   ```
   {: pre}

4. Geben Sie den AES-Verschlüsselungsschlüssel an, den Sie für die Ver- und Entschlüsselung Ihres Plattenimage benötigen, und schreiben Sie ihn in eine Schlüsseldatei. Dieser AES-Verschlüsselungsschlüssel ist mit dem Schlüssel zur Datenverschlüsselung identisch, für den Sie mit dem Key Protect-Kundenrootschlüssel im Schritt zur [Vorbereitung Ihrer Umgebung](/docs/infrastructure/image-templates?topic=image-templates-using-end-to-end-e2e-encryption-to-provision-an-encrypted-instance#preparing-your-environment) ein Wrapping durchgeführt haben. Für die Schlüsselinformationen, die in Schlüsseldateien geschrieben werden, darf kein Wrapping durchgeführt worden sein und sie dürfen nicht verschlüsselt sein. 

   Da der Datenschlüssel innerhalb der Schlüsseldateien nicht mit Base64 verschlüsselt ist, ist es nicht möglich, mithilfe der standardmäßigen ASCII-Zeichen den Inhalt der Schlüsseldatei über die Befehlszeile zu drucken oder anzuzeigen.
   {: tip}

   Verwenden Sie für die Erstellung von Schlüsseldateien unter Anwendung des Verschlüsselungsschlüssels **AES 256-Bit** oder **AES 512-Bit** den folgenden Befehl: 
   
   ```
   echo <data_key> | base64 -d - > <keyfile_name>
   ```
   {: pre} 

   Beispielbefehl:

   ```
   echo Nrfen98EpMxF2B+wdgLfagzrqvgUZfMK4vL2T0NsT20ihrsNC9pUUHtizF6218pze8RLCgQ6kwxuE58IWLzgDA== | base64 -d - > aes512.dek
   ```
   {: screen}

5. Verwenden Sie den folgenden Befehl, um die Schlüsseldateien zu prüfen, die Sie im vorherigen Schritt erstellt haben:

   ```
   vhd-util key -C -k <keyfile_name>
   ```
   {: pre}

   Beispielbefehl mit Ausgabe:

   ```
   vhd-util key -C -k aes512.dek
   vhd_util_read_key: using keyfile aes512.dek, Size (bytes) 64
   21681bba94f04b33b112f5f90a0faa885a6d1dbf1bd68ed16c5b995143088eda
   ```
   {: screen}

   Die erste Zeile der Ausgabe des vorherigen Beispielbefehls gibt an, dass die Schlüsseldatei `aes512.dek` einen 64-Byte-Schlüssel enthält; die Zahlen in der zweiten Zeile sind die SHA256-Hashwerte bzw. die Sicherheitshashwerte für die entsprechenden Verschlüsselungsschlüssel. Die Ausgabe für Dateien mit einem AES 256-Bit-Verschlüsselungsschlüssel weisen auf einen 32-Byte-Schlüssel hin.
   {: tip} 

6. Verwenden Sie den folgenden Befehl, um verschlüsselte Kopien Ihrer VHD-Dateien zu erstellen. `target_vhd` steht für den Namen der Datei mit der verschlüsselten Version von `source_vhd`.

   ```
   vhd-util copy -n <source_vhd> -N <target_vhd> -k <keyfile_name>
   ```
   {: pre}    

   Beispielbefehl:

   ```
   vhd-util copy -n debian8-ne.vhd -N debian8-aes512.vhd -k aes512.dek
   ```
   {: screen}

7. Überprüfen Sie mithilfe des folgenden Befehls, ob die VHD-Dateien verschlüsselt wurden.

   ```
   vhd-util key -p -n <vhd_filename>
   ```
   {: pre}

   Beispielbefehl mit Ausgabe:

   ```
   vhd-util key -p -n debian8-aes512.vhd
   0000000000000000000000000000000000000000000000000000000000000000
   21681bba94f04b33b112f5f90a0faa885a6d1dbf1bd68ed16c5b995143088eda
   ```
   {: screen}

Ist die VHD-Datei verschlüsselt, sehen Sie ähnlich wie im vorherigen Beispiel in der Ausgabe zwei Hashwerte. Der erste Hashwert besteht nur aus Nullen. Der zweite Hashwert ist der SHA256-Hashwert, den der AES-Verschlüsselungsschlüssel für die Ver- und Entschlüsselung der VHD nutzt. Stellen Sie sicher, dass die SHA256-Hashwerte für die VHD-Dateien mit den in **Schritt 5** gezeigten Hashwerten identisch sind.

Durch den Beispielbefehl in **Schritt 7** wird eine neue verschlüsselte VHD-Datei mit dem Namen “debian8-aes512.vhd” erstellt. Sie wird mit dem AES 512-Bit-Verschlüsselungsschlüssel aus der Schlüsseldatei “aes512.dek” verschlüsselt. Der SHA256-Hashwert für die Verschlüsselung lautet `21681bba94f04b33b112f5f90a0faa885a6d1dbf1bd68ed16c5b995143088eda`.
