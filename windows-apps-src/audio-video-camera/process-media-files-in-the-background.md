---
ms.assetid: B5E3A66D-0453-4D95-A3DB-8E650540A300
description: In diesem Artikel wird beschrieben, wie Sie den MediaProcessingTrigger und eine Hintergrundaufgabe verwenden, um Mediendateien im Hintergrund zu verarbeiten.
title: Verarbeiten von Mediendateien im Hintergrund
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 992648c8c90a8ad62b772d417b2b1beeb6087c53
ms.sourcegitcommit: 6f32604876ed480e8238c86101366a8d106c7d4e
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/21/2019
ms.locfileid: "67318530"
---
# <a name="process-media-files-in-the-background"></a>Verarbeiten von Mediendateien im Hintergrund



In diesem Artikel wird beschrieben, wie Sie [**MediaProcessingTrigger**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.MediaProcessingTrigger) und eine Hintergrundaufgabe verwenden, um Mediendateien im Hintergrund zu verarbeiten.

Mit der in diesem Artikel beschriebenen Beispiel-App kann der Benutzer eine zu transcodierende Eingabemediendatei auswählen und eine Ausgabedatei für das Transcodierungsergebnis angeben. Anschließend wird eine Hintergrundaufgabe gestartet, um den Transcodierungsvorgang auszuführen. [  **MediaProcessingTrigger**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.MediaProcessingTrigger) unterstützt außer der Transcodierung noch viele andere Medienverarbeitungsszenarien, einschließlich Rendern von Medienkompositionen auf Datenträger und Hochladen von verarbeiteten Mediendateien nach Abschluss der Verarbeitung.

Ausführlichere Informationen zu den verschiedenen universellen Windows-App-Features in diesem Beispiel finden Sie unter:

