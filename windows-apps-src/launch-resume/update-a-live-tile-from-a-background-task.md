---
title: Aktualisieren einer Live-Kachel über eine Hintergrundaufgabe
description: Verwenden Sie eine Hintergrundaufgabe, um die Live-Kachel Ihrer App mit neuen Inhalten zu aktualisieren.
Search.SourceType: Video
ms.assetid: 9237A5BD-F9DE-4B8C-B689-601201BA8B9A
ms.date: 01/11/2018
ms.topic: article
keywords: Windows 10, UWP, Hintergrundaufgabe
ms.localizationpriority: medium
ms.openlocfilehash: f2700f0e5ffa8c2d1c9f0500e967096763757cd9
ms.sourcegitcommit: 9aef3bc26a56b8d266b3089d509f79b119234b6f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/01/2020
ms.locfileid: "80538190"
---
# <a name="update-a-live-tile-from-a-background-task"></a>Aktualisieren einer Live-Kachel über eine Hintergrundaufgabe

**Wichtige APIs**

-   [**IBackgroundTask**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.IBackgroundTask)
-   [**BackgroundTaskBuilder**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskBuilder)

Verwenden Sie eine Hintergrundaufgabe, um die Live-Kachel Ihrer App mit neuen Inhalten zu aktualisieren.

Das folgende Video zeigt, wie Sie Ihren Apps Live-Kacheln hinzufügen:

<iframe src="https://channel9.msdn.com/Blogs/One-Dev-Minute/Updating-a-live-tile-from-a-background-task/player" width="720" height="405" allowFullScreen="true" frameBorder="0"></iframe>

## <a name="create-the-background-task-project"></a>Erstellen des Hintergrundaufgabenprojekts  

Zum Aktivieren einer Live-Kachel für Ihre APP fügen Sie Ihrer Projekt Mappe ein neues Windows-Runtime Komponenten Projekt hinzu. Dies ist eine separate Assembly, die vom BS im Hintergrund geladen und ausgeführt wird, wenn Benutzer die App installieren.

1.  Klicken Sie im Projektmappen-Explorer auf die Projektmappe, klicken Sie auf **Hinzufügen**, und klicken Sie dann auf **Neues Projekt**.
2.  Wählen Sie im Dialogfeld **Neues Projekt hinzufügen** im Abschnitt **Installiert**  Andere Sprachen  **Visual C# &gt; Windows Universal&gt; die Vorlage &gt;Komponente für Windows-Runtime** aus.
3.  Geben Sie dem Projekt den Namen „BackgroundTasks“, und klicken oder tippen Sie auf **OK**. In Microsoft Visual Studio wird das neue Projekt der Projektmappe hinzugefügt.
4.  Fügen Sie im Hauptprojekt einen Verweis auf das Projekt „BackgroundTasks“ hinzu.

## <a name="implement-the-background-task"></a>Implementieren der Hintergrundaufgabe


Implementieren Sie die [**IBackgroundTask**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.IBackgroundTask)-Schnittstelle, um eine Klasse zu erstellen, mit der die Live-Kachel Ihrer App aktualisiert wird. Ihre Hintergrundarbeit bezieht sich auf die Run-Methode. In diesem Fall erhält die Aufgabe einen Veröffentlichungsfeed für die MSDN-Blogs. Richten Sie eine Verzögerung ein, um das zu frühe Schließen der Aufgabe zu verhindern, während noch asynchroner Code ausgeführt wird.

1.  Benennen Sie im Projektmappen-Explorer die automatisch generierte Datei „Class1.cs“ in „BlogFeedBackgroundTask.cs“ um.
2.  Ersetzen Sie in „BlogFeedBackgroundTask.cs“ den automatisch generierten Code durch den Stub-Code für die **BlogFeedBackgroundTask**-Klasse.
3.  Fügen Sie in der Implementierung der Run-Methode Code für die Methoden **GetMSDNBlogFeed** und **UpdateTile** hinzu.

