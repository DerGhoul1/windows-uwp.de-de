---
ms.assetid: ''
description: In diesem Artikel wird erläutert, wie Sie die Open Source Computer Vision-Bibliothek (OpenCV) mit der MediaFrameReader-Klasse verwenden.
title: Verwenden von OpenCV mit „MediaFrameReader”
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, openCV
ms.localizationpriority: medium
ms.openlocfilehash: a6594898dff1bf5f2262034b10e262082335f1b2
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/20/2019
ms.locfileid: "74255955"
---
# <a name="use-the-open-source-computer-vision-library-opencv-with-mediaframereader"></a>Verwenden Sie die Open Source Computer Vision-Bibliothek (OpenCV) mit MediaFrameReader

In diesem Artikel wird erläutert, wie Sie die Open Source Computer Vision-Bibliothek (OpenCV), eine systemeigene Code-Bibliothek, die eine Vielzahl von Algorithmen für die Bildverarbeitung bietet, mit der [**MediaFrameReader**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.Frames.MediaFrameReader)-Klasse verwenden, die Medien aus mehreren Quellen gleichzeitig lesen kann. Der Beispielcode in diesem Artikel führt Sie durch die Erstellung einer einfachen App, die Frames von einem Farbsensor erhält, jede Frame mithilfe der OpenCV-Bibliothek verwischt und dann das verarbeitete Bild in einem XAML-**Image** Steuerelement anzeigt. 

>[!NOTE]
>Opencv. Win. Core und opencv. Win. imgproc werden nicht regelmäßig aktualisiert und bestehen nicht in den Geschäfts Kompatibilitäts Prüfungen. diese Pakete sind daher nur für Experimente gedacht.

Dieser Artikel baut auf dem Inhalt von zwei anderen Artikeln auf:

* [Verarbeiten von Medienframes mit "MediaFrameReader"](process-media-frames-with-mediaframereader.md) – dieser Artikel enthält ausführliche Informationen zur Verwendung von **MediaFrameReader** zum Abrufen von Frames aus einer oder mehreren Medienframequellen und beschreibt ausführlich die meisten Beispielcodes in diesem Artikel. Vor allem der Artikel **Verarbeiten von Medienframes mit „MediaFrameReader“** enthält den Code für eine Hilfsklasse **FrameRenderer**, die die Darstellung von Medienframes in einem XAML-**Bild**element behandelt. Der Beispielcode in diesem Artikel verwendet ebenfalls diese Hilfsklasse.

* [Verarbeiten von Software Bitmaps mit opencv](process-software-bitmaps-with-opencv.md) : in diesem Artikel werden die Schritte zum Erstellen eines systemeigenen Codes Windows-Runtime-Komponente ( **opencvbridge**) erläutert, die die Konvertierung zwischen dem vom **mediaframereader**-Objekt verwendeten **Software Update** und dem von der OpenCV-Bibliothek verwendeten **matentyp** unterstützt. Im Beispielcode in diesem Artikel wird vorausgesetzt, dass Sie die Schritte zum Hinzufügen der **OpenCVBridge**-Komponente zu Ihrer UWP-App-Lösung befolgt haben.

Wenn Sie zusätzlich zu diesem Artikel ein vollständiges, funtionsfähiges End-to-End-Beispiel des in diesem Artikel beschriebenen Szenarios anzeigen und herunterladen möchten, finden Sie dies unter [Kamera-Frames + OpenCV – Beispiel](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CameraOpenCV) im GitHub-Repository für Beispiele für die Universelle Windows-Plattform.

