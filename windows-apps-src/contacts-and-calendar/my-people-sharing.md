---
title: Freigeben von „Meine Kontakte”
description: Erläutert das Hinzufügen von Support für das Freigeben von „Meine Kontakte”
ms.date: 06/28/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: ff37a243f88bdd378998070f58ec35196c62a6cf
ms.sourcegitcommit: 26bb75084b9d2d2b4a76d4aa131066e8da716679
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/06/2020
ms.locfileid: "75683488"
---
# <a name="my-people-sharing"></a>Freigeben von „Meine Kontakte”

Die Feature „Meine Kontakte” ermöglicht Benutzern das Anheften von Kontakten an die Taskleiste, damit sie überall über Windows in Kontakt bleiben können unabhängig von der Anwendung, durch die sie verbunden sind. Jetzt können Benutzer Inhalte für ihre angeheftete Kontakte freigeben, indem sie Dateien aus dem Datei-Explorer auf die Startseite von „Meine Kontakte” anheften. Sie können Inhalte ebenfalls für alle Kontakte im Kontaktspeicher von Windows über den standardmäßigen Charms "Teilen" freigeben. Nachfolgend erhalten Sie weitere Informationen zum Aktivieren Ihrer Anwendung als Freigabeziel für Meine Kontakte.

![„Meine Kontakte”-Freigabecenter](images/my-people-sharing.png)

## <a name="requirements"></a>Anforderungen

