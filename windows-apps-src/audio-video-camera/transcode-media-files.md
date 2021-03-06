---
ms.assetid: A1A0D99A-DCBF-4A14-80B9-7106BEF045EC
description: Mit den Windows.Media.Transcoding-APIs können Sie Videodateien von einem Format in ein anderes transcodieren.
title: Transkodieren von Mediendateien
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 58cc932ee8801835c44282de159900e3bb167e01
ms.sourcegitcommit: c9bab19599c0eb2906725fd86d0696468bb919fa
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/03/2020
ms.locfileid: "78256163"
---
# <a name="transcode-media-files"></a>Transkodieren von Mediendateien



Mit den [**Windows.Media.Transcoding**](https://docs.microsoft.com/uwp/api/Windows.Media.Transcoding)-APIs können Sie Videodateien in ein anderes Format transcodieren.

Beim *Transcodieren* wird eine digitale Mediendatei (etwa eine Video- oder Audiodatei) in ein anderes Format konvertiert. Dies geschieht in der Regel durch Decodieren und erneutes Codieren der Datei. Beispiel: Sie konvertieren eine Windows Media-Datei in MP4, um sie auf einem Gerät wiederzugeben, das das MP4-Format unterstützt. Oder Sie können eine Videodatei mit hoher Auflösung in eine niedrigere Auflösung konvertieren. In diesem Fall kann die neu codierte Datei den gleichen Codec wie die Originaldatei verwenden, sie besitzt jedoch ein anderes Codierungsprofil.

## <a name="set-up-your-project-for-transcoding"></a>Einrichten des Projekts für die Transcodierung

Zusätzlich zu den Namespaces, auf die von der Standardprojektvorlage verwiesen wird, müssen Sie auf die folgenden Namespaces verweisen, um Mediendateien mit dem Code in diesem Artikel transcodieren zu können:

[!code-cs[Using](./code/TranscodeWin10/cs/MainPage.xaml.cs#SnippetUsing)]

## <a name="select-source-and-destination-files"></a>Auswählen der Quell- und Zieldateien

Die Art, wie Ihre App die Quell- und Zieldateien für die Transcodierung ermittelt, hängt von der Implementierung ab. In diesem Beispiel werden eine [**FileOpenPicker**](https://docs.microsoft.com/uwp/api/Windows.Storage.Pickers.FileOpenPicker)-Klasse und eine [**FileSavePicker**](https://docs.microsoft.com/uwp/api/Windows.Storage.Pickers.FileSavePicker)-Klasse verwendet, um Benutzern die Auswahl einer Quell- und Zieldatei zu ermöglichen.

[!code-cs[TranscodeGetFile](./code/TranscodeWin10/cs/MainPage.xaml.cs#SnippetTranscodeGetFile)]

## <a name="create-a-media-encoding-profile"></a>Erstellen eines Mediencodierungsprofils

Das Codierungsprofil enthält die Einstellungen, die die Codierungsart der Zieldatei festlegen. Hier stehen die meisten Optionen zum Transcodieren einer Datei zur Verfügung.

Die [**MediaEncodingProfile**](https://docs.microsoft.com/uwp/api/Windows.Media.MediaProperties.MediaEncodingProfile)-Klasse stellt statische Methoden zum Erstellen vordefinierter Codierungsprofile bereit:

### <a name="methods-for-creating-audio-only-encoding-profiles"></a>Methoden zum Erstellen von Codierungsprofilen, die nur Audio enthalten

Methode  |Profil  |
---------|---------|
[**In der Systemsteuerung**](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingprofile.createalac)     |Apple Lossless Audio Codec (ALAC)-Audio         |
[ **"Kreateflac"** ](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingprofile.createflac)     |Free Lossless Audio Codec (FLAC)-Audio.         |
[**CreateM4a**](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingprofile.createm4a)     |AAC-Audio (M4A)         |
[**CreateMp3**](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingprofile.createmp3)     |MP3-Audio         |
[ **"Kreatewav"** ](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingprofile.createwav)     |WAV-Audio         |
[ **"Kreatewmv"** ](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingprofile.createwmv)     |Windows Media Audio (WMA)         |

### <a name="methods-for-creating-audio--video-encoding-profiles"></a>Methoden zum Erstellen von Audio-/Videocodierungsprofilen

Methode  |Profil  |
---------|---------|
[ **"Kreateavi"** ](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingprofile.createavi) |AVI |
[ **"Erker"** ](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingprofile.createhevc) |High Efficiency Video Coding (HEVC)-Video, auch als H.265-Video bezeichnet |
[**CreateMp4**](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingprofile.createmp4) |MP4-Video (H.264-Video und AAC-Audio) |
[ **"Kreatewmv"** ](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingprofile.createwmv) |Windows Media Video (WMV) |


Der folgende Code erstellt ein Profil für MP4-Video:

[!code-cs[TranscodeMediaProfile](./code/TranscodeWin10/cs/MainPage.xaml.cs#SnippetTranscodeMediaProfile)]

Die statische [**CreateMp4**](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingprofile.createmp4)-Methode erstellt ein MP4-Codierungsprofil. Der Parameter für diese Methode gibt die Zielauflösung für das Video an. In diesem Fall bedeutet [**VideoEncodingQuality.hd720p**](https://docs.microsoft.com/uwp/api/Windows.Media.MediaProperties.VideoEncodingQuality): 1280 x 720 Pixel mit 30 Bildern pro Sekunde. („720p“ steht für 720 progressive Scanlinien pro Frame.) Dieses Muster gilt auch für die anderen Methoden zum Erstellen vordefinierter Profile.

Alternativ können Sie mit der [**MediaEncodingProfile.CreateFromFileAsync**](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingprofile.createfromfileasync)-Methode ein Profil erstellen, das einer vorhandenen Mediendatei entspricht. Wenn Sie die genauen gewünschten Codierungseinstellungen kennen, können Sie auch ein neues [**MediaEncodingProfile**](https://docs.microsoft.com/uwp/api/Windows.Media.MediaProperties.MediaEncodingProfile)-Objekt erstellen und die Profildetails ausfüllen.

## <a name="transcode-the-file"></a>Transcodieren der Datei

Erstellen Sie zum Transcodieren der Datei ein neues [**MediaTranscoder**](https://docs.microsoft.com/uwp/api/Windows.Media.Transcoding.MediaTranscoder)-Objekt, und rufen Sie die [**MediaTranscoder.PrepareFileTranscodeAsync**](https://docs.microsoft.com/uwp/api/windows.media.transcoding.mediatranscoder.preparefiletranscodeasync)-Methode auf. Übergeben Sie die Quelldatei, die Zieldatei und das Codierungsprofil. Rufen Sie dann die [**TranscodeAsync**](https://docs.microsoft.com/uwp/api/windows.media.transcoding.preparetranscoderesult.transcodeasync)-Methode für das [**PrepareTranscodeResult**](https://docs.microsoft.com/uwp/api/Windows.Media.Transcoding.PrepareTranscodeResult)-Objekt auf, das mit dem asynchronen Transcodierungsvorgang zurückgegeben wurde.

[!code-cs[TranscodeTranscodeFile](./code/TranscodeWin10/cs/MainPage.xaml.cs#SnippetTranscodeTranscodeFile)]

## <a name="respond-to-transcoding-progress"></a>Reaktion auf Transcodierungsfortschritt

Sie können Reaktionsereignisse registrieren, wenn sich der Fortschritt der asynchronen [**TranscodeAsync**](https://docs.microsoft.com/uwp/api/windows.media.transcoding.preparetranscoderesult.transcodeasync)-Klasse ändert. Diese Ereignisse sind Teil des asynchronen Programmierungsframeworks für Apps für die universelle Windows-Plattform (UWP) und nicht für die Transcodierungs-API spezifisch.

[!code-cs[TranscodeCallbacks](./code/TranscodeWin10/cs/MainPage.xaml.cs#SnippetTranscodeCallbacks)]