Um mit der Entwicklung zu beginnen, können Sie die OpenCV-Bibliothek in ein UWP-App-Projekt einfügen, indem Sie nuget-Pakete verwenden. diese Pakete übergeben jedoch möglicherweise nicht den Zertifizierungsprozess der APP, wenn Sie Ihre APP an den Store übermitteln. es wird daher empfohlen, opencv herunterzuladen. der Quellcode der Bibliothek, und erstellen Sie die Binärdateien selbst, bevor Sie Ihre APP übermitteln. Informationen zum Entwickeln mit OpenCV finden Sie unter [https://opencv.org](https://opencv.org)


## <a name="implement-the-opencvhelper-native-windows-runtime-component"></a>Implementieren der systemeigenen Windows-Runtime-Komponente von opencvhelper
Führen Sie die Schritte in [Verarbeiten von Software Bitmaps mit opencv](process-software-bitmaps-with-opencv.md) aus, um die opencv Helper Windows-Runtime-Komponente zu erstellen und der UWP-App-Lösung einen Verweis auf das Komponenten Projekt hinzuzufügen.

## <a name="find-available-frame-source-groups"></a>Suchen nach verfügbaren Framequellgruppen
Zunächst müssen Sie eine Framequellgruppe von Medien finden, aus der die Medienframes abgerufen werden. Erhalten Sie die Liste der verfügbaren Gruppen auf dem aktuellen Gerät durch Aufrufen von **[MediaFrameSourceGroup.FindAllAsync](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframesourcegroup.FindAllAsync)** . Wählen Sie anschließend die Quellgruppen aus, die die für Ihr App-Szenario erforderlichen Sensortypen enthalten. In diesem Beispiel benötigen wir lediglich eine Quellgruppe, die Frames von einer RGB-Kamera bereitstellt.

[!code-cs[OpenCVFrameSourceGroups](./code/Frames_Win10/Frames_Win10/MainPage.OpenCV.xaml.cs#SnippetOpenCVFrameSourceGroups)]

## <a name="initialize-the-mediacapture-object"></a>Initialisieren des MediaCapture-Objekts
Initialisieren Sie anschließend das **MediaCapture**-Objekt durch Festlegen der Eigenschaften von **[SourceGroup](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacaptureinitializationsettings.SourceGroup)** der **MediaCaptureInitializationSettings**, um die im vorherigen Schritt ausgewählte Framequellgruppe zu verwenden.

> [!NOTE] 
> Die Technik, die von der OpenCV-Helferkomponente verwendet wird und die in [Verarbeiten von Softwarebitmaps mit OpenCV](process-software-bitmaps-with-opencv.md) ausführlich beschrieben ist, erfordert, dass sich die Bilddaten im CPU-Speicher und nicht im GPU-Speicher befinden. Sie sollten daher **MemoryPreference.CPU** für die **[MemoryPreference](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacaptureinitializationsettings.MemoryPreference)** im Feld der **MediaCaptureInitializationSettings** angeben.

Nachdem das **MediaCapture**-Objet initialisiert wurde, rufen Sie einen Verweis auf die RGB-Framequelle durch den Zugriff auf die **[MediaCapture.FrameSources](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture.FrameSources)** -Eigenschaft auf.

[!code-cs[OpenCVInitMediaCapture](./code/Frames_Win10/Frames_Win10/MainPage.OpenCV.xaml.cs#SnippetOpenCVInitMediaCapture)]

## <a name="initialize-the-mediaframereader"></a>Initialisieren des MediaFrameReader
Als Nächstes erstellen Sie einen [**MediaFrameReader**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.Frames.MediaFrameReader) für die RGB-Framequelle, die in den vorherigen Schritten abgerufen wurde. Um eine gute Framerate zu gewährleisten, sollten Sie Frames verarbeiten, die eine niedrigere Auflösung als die Auflösung des Sensors haben. Dieses Beispiel bietet das optionale **[BitmapSize](https://docs.microsoft.com/uwp/api/windows.graphics.imaging.bitmapsize)** -Argument für die **[MediaCapture.CreateFrameReaderAsync](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture.createframereaderasync)** -Methode, um anzufordern, dass die vom Frame-Reader bereitgestellten Frames auf 640 x 480 Pixel geändert werden.

Nach dem Erstellen des Frame-Readers, registrieren Sie einen Handler für das **[FrameArrived](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframereader.FrameArrived)** -Ereignis. Erstellen Sie anschließend ein neues **[SoftwareBitmapSource](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imaging.softwarebitmapsource)** -Objekt, das die **FrameRenderer**-Hilfsklasse verwendet, um das verarbeitete Bild darzustellen. Rufen Sie dann den Konstruktor für den **FrameRenderer** auf. Initialisieren Sie die Instanz der **opencvhelper** -Klasse, die in der Windows-Runtime Komponente von opencvbridge definiert ist. Diese Hilfsklasse wird im **FrameArrived**-Ereignishandler verwendet, um jeden Frame zu verarbeiten. Starten Sie anschließend die den Framereader durch Aufrufen von **[StartAsync](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframereader.StartAsync)** .

[!code-cs[OpenCVFrameReader](./code/Frames_Win10/Frames_Win10/MainPage.OpenCV.xaml.cs#SnippetOpenCVFrameReader)]


## <a name="handle-the-framearrived-event"></a>Behandeln des „FrameArrived“-Ereignisses
Das **FrameArrived**-Ereignis wird ausgelöst, wenn ein neuer Frame von FrameReader verfügbar ist. Rufen Sie **[TryAcquireLatestFrame](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframereader.TryAcquireLatestFrame)** auf, um das Frame zu erhalten, wenn es vorhanden ist. Rufen Sie **SoftwareBitmap** aus **[MediaFrameReference](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframereference)** auf. Beachten Sie, dass die **CVHelper**-Klasse, die in diesem Beispiel verwendet wird, Bilder im BRGA8-Format mit vormultipliziertem Alpha erfordert. Wenn das in das Ereignis übergebene Frame ein anderes Format hat, konvertieren Sie die **SoftwareBitmap** in das richtige Format. Erstellen Sie als Nächstes ein **SoftwareBitmap**, um als Ziel für den Weichzeichnervorgang zu verwenden. Die Quell-Bildeigenschaften werden als Argumente für den Konstruktor verwendet, um eine Bitmap mit übereinstimmendem Format erstellen. Rufen Sie die **Weichzeichner**-Methode der Hilfsklasse auf, um den Frame zu verarbeiten. Übergeben Sie abschließend das Bild des Weichzeichnervorgangs in **PresentSoftwareBitmap**, die Methode der **FrameRenderer**-Hilfsklasse, die das Bild im XAML-**Bild**-Steuerelement anzeigt, mit dem es initialisiert wurde.

[!code-cs[OpenCVFrameArrived](./code/Frames_Win10/Frames_Win10/MainPage.OpenCV.xaml.cs#SnippetOpenCVFrameArrived)]

## <a name="related-topics"></a>Verwandte Themen

* [Kamera](camera.md)
* [Einfaches Foto, Video und Audioerfassung mit mediacapture](basic-photo-video-and-audio-capture-with-MediaCapture.md)
* [Verarbeiten von Medien Frames mit mediaframereader](process-media-frames-with-mediaframereader.md)
* [Verarbeiten von Software Bitmaps mit opencv](process-software-bitmaps-with-opencv.md)
* [Beispiel für Kamera Frames](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CameraFrames)
* [Kamera Frames + opencv-Beispiel](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CameraOpenCV)
 

 