+ Windows 10 und Microsoft Visual Studio 2019. Ausführliche Informationen zur Installation finden Sie unter [Visual Studio einrichten](https://docs.microsoft.com/windows/uwp/get-started/get-set-up).
+ Grundkenntnisse in C# oder einer ähnlichen objektorientierten Programmiersprache. Die ersten Schritte mit C# finden Sie unter [Erstellen der App „Hello, world“ (C++)](https://docs.microsoft.com/windows/uwp/get-started/create-a-hello-world-app-xaml-universal).

## <a name="overview"></a>Übersicht

Es gibt drei Schritte, die Sie ausführen müssen, um die Anwendung als Freigabeziel für „Meine Kontakte” zu verwenden:

1. [Deklarieren Sie die Unterstützung für den sharetarget-Aktivierungs Vertrag in Ihrem Anwendungs Manifest.](https://docs.microsoft.com/windows/uwp/contacts-and-calendar/my-people-sharing#declaring-support-for-the-share-contract)
2. [Kommentieren Sie die Kontakte, die die Benutzer für die Verwendung Ihrer APP freigeben können.](https://docs.microsoft.com/windows/uwp/contacts-and-calendar/my-people-sharing#annotating-contacts)
3. Unterstützen Sie mehrere Instanzen der Anwendung, die zur gleichen Zeit ausgeführt werden.  Benutzer müssen mit einer vollständigen Version Ihrer Anwendung interagieren und diese auch für andere Personen freigeben können. Sie können diese in mehreren Freigabefenstern gleichzeitig verwenden. Um dies zu unterstützen, muss Ihre Anwendung mehrere Ansichten gleichzeitig ausführen können. Weitere Informationen hierzu finden Sie im Artikel ["Anzeigen mehrerer Ansichten für eine App"](https://docs.microsoft.com/windows/uwp/design/layout/show-multiple-views).

Wenn Sie dies getan haben, wird die Anwendung als Freigabeziel im Freigabefenster „Meine Kontakte” angezeigt, das auf zwei Arten gestartet werden kann:
1. Ein Kontakt wird über den Charm "Freigabe" ausgewählt.
2. Dateien werden über Drag & Drop auf einen Kontakt an die Taskleiste angeheftet.

## <a name="declaring-support-for-the-share-contract"></a>Deklarieren von Support für den Freigabe-Vertrag

Um Unterstützung für die Anwendung als Freigabeziel zu deklarieren, öffnen Sie die Anwendung zunächst in Visual Studio. Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf die Datei **Package.appxmanifest** und wählen Sie **Öffnen mit** aus. Wählen Sie aus dem Menü anschließend **XML (Text)-Editor**, und klicken Sie auf **OK**. Nehmen Sie danach folgenden Änderungen am Manifest vor.


**Vorher**
```xml
<Applications>
    <Application Id="MyApp"
      Executable="$targetnametoken$.exe"
      EntryPoint="My.App">
    </Application>
</Applications>
```

**Nachher**

```xml
<Applications>
    <Application Id="MyApp"
      Executable="$targetnametoken$.exe"
      EntryPoint="My.App">
        <Extensions>
            <uap:Extension Category="windows.shareTarget">
                <uap:ShareTarget Description="Share with MyApp">
                    <uap:SupportedFileTypes>
                        <uap:SupportsAnyFileType/>
                    </uap:SupportedFileTypes>
                    <uap:DataFormat>Text</uap:DataFormat>
                    <uap:DataFormat>Bitmap</uap:DataFormat>
                    <uap:DataFormat>Html</uap:DataFormat>
                    <uap:DataFormat>StorageItems</uap:DataFormat>
                    <uap:DataFormat>URI</uap:DataFormat>
                </uap:ShareTarget>
            </uap:Extension>
         </Extensions>
    </Application>
</Applications>
```

Dieser Code fügt Unterstützung für alle Dateien und Daten-Formate hinzu. Sie können allerdings auch angeben, welche Arten von Dateien und Datenformate unterstützt werden (weitere Details finden Sie unter [ShareTarget Klassendokumentation](https://docs.microsoft.com/uwp/schemas/appxpackage/appxmanifestschema/element-sharetarget)).

## <a name="annotating-contacts"></a>Kontakte mit Kommentaren versehen

Um zuzulassen, dass das Freigabefenster für „Meine Kontakte” Ihre Anwendung als Freigabeziel für Ihre Kontakte angibt, müssen Sie diese auf den Windows-Kontaktspeicher schreiben. Weitere Informationen zum Schreiben von Kontakten finden Sie unter [Beispiel für die Visitenkartenintegration](https://github.com/Microsoft/Windows-universal-samples/tree/6370138b150ca8a34ff86de376ab6408c5587f5d/Samples/ContactCardIntegration). 

Damit Ihre Anwendung als Freigabeziel für „Meine Kontakte” angezeigt werden kann, wenn Sie zu einen Kontakt freigeben, müssen sie eine Anmerkung darauf schreiben. Kommentare sind Datenteile von Ihrer Anwendung, die mit einem Kontakt verknüpft sind. Die Anmerkung muss die aktivierbare Klassen für die gewünschte Ansicht in seinem Element **ProviderProperties** und Support für den **Freigabe**-Vorgang deklarieren.

Sie können Kontakten jederzeit einen Kommentar hinzufügen, während die App ausgeführt wird, aber in der Regel sollten Sie Kontakte sofort kommentieren, wenn sie dem Windows-Kontaktspeicher hinzugefügt werden.

```Csharp
if (ApiInformation.IsApiContractPresent("Windows.Foundation.UniversalApiContract", 5))
{
    // Create a new contact annotation
    ContactAnnotation annotation = new ContactAnnotation();
    annotation.ContactId = myContact.Id;

    // Add appId and Share support to the annotation
    String appId = "MyApp_vqvv5s4y3scbg!App";
    annotation.ProviderProperties.Add("ContactShareAppID", appId);
    annotation.SupportedOperations = ContactAnnotationOperations::Share;

    // Save annotation to contact annotation list
    // Windows.ApplicationModel.Contacts.ContactAnnotationList 
    await contactAnnotationList.TrySaveAnnotationAsync(annotation);
}
```

Die “appId” ist der Paketfamilienname, gefolgt von ‘!’ und einer aktivierbaren Klassen-ID. Um Ihren Paketfamilienname zu suchen, öffnen Sie **Package.appxmanifest** mithilfe des Standard-Editors, und suchen Sie den Namen auf der Registerkarte "Verpacken". Hier entspricht "App" der aktivierbaren Klassen der Zielfreigabeansicht.

## <a name="running-as-a-my-people-share-target"></a>Ausführung als Freigabeziel für „Meine Kontakte”

Zum Ausführen der App müssen Sie die [OnShareTargetActivated](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Application#Windows_UI_Xaml_Application_OnShareTargetActivated_Windows_ApplicationModel_Activation_ShareTargetActivatedEventArgs_)-Methode in der App-Hauptklasse außer Kraft setzen, um die Zielfreigabeaktivierung behandeln zu können. Die [ShareTargetActivatedEventArgs.ShareOperation.Contacts](https://docs.microsoft.com/uwp/api/windows.applicationmodel.datatransfer.sharetarget.shareoperation#Properties)-Eigenschaft enthält die Kontakte, die freigegeben werden, oder leer bleiben, wenn es sich um einen standardmäßigen Freigabevorgang handelt (und nicht eine Freigabe für „Meine Kontakte”).

```Csharp
protected override void OnShareTargetActivated(ShareTargetActivatedEventArgs args)
{
    bool isPeopleShare = false;
    if (ApiInformation.IsApiContractPresent("Windows.Foundation.UniversalApiContract", 5))
    {
        // Make sure the current OS version includes the My People feature before
        // accessing the ShareOperation.Contacts property
        isPeopleShare = (args.ShareOperation.Contacts.Count > 0);
    }

    if (isPeopleShare)
    {
        // Show share UI for MyPeople contact(s)
    }
    else
    {
        // Show standard share UI for unpinned contacts
    }
}
```

## <a name="see-also"></a>Weitere Informationen:
+ [Unterstützung für meine Personen hinzufügen](my-people-support.md)
+ [Sharetarget-Klasse](https://docs.microsoft.com/uwp/schemas/appxpackage/appxmanifestschema/element-sharetarget)
+ [Beispiel für eine Kontaktkarten Integration](https://github.com/Microsoft/Windows-universal-samples/tree/6370138b150ca8a34ff86de376ab6408c5587f5d/Samples/ContactCardIntegration)