```cs
using System;
using System.Collections.Generic;
using System.Diagnostics;
using System.Linq;
using System.Text;
using System.Threading.Tasks;

// Added during quickstart
using Windows.ApplicationModel.Background;
using Windows.Data.Xml.Dom;
using Windows.UI.Notifications;
using Windows.Web.Syndication;

namespace BackgroundTasks
{
    public sealed class BlogFeedBackgroundTask  : IBackgroundTask
    {
        public async void Run( IBackgroundTaskInstance taskInstance )
        {
            // Get a deferral, to prevent the task from closing prematurely
            // while asynchronous code is still running.
            BackgroundTaskDeferral deferral = taskInstance.GetDeferral();

            // Download the feed.
            var feed = await GetMSDNBlogFeed();

            // Update the live tile with the feed items.
            UpdateTile( feed );

            // Inform the system that the task is finished.
            deferral.Complete();
        }

        private static async Task<SyndicationFeed> GetMSDNBlogFeed()
        {
            SyndicationFeed feed = null;

            try
            {
                // Create a syndication client that downloads the feed.  
                SyndicationClient client = new SyndicationClient();
                client.BypassCacheOnRetrieve = true;
                client.SetRequestHeader( customHeaderName, customHeaderValue );

                // Download the feed.
                feed = await client.RetrieveFeedAsync( new Uri( feedUrl ) );
            }
            catch( Exception ex )
            {
                Debug.WriteLine( ex.ToString() );
            }

            return feed;
        }

        private static void UpdateTile( SyndicationFeed feed )
        {
            // Create a tile update manager for the specified syndication feed.
            var updater = TileUpdateManager.CreateTileUpdaterForApplication();
            updater.EnableNotificationQueue( true );
            updater.Clear();

            // Keep track of the number feed items that get tile notifications.
            int itemCount = 0;

            // Create a tile notification for each feed item.
            foreach( var item in feed.Items )
            {
                XmlDocument tileXml = TileUpdateManager.GetTemplateContent( TileTemplateType.TileWide310x150Text03 );

                var title = item.Title;
                string titleText = title.Text == null ? String.Empty : title.Text;
                tileXml.GetElementsByTagName( textElementName )[0].InnerText = titleText;

                // Create a new tile notification.
                updater.Update( new TileNotification( tileXml ) );

                // Don't create more than 5 notifications.
                if( itemCount++ > 5 ) break;
            }
        }

        // Although most HTTP servers do not require User-Agent header, others will reject the request or return
        // a different response if this header is missing. Use SetRequestHeader() to add custom headers.
        static string customHeaderName = "User-Agent";
        static string customHeaderValue = "Mozilla/5.0 (compatible; MSIE 10.0; Windows NT 6.2; WOW64; Trident/6.0)";

        static string textElementName = "text";
        static string feedUrl = @"http://blogs.msdn.com/b/MainFeed.aspx?Type=BlogsOnly";
    }
}
```

## <a name="set-up-the-package-manifest"></a>Einrichten des Paketmanifests


Öffnen Sie das Paketmanifest, um es einzurichten, und fügen Sie eine Deklaration einer neuen Hintergrundaufgabe hinzu. Legen Sie den Einstiegspunkt für die Aufgabe auf den Klassennamen fest, und fügen Sie auch den Namespace ein.

1.  Öffnen Sie im Projektmappen-Explorer die Datei „Package.appxmanifest“.
2.  Klicken oder tippen Sie auf die Registerkarte **Deklarationen**.
3.  Wählen Sie unter **Verfügbare Deklarationen**die Option **BackgroundTasks** aus, und klicken Sie auf **Hinzufügen**. In Visual Studio wird **BackgroundTasks** unter **Unterstützte Deklarationen** hinzugefügt.
4.  Stellen Sie unter **Unterstützte Aufgabentypen** sicher, dass **Timer** aktiviert ist.
5.  Legen Sie unter **App-Einstellungen** den Einstiegspunkt auf **BackgroundTasks.BlogFeedBackgroundTask** fest.
6.  Klicken oder tippen Sie auf die Registerkarte **Anwendungsbenutzeroberfläche**.
7.  Legen Sie die Option **Benachrichtigungen bei gesperrtem Bildschirm** auf **Text für Infoanzeiger und Kachel**fest.
8.  Legen Sie im Feld **Infoanzeigerlogo** einen Pfad zu einem Symbol mit einer Größe von 24 x 24 Pixel fest.
    **Wichtig**  dieses Symbol darf nur monochrome und transparente Pixel verwenden.
9.  Legen Sie im Feld **Kleines Logo** einen Pfad zu einem Symbol der Größe 30 x 30 Pixel fest.
10. Legen Sie im Feld **Breites Logo** einen Pfad zu einem Symbol mit einer Größe von 310 x 150 Pixel fest.

## <a name="register-the-background-task"></a>Registrieren der Hintergrundaufgabe


Erstellen Sie zum Registrieren Ihrer Aufgabe ein [**BackgroundTaskBuilder**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskBuilder)-Objekt.

> **Hinweis**  ab Windows 8.1 werden die Registrierungs Parameter für die Hintergrundaufgabe zum Zeitpunkt der Registrierung überprüft. Bei ungültigen Registrierungsparametern wird ein Fehler zurückgegeben. Ihre App muss Szenarien mit nicht erfolgreicher Registrierung von Hintergrundaufgaben problemlos verarbeiten können. Verwenden Sie beispielsweise eine Bedingungsanweisung, um die App auf Registrierungsfehler zu prüfen, und wiederholen Sie die nicht erfolgreiche Registrierung mit anderen Parameterwerten.
 

