---
author: laurenhughes
title: Erstellen eines Paketsignaturzertifikats
description: PowerShell-Tools helfen bei der Erstellung und beim Export eines App-Paketsignaturzertifikats.
ms.author: lahugh
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "Windows 10, UWP"
ms.assetid: 7bc2006f-fc5a-4ff6-b573-60933882caf8
translationtype: Human Translation
ms.sourcegitcommit: 5645eee3dc2ef67b5263b08800b0f96eb8a0a7da
ms.openlocfilehash: 2332abe43732299dfb0f4bc265bf1b12877a17aa
ms.lasthandoff: 02/08/2017

---

# <a name="create-a-certificate-for-package-signing"></a>Erstellen eines Paketsignaturzertifikats

\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

In diesem Artikel wird die Erstellung und der Export eines App-Paketsignaturzertifikats mithilfe von PowerShell-Tools erläutert. Es wird empfohlen, Visual Studio zum [Verpacken von UWP-Apps](https://msdn.microsoft.com/windows/uwp/packaging/packaging-uwp-apps) zu verwenden. Sie können allerdings auch weiterhin App-Pakete für den Store manuell verpacken, wenn Sie Ihre App nicht mit Visual Studio erstellt haben.

## <a name="prerequisites"></a>Voraussetzungen

- **Verpackte oder unverpackte Apps**  
Eine App mit einer AppxManifest.xml-Datei. Sie müssen während der Erstellung des Zertifikats auf die Manifestdatei verweisen, die zum Signieren der endgültigen Version des App-Pakets verwendet wird. Details zum manuellen Verpacken von Apps finden Sie unter [Erstellen eines App-Pakets mit dem Tool „MakeAppx.exe“](https://msdn.microsoft.com/windows/uwp/packaging/create-app-package-with-makeappx-tool).

- **PKI-Cmdlets (Public Key-Infrastruktur)**  
Sie benötigen die PKI-Cmdlets zum Erstellen und Exportieren Ihres Signaturzertifikats. For more information, see [Public Key-Infrastruktur-Cmdlets](https://technet.microsoft.com/library/hh848636.aspx).

## <a name="create-a-self-signed-certificate"></a>Erstellen eines selbstsignierten Zertifikats

Ein selbstsigniertes Zertifikat eignet sich für das Testen Ihrer App, bevor es im Store veröffentlicht wird. Folgen Sie den in diesem Abschnitt beschriebenen Anweisungen zum Erstellen eines selbstsignierten Zertifikats.

### <a name="determine-the-subject-of-your-packaged-app"></a>Festlegen des Betreffs Ihres App-Pakets  

Zur Verwendung eines Zertifikats für die Signatur des App-Pakets **muss** der Betreff im Zertifikat dem Abschnitt „Herausgeber” des App-Manifests entsprechen.

Beispielsweise sollte der Abschnitt „Identität” in der „AppxManifest.xml”-Datei Ihrer App etwa wie folgt aussehen:
```
  <Identity Name="Contoso.AssetTracker" 
    Version="1.0.0.0" 
    Publisher="CN=Contoso Software, O=Contoso Corporation, C=US"/>
```

Der „Herausgeber” ist in diesem Fall „CN=Contoso Software, O=Contoso Corporation, C=US", was zum Erstellen eines Zertifikats verwendet werden muss. 

### <a name="use-new-selfsignedcertificate-to-create-a-certificate"></a>Verwenden von **New-SelfSignedCertificate** zum Erstellen eines Zertifikats
Verwenden Sie das PowerShell-Cmdlet **New-SelfSignedCertificate** zum Erstellen eines selbstsignierten Zertifikats. **New-SelfSignedCertificate** verfügt über mehrere anpassbare Parameter. Bei diesem Artikel konzentrieren wir uns allerdings auf das Erstellen eines einfachen Zertifikats, das mit **SignTool** funktioniert. Weitere Beispiele und Verwendungen dieses Cmdlets finden Sie unter [New-SelfSignedCertificate](https://technet.microsoft.com/library/hh848633.aspx).

Basierend auf der „AppxManifest.xml”-Datei aus dem vorherigen Beispiel, sollten Sie folgende Syntax verwenden, um ein Zertifikat zu erstellen. In einer PowerShell-Eingabeaufforderung mit erhöhten Rechten:
```
New-SelfSignedCertificate -Type Custom -Subject "CN=Contoso Software, O=Contoso Corporation, C=US" -KeyUsage DigitalSignature -FriendlyName <Your Friendly Name> -CertStoreLocation "Cert:\LocalMachine\My"
```

Nach dem Ausführen dieses Befehls wird das Zertifikat dem lokalen Zertifikatspeicher hinzugefügt, wie im Parameter ‑CertStoreLocation angegeben. Das Ergebnis des Befehls erzeugt den Zertifikatfingerabdruck.  

**Hinweis:**  
Sie können das Zertifikat mithilfe der folgenden Befehle in einem PowerShell-Fenster anzeigen:
```
Set-Location Cert:\LocalMachine\My
Get-ChildItem | Format-Table Subject, FriendlyName, Thumbprint
```
Dies zeigt alle Zertifikate in Ihrem lokalen Speicher an.

## <a name="export-a-certificate"></a>Exportieren eines Zertifikats 

Verwenden Sie das **Export-PfxCertificate**-Cmdlet, um das Zertifikat im lokalen Speicher auf eine private Informationsaustausch-Datei (PFX) zu exportieren.

Sie müssen bei der Verwendung von **Export-PfxCertificate** entweder ein Kennwort erstellen und verwenden oder den Parameter „‑ProtectTo” verwenden, um anzugeben, welche Benutzer oder Gruppen ohne Kennwort auf die Datei zugreifen können. Es wird eine Fehlermeldung angezeigt, wenn Sie weder den „-Kennwort”-, noch den „-ProtectTo”-Parameter verwenden.

- **Verwendung von Kennwörtern**
```
$pwd = ConvertTo-SecureString -String <Your Password> -Force -AsPlainText 
Export-PfxCertificate -cert "Cert:\LocalMachine\My\<Certificate Thumbprint>" -FilePath <FilePath>.pfx -Password $pwd
```

- **Verwendung von „ProtectTo”**
```
Export-PfxCertificate -cert Cert:\LocalMachine\My\<Certificate Thumbprint> -FilePath <FilePath>.pfx -ProtectTo <Username or group name>
```

Nachdem Sie Ihr Zertifikat erstellt und exportiert haben, sind Sie zum Signieren des App-Pakets mit **SignTool** bereit. Informationen zum nächsten Schritt bei der manuellen Verpackung finden Sie unter [Signieren eines App-Pakets mithilfe von SignTool](https://msdn.microsoft.com/windows/uwp/packaging/sign-app-package-using-signtool).

## <a name="security-considerations"></a>Sicherheitsüberlegungen 
Das Hinzufügen eines Zertifikats zum [Zertifikatspeicher des lokalen Computers](https://msdn.microsoft.com/windows/hardware/drivers/install/local-machine-and-current-user-certificate-stores) wirkt sich auf die Zertifikatvertrauensstellung für alle Benutzer des Computers aus. Sie sollten die Zertifikate entfernen, wenn sie nicht mehr erforderlich sind, um zu verhindern, dass diese die Systemvertrauensstellung gefährden.