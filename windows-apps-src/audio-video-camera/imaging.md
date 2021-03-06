---
ms.assetid: 3FD2AA71-EF67-47B2-9332-3FFA5D3703EA
description: In diesem Artikel wird das Laden und Speichern von Bilddateien mit BitmapDecoder und BitmapEncoder sowie das Verwenden des SoftwareBitmap-Objekts zum Darstellen von Bitmapbildern erläutert.
title: Erstellen, Bearbeiten und Speichern von Bitmapbildern
ms.date: 03/22/2018
ms.topic: article
keywords: windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: c25fc09d606c0f143f357dd7f89026fa94b80922
ms.sourcegitcommit: 6f32604876ed480e8238c86101366a8d106c7d4e
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/21/2019
ms.locfileid: "67318354"
---
# <a name="create-edit-and-save-bitmap-images"></a>Erstellen, Bearbeiten und Speichern von Bitmapbildern



In diesem Artikel wird das Laden und Speichern von Bilddateien mit [**BitmapDecoder**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging.BitmapDecoder) und [**BitmapEncoder**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging.BitmapEncoder) sowie das Verwenden des [**SoftwareBitmap**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging.SoftwareBitmap)-Objekts zum Darstellen von Bitmapbildern erläutert.

Die **SoftwareBitmap**-Klasse ist eine vielseitige API, die aus mehreren Quellen, z. B. Bilddateien, [**WriteableBitmap**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Imaging.WriteableBitmap)-Objekten, Direct3D-Oberflächen und Code erstellt werden kann. Mit **SoftwareBitmap** können Sie problemlos zwischen den unterschiedlichen Pixelformaten und Alphamodi konvertieren und verfügen über Low-Level-Zugriff auf Pixeldaten. **SoftwareBitmap** stellt des Weiteren eine gemeinsame Schnittstelle dar, die von mehreren Features von Windows verwendet wird, darunter:

-   [**CapturedFrame** ](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.CapturedFrame) können Sie Frames erfasst, die von der Kamera als eine **"softwarebitmap"** .