Fügen Sie auf der Hauptseite der App die **RegisterBackgroundTask**-Methode hinzu, und rufen Sie sie im **OnNavigatedTo**-Ereignishandler auf.

```cs
using System;
using System.Collections.Generic;
using System.IO;
using System.Linq;
using System.Threading.Tasks;
using Windows.ApplicationModel.Background;
using Windows.Data.Xml.Dom;
using Windows.Foundation;
using Windows.Foundation.Collections;
using Windows.UI.Xaml;
using Windows.UI.Xaml.Controls;
using Windows.UI.Xaml.Controls.Primitives;
using Windows.UI.Xaml.Data;
using Windows.UI.Xaml.Input;
using Windows.UI.Xaml.Media;
using Windows.UI.Xaml.Navigation;
using Windows.Web.Syndication;

// The Blank Page item template is documented at https://go.microsoft.com/fwlink/p/?LinkID=234238

namespace ContosoApp
{
    /// <summary>
    /// An empty page that can be used on its own or navigated to within a Frame.
    /// </summary>
    public sealed partial class MainPage : Page
    {
        public MainPage()
        {
            this.InitializeComponent();
        }

        /// <summary>
        /// Invoked when this page is about to be displayed in a Frame.
        /// </summary>
        /// <param name="e">Event data that describes how this page was reached.  The Parameter
        /// property is typically used to configure the page.</param>
        protected override void OnNavigatedTo( NavigationEventArgs e )
        {
            this.RegisterBackgroundTask();
        }


        private async void RegisterBackgroundTask()
        {
            var backgroundAccessStatus = await BackgroundExecutionManager.RequestAccessAsync();
            if( backgroundAccessStatus == BackgroundAccessStatus.AllowedSubjectToSystemPolicy ||
                backgroundAccessStatus == BackgroundAccessStatus.AlwaysAllowed )
            {
                foreach( var task in BackgroundTaskRegistration.AllTasks )
                {
                    if( task.Value.Name == taskName )
                    {
                        task.Value.Unregister( true );
                    }
                }

                BackgroundTaskBuilder taskBuilder = new BackgroundTaskBuilder();
                taskBuilder.Name = taskName;
                taskBuilder.TaskEntryPoint = taskEntryPoint;
                taskBuilder.SetTrigger( new TimeTrigger( 15, false ) );
                var registration = taskBuilder.Register();
            }
        }

        private const string taskName = "BlogFeedBackgroundTask";
        private const string taskEntryPoint = "BackgroundTasks.BlogFeedBackgroundTask";
    }
}
```

## <a name="debug-the-background-task"></a>Debuggen der Hintergrundaufgabe


Legen Sie zum Debuggen der Hintergrundaufgabe in der Run-Methode der Aufgabe einen Haltepunkt fest. Wählen Sie die Hintergrundaufgabe auf der Symbolleiste **Debugspeicherort** aus. Das System ruft dann sofort die Run-Methode auf.

1.  Legen Sie in der Run-Methode der Aufgabe einen Haltepunkt fest.
2.  Drücken Sie F5 oder tippen Sie auf **Debuggen &gt; Debugging starten**, um die App bereitzustellen und auszuführen.
3.  Wechseln Sie nach dem Start der App zurück zu Visual Studio.
4.  Stellen Sie sicher, dass die Symbolleiste **Debugspeicherort** sichtbar ist. Sie befindet sich im Menü **Ansicht &gt; Symbolleisten**.
5.  Klicken Sie in der Symbolleiste **Debugspeicherort** auf die Dropdownliste **Anhalten**, und wählen Sie **BlogFeedBackgroundTask**aus.
6.  Visual Studio hält die Ausführung am Haltepunkt an.
7.  Drücken Sie F5 oder tippen Sie auf **Debuggen &gt; Fortsetzen**, um die Ausführung der App fortzusetzen.
8.  Drücken Sie UMSCHALT+F5 oder tippen Sie auf **Debuggen &gt; Debugging** beenden, um das Debuggen zu beenden.
9.  Kehren Sie zur Kachel der App auf dem Startbildschirm zurück. Nach einigen Sekunden werden in der Kachel der App Kachelbenachrichtigungen angezeigt.

## <a name="related-topics"></a>Verwandte Themen


* [**BackgroundTaskBuilder**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskBuilder)
* [**Tileupdatemanager**](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.TileUpdateManager)
* [**Tilenotifizierung**](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.TileNotification)
* [Unterstützen Ihrer App mit Hintergrundaufgaben](support-your-app-with-background-tasks.md)
* [Richtlinien und Prüfliste für Kacheln und Ausweise](https://docs.microsoft.com/windows/uwp/controls-and-patterns/tiles-and-notifications-creating-tiles)

 

 