-   [Transkodieren von Mediendateien](transcode-media-files.md)
-   [Fortsetzen und Hintergrundaufgaben starten](https://docs.microsoft.com/windows/uwp/launch-resume/index)
-   [Kacheln, Signale und Benachrichtigungen](https://docs.microsoft.com/windows/uwp/controls-and-patterns/tiles-badges-notifications)

## <a name="create-a-media-processing-background-task"></a>Erstellen einer Hintergrundaufgabe für die Medienverarbeitung

Um Ihrer vorhandenen Microsoft Visual Studio-Projektmappe eine Hintergrundaufgabe hinzuzufügen, geben Sie einen Namen für die Komposition ein.

1.  Wählen Sie im Menü **Datei** die Option **Hinzufügen** und dann **Neues Projekt** aus.
2.  Wählen Sie den Projekttyp **Komponente für Windows-Runtime (Universal Windows)** aus.
3.  Geben Sie einen Namen für das neue Komponentenprojekt ein. In diesem Beispiel wird der Projektname **MediaProcessingBackgroundTask** verwendet.
4.  Klicken Sie auf „OK“.

Klicken Sie im **Projektmappen-Explorer**mit der rechten Maustaste auf das Symbol für die Datei „Class1.cs“, die standardmäßig erstellt wird, und wählen Sie **Umbenennen**aus. Benennen Sie die Datei in „MediaProcessingTask.cs“ um. Wenn Sie von Visual Studio gefragt werden, ob Sie alle Verweise auf diese Klasse umbenennen möchten, klicken Sie auf **Ja**.

Fügen Sie in der umbenannten Klassendatei die folgenden **using**-Direktiven hinzu, um diese Namespaces in Ihr Projekt einzuschließen.
                                  
[!code-cs[BackgroundUsing](./code/MediaProcessingTriggerWin10/cs/MediaProcessingBackgroundTask/MediaProcessingTask.cs#SnippetBackgroundUsing)]

Aktualisieren Sie die Klassendeklaration so, dass die Klasse von [**IBackgroundTask**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.IBackgroundTask) erbt.

[!code-cs[BackgroundClass](./code/MediaProcessingTriggerWin10/cs/MediaProcessingBackgroundTask/MediaProcessingTask.cs#SnippetBackgroundClass)]

Fügen Sie der Klasse die folgenden Membervariablen hinzu:

-   Eine [**IBackgroundTaskInstance**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.IBackgroundTaskInstance)-Schnittstelle, die verwendet wird, um die Vordergrund-App mit dem Status der Hintergrundaufgabe zu aktualisieren.
-   Eine [**BackgroundTaskDeferral**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskDeferral)-Klasse, die verhindert, dass das System die Hintergrundaufgabe herunterfährt, während die Medientranscodierung asynchron ausgeführt wird.
-   Ein **CancellationTokenSource**-Objekt, mit dem der asynchrone Transcodierungsvorgang abgebrochen werden kann.
-   Das [**MediaTranscoder**](https://docs.microsoft.com/uwp/api/Windows.Media.Transcoding.MediaTranscoder)-Objekt, das zum Transcodieren von Mediendateien verwendet wird.

[!code-cs[BackgroundMembers](./code/MediaProcessingTriggerWin10/cs/MediaProcessingBackgroundTask/MediaProcessingTask.cs#SnippetBackgroundMembers)]

Das System ruft die [**Run**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.ibackgroundtask.run)-Methode einer Hintergrundaufgabe auf, wenn die Aufgabe gestartet wird. Legen Sie das an die Methode übergebene [**IBackgroundTask**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.IBackgroundTask)-Objekt auf die entsprechende Membervariable fest. Registrieren Sie einen Handler für das [**Canceled**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.ibackgroundtaskinstance.canceled)-Ereignis, das ausgelöst wird, wenn das System die Hintergrundaufgabe herunterfahren muss. Legen Sie anschließend die [**Progress**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.ibackgroundtaskinstance.progress)-Eigenschaft auf 0 (null) fest.

Rufen Sie als Nächstes die [**GetDeferral**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.ibackgroundtaskinstance.getdeferral)-Methode des Hintergrundaufgabenobjekts auf, um eine Verzögerung abzurufen. Dies weist das System an, die Aufgabe nicht herunterzufahren, da Sie asynchrone Vorgänge ausführen.

Rufen Sie als Nächstes die **TranscodeFileAsync**-Hilfsmethode auf, die im nächsten Abschnitt definiert wird. Wenn diese Methode erfolgreich abgeschlossen wird, wird eine Hilfsmethode aufgerufen, um eine Popupbenachrichtigung zu öffnen, die den Benutzer darauf hinweist, dass die Transcodierung abgeschlossen ist.

Rufen Sie am Ende der **Run**-Methode die [**Complete**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.backgroundtaskdeferral.complete)-Methode für das Verzögerungsobjekt auf, um dem System mitzuteilen, dass die Hintergrundaufgabe abgeschlossen ist und beendet werden kann.

[!code-cs[Run](./code/MediaProcessingTriggerWin10/cs/MediaProcessingBackgroundTask/MediaProcessingTask.cs#SnippetRun)]

In der **TranscodeFileAsync**-Hilfsmethode werden die Dateinamen für die Eingabe- und Ausgabedateien für die Transcodierungsvorgänge aus der [**LocalSettings**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.localsettings)-Eigenschaft für Ihre App abgerufen. Diese Werte werden von der Vordergrund-App festgelegt. Erstellen Sie ein [**StorageFile**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFile)-Objekt für die Eingabe- und Ausgabedateien, und erstellen Sie dann ein Codierungsprofil für die Transcodierung.

Rufen Sie [**PrepareFileTranscodeAsync**](https://docs.microsoft.com/uwp/api/windows.media.transcoding.mediatranscoder.preparefiletranscodeasync) auf, und übergeben Sie die Eingabedatei, die Ausgabedatei und das Codierungsprofil. Das von diesem Aufruf zurückgegebene [**PrepareTranscodeResult**](https://docs.microsoft.com/uwp/api/Windows.Media.Transcoding.PrepareTranscodeResult)-Objekt teilt Ihnen mit, ob die Transcodierung durchgeführt werden kann. Wenn die [**CanTranscode**](https://docs.microsoft.com/uwp/api/windows.media.transcoding.preparetranscoderesult.cantranscode)-Eigenschaft „true“ ist, rufen Sie [**TranscodeAsync**](https://docs.microsoft.com/uwp/api/windows.media.transcoding.preparetranscoderesult.transcodeasync) auf, um den Transcodierungsvorgang auszuführen.

Mit der **AsTask**-Methode können Sie den Status des asynchronen Vorgangs nachverfolgen oder den Vorgang abbrechen. Erstellen Sie ein neues **Progress**-Objekt, wobei Sie die gewünschten Statuseinheiten und den Namen der Methode angeben, die aufgerufen wird, um Sie über den aktuellen Status der Aufgabe zu informieren. Übergeben Sie das **Progress**-Objekt zusammen mit dem Abbruchtoken, das Ihnen das Abbrechen der Aufgabe ermöglicht, an die **AsTask**-Methode.

[!code-cs[TranscodeFileAsync](./code/MediaProcessingTriggerWin10/cs/MediaProcessingBackgroundTask/MediaProcessingTask.cs#SnippetTranscodeFileAsync)]

Legen Sie in der Methode, die Sie zum Erstellen des Statusobjekts im vorherigen Schritt verwendet haben (**Progress**), den Status der Hintergrundaufgabeninstanz fest. Hierdurch wird der Status an die Vordergrund-App übergeben, sofern diese ausgeführt wird.

[!code-cs[Progress](./code/MediaProcessingTriggerWin10/cs/MediaProcessingBackgroundTask/MediaProcessingTask.cs#SnippetProgress)]

Die **SendToastNotification**-Hilfsmethode erstellt eine neue Popupbenachrichtigung, indem ein XML-Vorlagendokument für ein Popup abgerufen wird, das nur Text enthält. Das Textelement des Popup-XML-Codes wird festgelegt. Anschließend wird ein neues [**ToastNotification**](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.ToastNotification)-Objekt aus dem XML-Dokument erstellt. Schließlich wird dem Benutzer das Popup durch Aufrufen von [**ToastNotifier.Show**](https://docs.microsoft.com/uwp/api/windows.ui.notifications.toastnotifier.show) angezeigt.

[!code-cs[SendToastNotification](./code/MediaProcessingTriggerWin10/cs/MediaProcessingBackgroundTask/MediaProcessingTask.cs#SnippetSendToastNotification)]

Im Handler für das [**Canceled**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.ibackgroundtaskinstance.canceled)-Ereignis, das aufgerufen wird, wenn das System die Hintergrundaufgabe abbricht, können Sie den Fehler zu Telemetriezwecken protokollieren.

[!code-cs[OnCanceled](./code/MediaProcessingTriggerWin10/cs/MediaProcessingBackgroundTask/MediaProcessingTask.cs#SnippetOnCanceled)]

## <a name="register-and-launch-the-background-task"></a>Registrieren und Starten der Hintergrundaufgabe

Bevor die Hintergrundaufgabe aus der Vordergrund-App gestartet werden kann, müssen Sie die Datei „Package.appmanifest“ der Vordergrund-App aktualisieren, um dem System mitzuteilen, dass Ihre App eine Hintergrundaufgabe verwendet.

1.  Doppelklicken Sie im **Projektmappen-Explorer** auf das Symbol der Datei „Package.appmanifest“, um den Manifest-Editor zu öffnen.
2.  Klicken Sie auf die Registerkarte **Deklarationen**.
3.  Wählen Sie unter **Verfügbare Deklarationen** die Option **Hintergrundaufgaben** aus, und klicken Sie auf **Hinzufügen**.
4.  Vergewissern Sie sich, dass unter **Unterstützte Deklarationen** das Element **Hintergrundaufgaben** ausgewählt ist. Aktivieren Sie unter **Eigenschaften** das Kontrollkästchen für **Medienverarbeitung**.
5.  Geben Sie im Textfeld **Einstiegspunkt** den Namespace und den Klassennamen für den Hintergrundtest durch einen Punkt getrennt an. Für dieses Beispiel lautet die Eingabe:
   ```csharp
   MediaProcessingBackgroundTask.MediaProcessingTask
   ```
Als Nächstes müssen Sie der Vordergrund-App einen Verweis auf die Hintergrundaufgabe hinzufügen.
1.  Klicken Sie im **Projektmappen-Explorer** unter dem Vordergrund-App-Projekt mit der rechten Maustaste auf den Ordner **Verweise**, und wählen Sie **Verweis hinzufügen** aus.
2.  Erweitern Sie den Knoten **Projekte**, und wählen Sie **Projektmappe** aus.
3.  Aktivieren Sie das Kontrollkästchen neben dem Hintergrundaufgabenprojekt, und klicken Sie auf **OK**.

Fügen Sie den restlichen Code in diesem Beispiel der Vordergrund-App hinzu. Zunächst müssen Sie dem Projekt die folgenden Namespaces hinzufügen.

[!code-cs[ForegroundUsing](./code/MediaProcessingTriggerWin10/cs/MediaProcessingTriggerWin10/MainPage.xaml.cs#SnippetForegroundUsing)]

Als Nächstes fügen Sie die folgenden Membervariablen hinzu, die zum Registrieren der Hintergrundaufgabe benötigt werden.

[!code-cs[ForegroundMembers](./code/MediaProcessingTriggerWin10/cs/MediaProcessingTriggerWin10/MainPage.xaml.cs#SnippetForegroundMembers)]

Die **PickFilesToTranscode**-Hilfsmethode verwendet eine [**FileOpenPicker**](https://docs.microsoft.com/uwp/api/Windows.Storage.Pickers.FileOpenPicker)-Klasse und eine [**FileSavePicker**](https://docs.microsoft.com/uwp/api/Windows.Storage.Pickers.FileSavePicker)-Klasse, um die Eingabe- und Ausgabedateien für die Transcodierung zu öffnen. Der Benutzer kann Dateien an einem Speicherort auswählen, auf den Ihre App keinen Zugriff auf. Um sicherzustellen, dass die Hintergrundaufgabe die Dateien öffnen kann, fügen Sie sie der [**FutureAccessList**](https://docs.microsoft.com/uwp/api/windows.storage.accesscache.storageapplicationpermissions.futureaccesslist)-Eigenschaft für die App hinzu.

Legen Sie abschließend Einträge für die Eingabe- und Ausgabedateinamen in der [**LocalSettings**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.localsettings)-Eigenschaft für Ihre App fest. Die Hintergrundaufgabe ruft die Dateinamen von diesem Speicherort ab.

[!code-cs[PickFilesToTranscode](./code/MediaProcessingTriggerWin10/cs/MediaProcessingTriggerWin10/MainPage.xaml.cs#SnippetPickFilesToTranscode)]

Um die Hintergrundaufgabe registrieren, erstellen Sie eine neue [**MediaProcessingTrigger**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.MediaProcessingTrigger)-Klasse und eine neue [**BackgroundTaskBuilder**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskBuilder)-Klasse. Legen Sie den Namen des Hintergrundaufgaben-Generators fest, damit Sie ihn später identifizieren können. Legen Sie die [**TaskEntryPoint**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.backgroundtaskbuilder.taskentrypoint)-Eigenschaft auf die gleiche Namespace- und Klassennamenszeichenfolge fest, die Sie in der Manifestdatei verwendet haben. Legen Sie die [**Trigger**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.backgroundtaskregistration.trigger)-Eigenschaft auf die **MediaProcessingTrigger** Instanz fest.

Stellen Sie vor dem Registrieren der Aufgabe sicher, dass Sie die Registrierung aller zuvor registrierten Aufgaben aufheben, indem Sie eine Schleife durch die [**AllTasks**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.backgroundtaskregistration.alltasks)-Sammlung durchführen. Rufen Sie zudem [**Unregister**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.ibackgroundtaskregistration.unregister) für alle Aufgaben auf, die den in der [**BackgroundTaskBuilder.Name**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.backgroundtaskbuilder.name)-Eigenschaft angegebenen Namen aufweisen.

Registrieren Sie die Hintergrundaufgabe durch Aufrufen von [**Register**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.backgroundtaskbuilder.register). Registrieren Sie Handler für das [**Completed**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.backgroundtaskregistration.completed)-Ereignis und das [**Progress**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.ibackgroundtaskregistration.progress)-Ereignis.

[!code-cs[RegisterBackgroundTask](./code/MediaProcessingTriggerWin10/cs/MediaProcessingTriggerWin10/MainPage.xaml.cs#SnippetRegisterBackgroundTask)]

Eine typische app wird die Hintergrundaufgabe registrieren, wenn die app zuerst gestartet werden, z. B. in der **OnNavigatedTo** Ereignis.

Starten Sie die Hintergrundaufgabe durch Aufrufen der [**RequestAsync**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.mediaprocessingtrigger.requestasync)-Methode des **MediaProcessingTrigger**-Objekts. Das von dieser Methode zurückgegebene [**MediaProcessingTriggerResult**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.MediaProcessingTriggerResult)-Objekt informiert Sie darüber, ob die Hintergrundaufgabe erfolgreich gestartet wurde. Zudem teilt es Ihnen bei einem Fehler mit, warum die Hintergrundaufgabe nicht gestartet wurde. 

[!code-cs[LaunchBackgroundTask](./code/MediaProcessingTriggerWin10/cs/MediaProcessingTriggerWin10/MainPage.xaml.cs#SnippetLaunchBackgroundTask)]

Eine typische app startet die Hintergrundaufgabe als Reaktion auf Benutzerinteraktionen, z. B. der **klicken Sie auf** Ereignis ein UI-Steuerelement.

Der **OnProgress**-Ereignishandler wird aufgerufen, wenn die Hintergrundaufgabe den Vorgangsstatus aktualisiert. Sie können diese Möglichkeit nutzen, um die Benutzeroberfläche mit Statusinformationen zu aktualisieren.

[!code-cs[OnProgress](./code/MediaProcessingTriggerWin10/cs/MediaProcessingTriggerWin10/MainPage.xaml.cs#SnippetOnProgress)]

Der **OnCompleted**-Ereignishandler wird aufgerufen, wenn die Ausführung der Hintergrundaufgabe abgeschlossen ist. Dies ist eine weitere Möglichkeit zum Aktualisieren der Benutzeroberfläche, um Statusinformationen für den Benutzer bereitzustellen.

[!code-cs[OnCompleted](./code/MediaProcessingTriggerWin10/cs/MediaProcessingTriggerWin10/MainPage.xaml.cs#SnippetOnCompleted)]


 

 