-   [**VideoFrame** ](https://docs.microsoft.com/uwp/api/Windows.Media.VideoFrame) können Sie eine **"softwarebitmap"** Darstellung einer **VideoFrame**.

-   [**FaceDetector** ](https://docs.microsoft.com/uwp/api/Windows.Media.FaceAnalysis.FaceDetector) können Sie erkennen Gesichter in einem **"softwarebitmap"** .

Der Beispielcode in diesem Artikel verwendet APIs aus den folgenden Namespaces.

[!code-cs[Namespaces](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetNamespaces)]

## <a name="create-a-softwarebitmap-from-an-image-file-with-bitmapdecoder"></a>Erstellen eines SoftwareBitmap-Objekts aus einer Bilddatei mit „BitmapDecoder“

Rufen Sie zum Erstellen eines [**SoftwareBitmap**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging.SoftwareBitmap)-Objekts aus einer Datei eine Instanz von [**StorageFile**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFile) ab, die die Bilddaten enthält. In diesem Beispiel wird [**FileOpenPicker**](https://docs.microsoft.com/uwp/api/Windows.Storage.Pickers.FileOpenPicker) verwendet, damit Benutzer eine Bilddatei auswählen können.

[!code-cs[PickInputFile](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetPickInputFile)]

Rufen Sie die [**OpenAsync**](https://docs.microsoft.com/uwp/api/windows.storage.istoragefile.openasync)-Methode des **StorageFile**-Objekts auf, um einen Datenstrom mit wahlfreiem Zugriff mit den Bilddaten abzurufen. Rufen Sie die statische [**BitmapDecoder.CreateAsync**](https://docs.microsoft.com/uwp/api/windows.graphics.imaging.bitmapdecoder.createasync)-Methode auf, um eine Instanz der [**BitmapDecoder**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging.BitmapDecoder)-Klasse für den angegebenen Datenstrom abzurufen. Rufen Sie [**GetSoftwareBitmapAsync**](https://docs.microsoft.com/uwp/api/windows.graphics.imaging.bitmapdecoder.getsoftwarebitmapasync) auf, um ein [**SoftwareBitmap**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging.SoftwareBitmap)-Objekt mit dem Bild abzurufen.

[!code-cs[CreateSoftwareBitmapFromFile](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetCreateSoftwareBitmapFromFile)]

## <a name="save-a-softwarebitmap-to-a-file-with-bitmapencoder"></a>Speichern eines SoftwareBitmap-Objekts in einer Datei mit „BitmapEncoder“

Rufen Sie zum Speichern eines **SoftwareBitmap**-Objekts in einer Datei eine Instanz von **StorageFile** auf, in der das Bild gespeichert wird. In diesem Beispiel wird [**FileSavePicker**](https://docs.microsoft.com/uwp/api/Windows.Storage.Pickers.FileSavePicker) verwendet, damit Benutzer eine Ausgabedatei auswählen können.

[!code-cs[PickOutputFile](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetPickOutputFile)]

Rufen Sie die [**OpenAsync**](https://docs.microsoft.com/uwp/api/windows.storage.istoragefile.openasync)-Methode des **StorageFile**-Objekts auf, um einen Datenstrom mit wahlfreiem Zugriff abzurufen, in den das Bild geschrieben wird. Rufen Sie die statische [**BitmapEncoder.CreateAsync**](https://docs.microsoft.com/uwp/api/windows.graphics.imaging.bitmapencoder.createasync)-Methode auf, um eine Instanz der [**BitmapEncoder**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging.BitmapEncoder)-Klasse für den angegebenen Datenstrom abzurufen. Der erste Parameter für **CreateAsync** ist eine GUID, die den zum Codieren des Bilds zu verwendenden Codec darstellt. Die **BitmapEncoder**-Klasse stellt eine Eigenschaft bereit, die die ID für jeden vom Encoder unterstützten Codec enthält, z. B. [**JpegEncoderId**](https://docs.microsoft.com/uwp/api/windows.graphics.imaging.bitmapencoder.jpegencoderid).

Verwenden Sie die [**SetSoftwareBitmap**](https://docs.microsoft.com/uwp/api/windows.graphics.imaging.bitmapencoder.setsoftwarebitmap)-Methode zum Festlegen des Bilds, das codiert werden soll. Sie können auch Werte der [**BitmapTransform**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging.BitmapTransform)-Eigenschaft festlegen, um grundlegende Transformationen auf das Bild anzuwenden, während es codiert wird. Die [**IsThumbnailGenerated**](https://docs.microsoft.com/uwp/api/windows.graphics.imaging.bitmapencoder.isthumbnailgenerated)-Eigenschaft bestimmt, ob eine Miniaturansicht vom Encoder generiert wird. Beachten Sie, dass nicht alle Dateiformate die Miniaturansicht unterstützen. Deshalb sollten Sie bei Verwendung dieses Features die Fehler zu nicht unterstützten Vorgängen auffangen, die ausgelöst werden, wenn die Miniaturansicht nicht unterstützt wird.

Rufen Sie die [**FlushAsync**](https://docs.microsoft.com/uwp/api/windows.graphics.imaging.bitmapencoder.flushasync)-Methode auf, damit der Encoder die Bilddaten in die angegebene Datei schreibt.

[!code-cs[SaveSoftwareBitmapToFile](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetSaveSoftwareBitmapToFile)]

Sie können beim Erstellen von **BitmapEncoder** zusätzliche Codierungsoptionen angeben, indem Sie ein neues [**BitmapPropertySet**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging.BitmapPropertySet)-Objekt erstellen und dieses mit einem oder mehreren [**BitmapTypedValue**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging.BitmapTypedValue)-Objekten auffüllen, die die Encodereinstellungen darstellen. Eine Liste der unterstützten Encoderoptionen finden Sie unter [Referenz zu BitmapEncoder-Optionen](bitmapencoder-options-reference.md).

[!code-cs[UseEncodingOptions](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetUseEncodingOptions)]

## <a name="use-softwarebitmap-with-a-xaml-image-control"></a>Verwenden von „SoftwareBitmap“ mit einem XAML-Image-Steuerelement

Um ein Bild unter Verwendung des [**Image**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Image)-Steuerelements auf einer XAML-Seite anzuzeigen, definieren Sie zunächst ein **Image**-Steuerelement in der XAML-Seite.

[!code-xml[ImageControl](./code/ImagingWin10/cs/MainPage.xaml#SnippetImageControl)]

Zurzeit unterstützt das Steuerelement **Image** nur Bilder, die BGRA8-Codierung und einen vormultiplizierten oder No-Alpha-Kanal verwenden. Bevor Sie versuchen, ein Bild anzuzeigen, sollten Sie einen Test durchführen, um sicherzustellen, dass es das richtige Format hat. Wenn dies nicht der Fall ist, verwenden Sie die statische [**Convert**](https://docs.microsoft.com/uwp/api/windows.graphics.imaging.softwarebitmap.convert)-Methode von **SoftwareBitmap**, um das Bild in das unterstützte Format zu konvertieren.

Erstellen Sie ein neues [**SoftwareBitmapSource**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Imaging.SoftwareBitmapSource)-Objekt. Legen Sie den Inhalt des Quellobjekts durch Aufrufen von [**SetBitmapAsync**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imaging.softwarebitmapsource.setbitmapasync) fest, und übergeben Sie dabei ein **SoftwareBitmap**-Objekt. Anschließend können Sie die [**Source**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.image.source)-Eigenschaft des **Image**-Steuerelements auf das neu erstellte **SoftwareBitmapSource**-Objekt festlegen.

[!code-cs[SoftwareBitmapToWriteableBitmap](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetSoftwareBitmapToWriteableBitmap)]

Sie können auch **SoftwareBitmapSource** verwenden, um für [**ImageBrush**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.ImageBrush) ein **SoftwareBitmap**-Objekt als [**ImageSource**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imagebrush.imagesource) festzulegen.

## <a name="create-a-softwarebitmap-from-a-writeablebitmap"></a>Erstellen eines SoftwareBitmap-Objekts aus „WriteableBitmap“

Sie können ein **SoftwareBitmap**-Objekt aus einem vorhandenen **WriteableBitmap**-Objekt erstellen, indem Sie die [**SoftwareBitmap.CreateCopyFromBuffer**](https://docs.microsoft.com/uwp/api/windows.graphics.imaging.softwarebitmap.createcopyfrombuffer)-Methode aufrufen und die **PixelBuffer**-Eigenschaft des **WriteableBitmap**-Objekts zum Festlegen der Pixeldaten bereitstellen. Mit dem zweiten Argument können Sie ein Pixelformat für das neu erstellte **WriteableBitmap**-Objekt anfordern. Sie können die [**PixelWidth**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imaging.bitmapsource.pixelwidth)- und [**PixelHeight**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imaging.bitmapsource.pixelheight)-Eigenschaften des **WriteableBitmap**-Objekts zum Festlegen der Abmessungen des neuen Bilds verwenden.

[!code-cs[WriteableBitmapToSoftwareBitmap](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetWriteableBitmapToSoftwareBitmap)]

## <a name="create-or-edit-a-softwarebitmap-programmatically"></a>Programmgesteuertes Erstellen oder Bearbeiten eines SoftwareBitmap-Objekts

Bisher wurde in diesem Thema das Arbeiten mit Bilddateien behandelt. Sie können ein neues **SoftwareBitmap**-Objekt auch programmgesteuert im Code erstellen und das gleiche Verfahren zum Zugreifen auf und Bearbeiten der Pixeldaten von **SoftwareBitmap** verwenden.

**SoftwareBitmap** verwendet COM-Interop, um den Rohdatenpuffer mit Pixeldaten zur Verfügung zu stellen.

Zur Verwendung von COM-Interop müssen Sie einen Verweis auf den **System.Runtime.InteropServices**-Namespace in Ihr Projekt einbeziehen.

[!code-cs[InteropNamespace](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetInteropNamespace)]

Initialisieren Sie die [**IMemoryBufferByteAccess**](https://docs.microsoft.com/previous-versions/mt297505(v=vs.85))-COM-Schnittstelle, indem Sie Ihrem Namespace den folgenden Code hinzufügen.

[!code-cs[COMImport](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetCOMImport)]

Erstellen Sie ein neues **SoftwareBitmap**-Objekt mit dem gewünschten Pixelformat und der gewünschten Größe. Sie können auch ein vorhandenes **SoftwareBitmap**-Objekt verwenden, für das Sie die Pixeldaten bearbeiten möchten. Rufen Sie die [**SoftwareBitmap.LockBuffer**](https://docs.microsoft.com/uwp/api/windows.graphics.imaging.softwarebitmap.lockbuffer)-Methode zum Abrufen einer Instanz der [**BitmapBuffer**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging.BitmapBuffer)-Klasse auf, die den Pixeldatenpuffer darstellt. Wandeln Sie **BitmapBuffer** in die **IMemoryBufferByteAccess**-COM-Schnittstelle um, und rufen Sie dann [**IMemoryBufferByteAccess.GetBuffer**](https://docs.microsoft.com/windows/desktop/WinRT/imemorybufferbyteaccess-getbuffer) auf, um ein Bytearray mit Daten aufzufüllen. Verwenden Sie die [**BitmapBuffer.GetPlaneDescription**](https://docs.microsoft.com/uwp/api/windows.graphics.imaging.bitmapbuffer.getplanedescription)-Methode zum Abrufen eines [**BitmapPlaneDescription**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging.BitmapPlaneDescription)-Objekts, mit dem Sie den Offset in den Puffer für jedes Pixel berechnen können.

[!code-cs[CreateNewSoftwareBitmap](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetCreateNewSoftwareBitmap)]

Da diese Methode auf den Rohdatenpuffer zugreift, der Windows-Runtime-Typen zugrunde liegt, muss diese mithilfe des Schlüsselworts **unsafe** deklariert werden. Außerdem müssen Sie das Projekt in Microsoft Visual Studio so konfigurieren, dass die Kompilierung von unsicherem Code zugelassen wird, indem Sie die Seite **Eigenschaften** des Projekts öffnen, auf die Eigenschaftenseite **Build** klicken und das Kontrollkästchen **Unsicheren Code zulassen** aktivieren.

## <a name="create-a-softwarebitmap-from-a-direct3d-surface"></a>Erstellen eines SoftwareBitmap-Objekts aus einer Direct3D-Oberfläche

Zum Erstellen eines **SoftwareBitmap**-Objekts aus einer Direct3D-Oberfläche müssen Sie den [**Windows.Graphics.DirectX.Direct3D11**](https://docs.microsoft.com/uwp/api/Windows.Graphics.DirectX.Direct3D11)-Namespace in Ihr Projekt einbeziehen.

[!code-cs[Direct3DNamespace](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetDirect3DNamespace)]

Rufen Sie die [**CreateCopyFromSurfaceAsync**](https://docs.microsoft.com/uwp/api/windows.graphics.imaging.softwarebitmap.createcopyfromsurfaceasync)-Methode zum Erstellen eines neuen **SoftwareBitmap**-Objekts aus der Oberfläche auf. Wie der Name schon sagt, verfügt das neue **SoftwareBitmap**-Objekt über eine separate Kopie der Bilddaten. Änderungen am **SoftwareBitmap**-Objekt haben keine Auswirkungen auf die Direct3D-Oberfläche.

[!code-cs[CreateSoftwareBitmapFromSurface](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetCreateSoftwareBitmapFromSurface)]

## <a name="convert-a-softwarebitmap-to-a-different-pixel-format"></a>Konvertieren eines SoftwareBitmap-Objekts in ein anderes Pixelformat

Die **SoftwareBitmap**-Klasse stellt die statische [**Convert**](https://docs.microsoft.com/uwp/api/windows.graphics.imaging.softwarebitmap.convert)-Methode dar, mit der Sie auf einfache Weise ein neues **SoftwareBitmap**-Objekt erstellen können, das Pixelformat und Alphamodus aus einem vorhandenen **SoftwareBitmap**-Objekt verwendet. Beachten Sie, dass die neu erstellte Bitmap eine separate Kopie der Bilddaten darstellt. Änderungen an der neuen Bitmap haben keine Auswirkungen auf die Quellbitmap.

[!code-cs[Convert](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetConvert)]

## <a name="transcode-an-image-file"></a>Transcodieren einer Bilddatei

Sie können eine Bilddatei direkt von einem [**BitmapDecoder**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging.BitmapDecoder) in ein [**BitmapEncoder**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging.BitmapEncoder) transcodieren. Erstellen Sie eine [**IRandomAccessStream**](https://docs.microsoft.com/uwp/api/Windows.Storage.Streams.IRandomAccessStream)-Schnittstelle aus der Datei, die transcodiert werden soll. Erstellen Sie anhand des Eingabedatenstroms ein neues **BitmapDecoder**-Objekt. Erstellen Sie eine neue [**InMemoryRandomAccessStream**](https://docs.microsoft.com/uwp/api/Windows.Storage.Streams.InMemoryRandomAccessStream)-Klasse für den Encoder zum Schreiben in und Aufrufen von [**BitmapEncoder.CreateForTranscodingAsync**](https://docs.microsoft.com/uwp/api/windows.graphics.imaging.bitmapencoder.createfortranscodingasync), indem Sie den In-Memory-Datenstrom und das Decoderobjekt übergeben. Codierungsoptionen werden beim Transcodieren nicht unterstützt. Verwenden Sie stattdessen [**CreateAsync**](https://docs.microsoft.com/uwp/api/windows.graphics.imaging.bitmapencoder.createasync). Alle Eigenschaften in der Eingabebilddatei, die Sie nicht speziell für den Encoder festlegen, werden unverändert in die Ausgabedatei geschrieben. Rufen Sie die [**FlushAsync**](https://docs.microsoft.com/uwp/api/windows.graphics.imaging.bitmapencoder.flushasync)-Methode auf, damit der Encoder den In-Memory-Datenstrom codiert. Navigieren Sie schließlich zum Anfang des Dateidatenstroms und des In-Memory-Datenstroms, und rufen Sie die [**CopyAsync**](https://docs.microsoft.com/uwp/api/windows.storage.streams.randomaccessstream.copyasync)-Methode auf, um den In-Memory-Datenstrom in den Dateidatenstrom zu schreiben.

[!code-cs[TranscodeImageFile](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetTranscodeImageFile)]

## <a name="related-topics"></a>Verwandte Themen

* [Referenz für "BitmapEncoder"-Optionen](bitmapencoder-options-reference.md)
* [Bildmetadaten](image-metadata.md)
 

 




