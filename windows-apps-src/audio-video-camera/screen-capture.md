---
author: eliotcowley
title: Bildschirmaufnahme
description: Der Windows.Graphics.Capture-Namespace enthält APIs zum Abrufen von Frames aus einer Anzeige oder einem Anwendungsfenster, um Videostreams zu erstellen oder gemeinsame und interaktive Benutzeroberflächen zu erstellen.
ms.assetid: 349C959D-9C74-44E7-B5F6-EBDB5CA87B9F
ms.author: elcowle
ms.date: 3/1/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows10, UWP, Bildschirmaufnahme
ms.localizationpriority: medium
ms.openlocfilehash: 2b7883acd351c721b4539141cd46e3c199a8d8a1
ms.sourcegitcommit: ef5a1e1807313a2caa9c9b35ea20b129ff7155d0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/08/2018
ms.locfileid: "1639809"
---
# <a name="screen-capture"></a>Bildschirmaufnahme

Ab Windows 10,Version 1803, enthält der [Windows.Graphics.Capture](https://docs.microsoft.com/uwp/api/windows.graphics.capture) APIs zum Abrufen von Frames aus einer Anzeige oder einem Anwendungsfenster, um Videostreams zu erstellen oder gemeinsame und interaktive Benutzeroberflächen zu erstellen.

Mit der Bildschirmaufnahme können Entwickler sichere System-UIs für Endbenutzer aufrufen, um das Fenster für die Anzeige oder die Anwendung für die Aufzeichnung auszuwählen. Es wird ein gelber Rahmen mit einer Benachrichtigung vom System um das aktiv erfasste Element herum gezeichnet. Im Fall von mehreren gleichzeitigen Aufnahmesitzungen wird ein gelber Rahmen um jedes erfasste Element gezeichnet.

## <a name="add-the-screen-capture-capability"></a>Hinzufügen der Bildschirmaufnahmefunktion

Die APIs unter dem **Windows.Graphics.Capture**-Namespace erfordern eine allgemeine Funktion, um in Ihrem App-Manifest deklariert werden zu können. Diese müssen der Datei direkt hinzugefügt werden:
    
1. Klicken Sie mit der rechten Maustaste auf **Solution Explorer** auf die Datei **appxmanifest**. 
2. Wählen Sie **Öffnen mit** aus 
3. Wählen Sie **XML (Text)-Editor** aus. 
4. Wählen Sie **OK** aus.
5. Fügen Sie dem **Paket**-Knoten das folgende Attribut hinzu:  `xmlns:uap6="http://schemas.microsoft.com/appx/manifest/uap/windows10/6"`
6. Fügen Sie dem **Paket**-Knoten ebenfalls die folgenden Optionen zum **IgnorableNamespaces**-Attribut hinzu:  `uap6`
7. Fügen Sie dem **Funktionen**-Knoten das folgende Element hinzu:  `<uap6:Capability Name="graphicsCapture"/>`

## <a name="launch-the-system-ui-to-start-screen-capture"></a>Starten Sie die System-Benutzeroberfläche, um die Bildschirmaufnahme zu starten

Vor dem Starten der Systembenutzeroberfläche sollten Sie überprüfen, ob Ihre Anwendung momentan Bildschirmaufnahmen erstellen kann. Es gibt verschiedene Gründe, warum die Anwendung möglicherweise keine Bildschirmaufnahme erstellen kann, z.B. wenn das Gerät die Hardwareanforderungen nicht erfüllt, oder wenn die Anwendung für die Aufnahme Bildschirmaufnahmen blockiert. Verwenden Sie die **IsSupported**-Methode in der [GraphicsCaptureSession](https://docs.microsoft.com/uwp/api/windows.graphics.capture.graphicscapturesession)-Klasse, um zu bestimmen, ob UWP.Bildschirmaufnahmen unterstützt werden:

```cs
// This runs when the application starts.
public void OnInitialization()
{
    if (!GraphicsCaptureSession.IsSupported()) 
    {
        // Hide the capture UI if screen capture is not supported.
        CaptureButton.Visibility = Visibility.Collapsed; 
    }    
}
```

Nachdem Sie überprüft haben, dass Bildschirmaufnahmen unterstützt werden, verwenden Sie die [GraphicsCapturePicker](https://docs.microsoft.com/uwp/api/windows.graphics.capture.graphicscapturepicker)-Klasse verwenden, um die System-Auswahl-UI aufzurufen. Der Benutzer verwendet die Benutzeroberfläche, um das Anzeige- oder Anwendungsfenster für die auszuführende Bildschirmaufnahmen auszuwählen. Die Auswahl gibt [GraphicsCaptureItem](https://docs.microsoft.com/uwp/api/windows.graphics.capture.graphicscaptureitem)zurück, das verwendet wird, um eine **GraphicsCaptureSession** zu erstellen:

```cs
public async Task StartCaptureAsync() 
{ 
    // The GraphicsCapturePicker follows the same pattern the 
    // file pickers do. 
    var picker = new GraphicsCapturePicker(); 
    GraphicsCaptureItem item = await picker.PickSingleItemAsync(); 

    // The item may be null if the user dismissed the 
    // control without making a selection or hit Cancel. 
    if (item != null) 
    {
        // We'll define this method later in the document.
        StartCaptureInternal(item); 
    } 
}
```

## <a name="create-a-capture-frame-pool-and-capture-session"></a>Erstellen Sie ein Frameerfassungspool und eine Sammlungssitzung

Mithilfe der **GraphicsCaptureItem** erstellen Sie ein [Direct3D11CaptureFramePool](https://docs.microsoft.com/uwp/api/windows.graphics.capture.direct3d11captureframepool) mit Ihrem D3D-Gerät, das unterstützte Pixel-Format (**DXGI\_FORMAT\_B8G8R8A8\_UNORM**), und die Anzahl der gewünschten Frames (die eine ganze Zahl sein können), und die Framegröße. Die **ContentSize**-Eigenschaft der **GraphicsCaptureItem**-Klasse kann ebenfalls als Anzahl der Frames verwendet werden:

```cs
private GraphicsCaptureItem _item;
private Direct3D11CaptureFramePool _framePool;
private CanvasDevice _canvasDevice;
private GraphicsCaptureSession _session;

public void StartCaptureInternal(GraphicsCaptureItem item) 
{
    _item = item;

    _framePool = Direct3D11CaptureFramePool.Create( 
        _canvasDevice, // D3D device 
        DirectXPixelFormat.B8G8R8A8UIntNormalized, // Pixel format 
        2, // Number of frames 
        _item.Size); // Size of the buffers   
} 
```

Als Nächstes rufen Sie eine Instanz der **GraphicsCaptureSession**-Klasse für Ihren **Direct3D11CaptureFramePool** durch Übergabe der **GraphicsCaptureItem** auf der **CreateCaptureSession**-Methode auf:

```cs
_session = _framePool.CreateCaptureSession(_item);
```

Nachdem der Benutzer die Zustimmung für die Aufnahme eines Anwendungsfensters oder einer Anzeige in der System-Benutzeroberfläche erteilt hat, kann die **GraphicsCaptureItem** auf mehrere **CaptureSession**-Objekte angewendet werden. Auf diese Weise kann die Anwendung das gleiche Element für verschiedene Umgebungen erfassen.

Um mehrere Elemente gleichzeitig zu erfassen, muss die Anwendung eine Sammlungssitzung für jedes aufzuzeichnende Element erstellen, was das Aufrufen der Dateiauswahl-UI für jedes Element erfordert, das erfasst werden muss.

## <a name="acquire-capture-frames"></a>Erwerben von Aufnahmeframes

Rufen Sie nach der Erstellung des Frame-Pool und der Aufnahmesitzung die **StartCapture**-Methode für Ihre **GraphicsCaptureSession**-Instanz auf, die das System benachrichtigt, die Aufnahmeframes an Ihre App zu senden:

```cs
_session.StartCapture();
```

Um diese Aufnahmeframes zu erwerben, die [Direct3D11CaptureFrame](https://docs.microsoft.com/uwp/api/windows.graphics.capture.direct3d11captureframe)-Objekte sind, können Sie das **Direct3D11CaptureFramePool.FrameArrived**-Ereignis verwenden:

```cs
_framePool.FrameArrived += (s, a) => 
{ 
    // The FrameArrived event fires for every frame on the thread that         
    // created the Direct3D11CaptureFramePool. This means we don't have to 
    // do a null-check here, as we know we're the only one  
    // dequeueing frames in our application.  

    // NOTE: Disposing the frame retires it and returns  
    // the buffer to the pool.
    using (var frame = _framePool.TryGetNextFrame()) 
    {
        // We'll define this method later in the document.
        ProcessFrame(frame); 
    }  
}; 
```

Es wird empfohlen, den UI-Thread für **FrameArrived** nach Möglichkeit zu vermeiden, da dieses Ereignis jedes Mal ausgelöst wird, wenn ein neuer Frame verfügbar ist, was häufig vorkommt. Wenn Sie sich entscheiden, **FrameArrived** im UI-Thread zu übernehmen, überlegen Sie, wie viel Arbeit Sie jedes Mal machen, wenn das Ereignis ausgelöst wird.

Alternativ können Sie manuell Frames mit der **Direct3D11CaptureFramePool.TryGetNextFrame**-Methode ziehen, bis Sie alle Frames haben, die Sie benötigen.

Das **Direct3D11CaptureFrame**-Objekt enthält die Eigenschaften **ContentSize**, **Surface** und **SystemRelativeTime**. Die **SystemRelativeTime** ist die QPC ([QueryPerformanceCounter](https://msdn.microsoft.com/library/windows/desktop/ms644904))-Zeit, die zum Synchronisieren von anderen Medienelemente verwendet werden kann.

## <a name="processing-capture-frames"></a>Verarbeiten von Aufnahmeframes

Frames aus **Direct3D11CaptureFramePool** werden beim Aufrufen von **TryGetNextFrame** ausgecheckt, und gemäß der Lebensdauer des **Direct3D11CaptureFrame**-Objekts wieder eingecheckt. Für systemeigene Anwendungen reicht das Veröffentlichen des **Direct3D11CaptureFrame**-Objekts aus, um die Frame zurück in den Frame-Pool einzuchecken. Für verwaltete Anwendungen wird empfohlen, die **Direct3D11CaptureFrame.Dispose** (**Schließen** in C++)-Methode zu verwenden. **Direct3D11CaptureFrame** implementiert die [IClosable](https://docs.microsoft.com/uwp/api/Windows.Foundation.IClosable)-Schnittstelle, die als [IDisposable](https://msdn.microsoft.com/library/system.idisposable.aspx) für C#-Aufrufer projiziert wird.

Anwendungen sollten weder Verweise auf **Direct3D11CaptureFrame**-Objekte speichern, noch Verweise auf die zugrunde liegende Direct3D-Oberfläche speichern, nachdem der Frame erneut eingecheckt wurde.

Während der Verarbeitung eines Frames, empfiehlt es sich, dass Anwendungen die [ID3D11Multithread](https://msdn.microsoft.com/library/windows/desktop/mt644886)-Sperre auf dem gleichen Gerät wie das **Direct3D11CaptureFramePool**-Objekt verwenden.

Die zugrunde liegende Direct3D-Oberfläche gibt immer die Größe an, die beim Erstellen (oder neuerstellen) von **Direct3D11CaptureFramePool** angegeben wird. Wenn der Inhalt größer als der Frame ist, werden die Inhalte auf die Größe des Frame zugeschnitten. Wenn der Inhalt kleiner als der Frames ist, enthält der Rest des Frames nicht definierte Daten. Es wird empfohlen, dass Anwendungen eine Sub-Rect mit der **ContentSize**-Eigenschaft für den **Direct3D11CaptureFrame** kopieren, um zu verhindern, dass der Inhalt nicht definiert ist.

## <a name="react-to-capture-item-resizing-or-device-lost"></a>Auf das Ändern der Größe von Elementen oder verlorene Geräte reagieren

Während der Aufnahme können Anwendungen Aspekte der ihrer **Direct3D11CaptureFramePool** ändern. Dies umfasst die Bereitstellung eines neuen Direct3D-Geräts, das Ändern der Größe der Framepuffer oder das Ändern der Anzahl der Puffer innerhalb des Pools. In jedem dieser Szenarien ist die **Recreate**-Methode für das **Direct3D11CaptureFramePool**-Objekt das empfohlene Tool.

Wenn **Recreate** aufgerufen wird, werden alle vorhandenen Frames verworfen. Dadurch wird die Ausgabe von Frames verhindert, deren zugrunde liegende Direct3D-Oberfläche zu einem Gerät gehören, auf das die Anwendung nicht mehr zugreifen kann. Aus diesem Grund sollten alle ausstehenden Frames vor dem Aufruf von **Recreate** verarbeitet werden.

## <a name="putting-it-all-together"></a>Zusammenfassung

Der folgende Codeausschnitt ist ein End-to-End-Beispiel für die Implementierung einer Bildschirmaufnahme in einer UWP-Anwendung:

```cs
using Microsoft.Graphics.Canvas; 
using System; 
using System.Threading.Tasks; 
using Windows.Graphics.Capture; 
using Windows.Graphics.DirectX; 
using Windows.UI.Composition; 
 
namespace CaptureSamples 
{ 
    class Sample
    {
        // Capture API objects.
        private Vector2 _lastSize; 
        private GraphicsCaptureItem _item; 
        private Direct3D11CaptureFramePool _framePool; 
        private GraphicsCaptureSession _session; 
 
        // Non-API related members.
        private CanvasDevice _canvasDevice; 
        private CompositionDrawingSurface _surface; 

        public async Task StartCaptureAsync() 
        { 
            // The GraphicsCapturePicker follows the same pattern the 
            // file pickers do. 
            var picker = new GraphicsCapturePicker(); 
            GraphicsCaptureItem item = await picker.PickSingleItemAsync(); 
 
            // The item may be null if the user dismissed the 
            // control without making a selection or hit Cancel. 
            if (item != null) 
            { 
                StartCaptureInternal(item); 
            }
        } 
 
        private void StartCaptureInternal(GraphicsCaptureItem item) 
        { 
             // Stop the previous capture if we had one.
            StopCapture(); 
 
            _item = item; 
            _lastSize = _item.Size; 
 
             _framePool = Direct3D11CaptureFramePool.Create( 
                _canvasDevice, // D3D device 
                DirectXPixelFormat.B8G8R8A8UIntNormalized, // Pixel format 
                2, // Number of frames 
                _item.Size); // Size of the buffers 
 
            _framePool.FrameArrived += (s, a) => 
            { 
                // The FrameArrived event is raised for every frame on the thread
                // that created the Direct3D11CaptureFramePool. This means we 
                // don't have to do a null-check here, as we know we're the only 
                // one dequeueing frames in our application.  
 
                // NOTE: Disposing the frame retires it and returns  
                // the buffer to the pool.
 
                using (var frame = _framePool.TryGetNextFrame()) 
                { 
                    ProcessFrame(frame); 
                }  
            }; 
 
            _item.Closed += (s, a) => 
            { 
                StopCapture(); 
            }; 
 
            _session = _framePool.CreateCaptureSession(_item); 
            _session.Start(); 
        } 
 
        public void StopCapture() 
        { 
            _session?.Dispose(); 
            _framePool?.Dispose(); 
            _item = null; 
            _session = null; 
            _framePool = null; 
        } 
 
        private void ProcessFrame(Direct3D11CaptureFrame frame) 
        { 
            // Resize and device-lost leverage the same function on the
            // Direct3D11CaptureFramePool. Refactoring it this way avoids 
            // throwing in the catch block below (device creation could always 
            // fail) along with ensuring that resize completes successfully and 
            // isn’t vulnerable to device-lost.   
            bool needsReset = false; 
            bool recreateDevice = false; 
 
            if (frame.ContentSize != _lastSize) 
            { 
                needsReset = true; 
                _lastSize = frame.ContentSize; 
            } 
            
            try 
            { 
                // Take the D3D11 surface and draw it into a  
                // Composition surface.
 
                // Convert our D3D11 surface into a Win2D object.
                var canvasBitmap = CanvasBitmap.CreateFromDirect3D11Surface( 
                    _canvasDevice, 
                    frame.Surface); 
 
                // Helper that handles the drawing for us, not shown. 
                FillSurfaceWithBitmap(_surface, canvasBitmap); 
            } 
            // This is the device-lost convention for Win2D.
            catch(Exception e) when (_canvasDevice.IsDeviceLost(e.HResult)) 
            { 
                // We lost our graphics device. Recreate it and reset 
                // our Direct3D11CaptureFramePool.  
                needsReset = true; 
                recreateDevice = true; 
            } 
 
            if (needsReset) 
            { 
                ResetFramePool(frame.ContentSize, recreateDevice); 
            }
        } 
 
        private void ResetFramePool(Vector2 size, bool recreateDevice) 
        { 
            do 
            { 
                try 
                { 
                    if (recreateDevice) 
                    { 
                        _canvasDevice = new CanvasDevice(); 
                    } 
 
                    _framePool.Recreate( 
                        _canvasDevice,  
                        DirectXPixelFormat.B8G8R8A8UIntNormalized,  
                        2, 
                        size); 
                } 
                // This is the device-lost convention for Win2D.
                catch(Exception e) when (_canvasDevice.IsDeviceLost(e.HResult)) 
                { 
                    _canvasDevice = null; 
                    recreateDevice = true; 
                } 
            } while (_canvasDevice == null); 
        } 
    } 
} 
```

## <a name="see-also"></a>Weitere Informationen:

* [Windows.Graphics.Capture Namespace](https://docs.microsoft.com/uwp/api/windows.graphics.capture)