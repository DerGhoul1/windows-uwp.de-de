---
author: QuinnRadich
title: Neues in Windows10 für Entwickler, Tools und Features
description: Windows 10, build 17763 und neue Entwicklertools stellen Tools, Features und Umgebungen bereit, indem Sie die universelle Windows-Plattform.
keywords: Was ist neu, Neuigkeiten, Update, Updates, features, neu, Windows 10, neueste, Entwickler, 17763
ms.author: quradic
ms.date: 10/03/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.localizationpriority: medium
ms.openlocfilehash: 19f2d29d94759a4b8fd273c8fdc0cdf5c93311de
ms.sourcegitcommit: 310a4555fedd4246188a98b31f6c094abb33ec60
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/19/2018
ms.locfileid: "5127881"
---
# <a name="whats-new-in-windows-10-for-developers-build-17763"></a>Was ist neu in Windows 10 für Entwickler, Build 17763

Windows 10, build 17763 (auch bekannt als die Oktober 2018 Update oder Version 1809), in Kombination mit Visual Studio 2017 und dem aktualisierten SDK, bereitstellen, Tools, Features und Umgebungen für die Entwicklung eindrucksvoller apps für universelle Windows-Plattform. Nach der [Installation der Tools und des SDKs](http://go.microsoft.com/fwlink/?LinkId=821431) unter Windows10 können Sie entweder [eine neue universelle Windows-App erstellen](../get-started/create-uwp-apps.md) oder sich mit der Verwendung von [vorhandenem App-Code unter Windows](../porting/index.md) vertraut machen.

Dies ist eine Sammlung von neuen und verbesserten Features und Richtlinien, die in dieser Version für Windows-Entwickler interessant sind. Eine vollständige Liste mit neuen Namespaces, die Windows SDK hinzugefügt wurden finden Sie unter der [Windows 10, build 17763 API-Änderungen](windows-10-build-17763-api-diff.md). Weitere Informationen zu den Highlights von Windows10 finden Sie unter [Die Highlights in Windows10](http://go.microsoft.com/fwlink/?LinkId=823181). Darüber hinaus finden Sie unter [Windows Developer Platform-Features](https://developer.microsoft.com/windows/platform/features) eine grobe Übersicht über die früheren und zukünftigen neuen Features der Windows-Plattform.

## <a name="design--ui"></a>Design und UI

Feature | Beschreibung
 :------ | :------
App-Symbole und Logos | Die [Seite "app-Symbole und Logos"](../design/style/app-icons-and-logos.md) wurde wurde neu geschrieben, und jetzt zeigt die neuesten Visual Studio-Symbol-Tools und enthält Informationen zum Hinzufügen von Bildern zu dem Eintrag Ihrer app im Microsoft Store.
Design-Startseite | Das [Design Zielseite aktualisiert](https://developer.microsoft.com/windows/apps/design) wurde auf einen Blick Überblick über UWP Design Bereiche und Informationen über die neusten Fluent Design.
Fluent Design-Steuerelemente | Die folgenden neuen UI-Steuerelemente haben zur Verbesserung der Fluent Design-Systems und die Apparence Ihrer Apps hinzugefügt wurde: </br> * [CommandBarFlyout](../design/controls-and-patterns/command-bar-flyout.md) können Sie die allgemeinen Schritte für Benutzer im Kontext eines Elements auf die UI-Canvas angezeigt. </br> * [DropDownButton](../design/controls-and-patterns/buttons.md#create-a-drop-down-button), [SplitButton](../design/controls-and-patterns/buttons.md#create-a-split-button)und [ToggleSplitButton](../design/controls-and-patterns/buttons.md#create-a-toggle-split-button) bieten Schaltflächensteuerelementen mit speziellen Features zur Verbesserung der Benutzeroberfläche Ihrer app. </br> * [Menüleiste](../design/controls-and-patterns/menus.md) zeigt eine Reihe von mehreren Menüs der obersten Ebene in horizontalen Zeilen. </br> * [NavigationView](../design/controls-and-patterns/navigationview.md) unterstützt jetzt oberen Navigationsleiste für Fälle, in denen Ihre app verfügt über eine kleinere Zahl von Navigationsoptionen und erfordert mehr Platz für Inhalte. </br> * [Strukturansicht](../design/controls-and-patterns/tree-view.md) wurde verbessert, um unterstützen die Datenbindung, Elementvorlagen, und Drag & Drop.
Fluent Design Update | Visuelle Updates und geringfügigen Änderungen wurden die folgenden Fluent Design-Seiten vorgenommen: </br> * [Ausrichtung, Abstände, Ränder](../design/layout/alignment-margin-padding.md) </br> * [Farbe](../design/style/color.md) </br> * [Fluent Design für Windows-apps](../design/fluent-design-system/index.md) </br> * [Einführung in das app-design](../design/basics/design-and-ui-intro.md) </br> * [Navigationsgrundlagen](../design/basics/navigation-basics.md) </br> * [Reaktionsfähige Designtechniken](../design/layout/responsive-design.md) </br> * [Bildschirmgrößen und Haltepunkte](../design/layout/screen-sizes-and-breakpoints-for-responsive-design.md) </br> * [Übersicht über die Stil](../design/style/index.md) </br> * [Schreibstil](../design/style/writing-style.md) </br> Darüber hinaus haben wir die folgenden Seiten mit neuen Informationen zu ihren Inhaltsbereiche umgeschrieben: </br> * [Symbole](../design/style/icons.md) enthält jetzt praktische Empfehlungen für Symbole und dass diese geklickt werden kann. </br> * [Typografie](../design/style/typography.md) konsolidiert Informationen aus ähnlichen Artikeln Sie alles an einem Ort mit aktualisierten Hilfestellungen und Illustrationen einfügen.
Eingabe via anvisieren und Interaktionen | [Interaktionen über anvisieren](../design/input/gaze-interactions.md) ermöglichen Ihrer app zum Nachverfolgen des Benutzers über anvisieren, Aufmerksamkeit und Vorhandensein basierend auf der Position und Bewegung seiner Augen. Dieses Feature kann als unterstützende Technologie verwendet werden, und bietet Möglichkeiten für Spiele und andere interaktiven Szenarien, in denen herkömmliche Eingabegeräte nicht verfügbar sind.
Handschriftansicht | [HandwritingView](../design/controls-and-patterns/text-handwriting-view.md) ist die neue input Freihand-Oberfläche für TextBox und RichEditBox. Benutzer können ein Text-Steuerelement mit ihren Stift das Steuerelement in einer Schreiboberfläche erweitern tippen. Dieser Leitfaden wird erläutert, wie verwalten und die HandwritingView in Ihrer Anwendung anpassen.
Bewegung in der Fluent Design | Die Verwendung von Bewegung in der Fluent Design-Systems wird weiterentwickelt, die auf die Grundlagen der Timing, geschwindigkeitsverlauf, direktionalität und Schwerkraft erstellt. Anwenden von diesen Grundlagen hilft Führung des Benutzers über Ihre app und Spiegeln der natürlichen Welt mit seiner digitalen Erfahrung verbunden. Weitere Informationen finden Sie in folgenden Artikeln: </br> * [Übersicht über die Bewegungen](../design/motion/index.md) wurde aktualisiert, um diese Grundlagen widerspiegeln. </br> * [In der Praxis Bewegung](../design/motion/motion-in-practice.md) enthält Beispiele zur Verwendung von diesen Grundlagen innerhalb Ihrer app anwenden. Es enthält auch Informationen zu impliziten Animationen bieten die einfache Interpolation zwischen alte und neue Wert, wenn ein XAML-Element-Eigenschaft geändert wird. </br> * [Direktionalität und Schwerkraft](../design/motion/directionality-and-gravity.md) festigt das mentale Benutzermodell Ihrer App. </br> * [Timing und geschwindigkeitsverlauf](../design/motion/timing-and-easing.md) wird die Bewegung in Ihrer app Realismus hinzugefügt. </br> * [XAML-Eigenschaftenanimationen](../design/motion/xaml-property-animations.md) können Sie Eigenschaften für ein XAML-Element direkt zu animieren, ohne Interaktion mit dem zugrunde liegenden Composition Visual.
Seitenübergänge | [Seitenübergänge](../design/motion/page-transitions.md) , wenn Benutzer zwischen Seiten in einer app navigieren. Sie können Benutzern das Verständnis, wo sie in der Navigationshierarchie sind, und geben Sie Feedback über die Beziehung zwischen Seiten.
Textskalierung | Der neue [Text Skalierung Leitfaden](../design/input/text-scaling.md) wird erläutert, wie aktualisieren Sie Ihre Anwendung, um den neuen Text-Skalierungen aufzunehmen, die die Möglichkeit für Benutzer relativen Schriftgrad über das Betriebssystem und die einzelnen Anwendungen ändern. Anstelle eine Bildschirmlupe-app (die in der Regel nur alles in einem Bereich des Bildschirms vergrößert und führt eine eigene Probleme hinsichtlich der Verwendbarkeit) verwenden, ändern Auflösung oder hierauf basieren DPI-Skalierung (die alles basierend auf der Anzeige und typische Anzeige ändert Abstand), Benutzer können schnell zugreifen, eine Einstellung, um nur Text, angefangen bei 100 % (die Standardgröße) Größe bis zu 225 %.
Toolkits | [Adobe XD und Adobe Illustrator-Toolkits](../design/downloads/index.md) wurde mit neuen Funktionen aktualisiert. Diese Design-Toolkits bieten Steuerelemente und Layoutvorlagen für das Design von UWP-apps.
UI-Befehle | Updates für die [UWP Steuerungselemente Infrastruktur](../design/basics/commanding-basics.md) umfassen eine bessere Kapselung eines Command-Objekts (Verhalten, Bezeichnung, Symbol, Zugriffstasten, Zugriffstaste und Beschreibung) und eine Reihe von häufig verwendete Befehle, z. B. Ausschneiden, kopieren, einfügen, beenden, usw.., die entfällt die Notwendigkeit können Sie diese Eigenschaften manuell festlegen. </br> Die neue [XamlUICommand](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.input.xamluicommand) Klasse bietet eine Basisklasse für deveining das Verhalten des Befehls eine interaktive UI-Elements, das eine Aktion beim Aufruf ausführt. Dies ist der übergeordneten Klasse für [StandardUICommand](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.input.standarduicommand), das einen Satz von standard-Plattform-Befehle mit vordefinierten Eigenschaften verfügbar macht. 
Windows-UI-Bibliothek | Der [Windows-UI-Bibliothek](https://aka.ms/winui-docs) ist ein Satz von NuGet-Pakete, die Steuerelemente und andere Elemente der Benutzeroberfläche für UWP-apps bereitstellen. Diese Pakete sind auch mit früheren Versionen von Windows 10 kompatibel, damit Ihre app funktioniert, auch wenn Ihre Benutzer die neueste Version des Betriebssystems besitzen. </br> Weitere Informationen zu in der Windows-UI-Bibliothek Neuigkeiten finden Sie unter [Diese Liste mit API-Namespaces in das NuGet-Paket enthalten.](https://docs.microsoft.com/uwp/api/overview/winui/)

## <a name="develop-windows-apps"></a>Entwickeln von Windows-Apps

Feature | Beschreibung
 :------ | :------
Strichcodescanner | Die [Strichcodescanner](https://docs.microsoft.com/windows/uwp/devices-sensors/pos-barcodescanner) -Dokumentation wurde neu angeordnet und durch weitere Details und Codeausschnitte verbessert. Wir haben auch ein neues Thema hinzugefügt [Abrufen und Verstehen von Strichcode-Daten](https://docs.microsoft.com/windows/uwp/devices-sensors/pos-barcodescanner-scan-data), was erklärt, Informationen zum Erhalt und Arbeiten mit Daten aus einem Strichcodescanner.
C++/WinRT | [C++ / WinRT](https://aka.ms/cppwinrt) enthält viele neue Features, Änderungen und Updates für diese Version. Es gibt neue Funktionen und unterstützt Sie bei der Implementierung Ihrer eigenen [Sammlungseigenschaften und Sammlungstypen](/windows/uwp/cpp-and-winrt-apis/collections)Basisklassen. und Sie können jetzt die [{Binding}](/windows/uwp/xaml-platform/binding-markup-extension) XAML-Markuperweiterung mit Ihrer C++ / WinRT-Runtime-Klassen (Codebeispiele finden Sie unter " [Übersicht über Datenbindung](/windows/uwp/data-binding/data-binding-quickstart)"). Eine vollständige Beschreibung aller neuen und geänderten in dieser Version finden Sie unter [Neuigkeiten in C++ / WinRT](../cpp-and-winrt-apis/news.md).</br></br>Andere neue C++ / WinRT-Inhalt gehören: [benutzerdefinierte XAML-Steuerelemente](/windows/uwp/cpp-and-winrt-apis/xaml-cust-ctrl). ["Author" COM-Komponenten](/windows/uwp/cpp-and-winrt-apis/author-coclasses); [Wertekategorien](/windows/uwp/cpp-and-winrt-apis/cpp-value-categories); und [starke und schwache Referenzen](../cpp-and-winrt-apis/weak-references.md).
C++ / WinRT-Codebeispiele | Wir haben hinzugefügt 250 C++ / WinRT-Codebeispiele Themen in unserer Dokumentation begleitende vorhandenen C++ / CX-Code-Beispiele.
Beitragenden Anleitungen | Wir haben [unsere Beitragenden Anleitung](https://github.com/MicrosoftDocs/windows-uwp/blob/docs/CONTRIBUTING.md) für unsere Dokumentation UWP aktualisiert. Diese neue Anleitungen verdeutlicht Workflow und Erwartungen an externe Beiträge unserer Dokumente.
DirectX-Grafiken Infastructure (DXGI) | Neue Dokumentation für fehlende DXGI-APIs hinzugefügt wurde, und wir haben einen Artikel über bewährte Methoden bereitgestellt, wenn unter Windows 10 darstellen. </br> * [Verwenden Sie für eine optimale Leistung DXGI Flip-Modell](https://docs.microsoft.com/windows/desktop/direct3ddxgi/for-best-performance--use-dxgi-flip-model): enthält Informationen zur Verwendung von Leistung und Effizienz bei der Präsentation Stapel auf moderne Windows-Versionen zu maximieren. </br> * [IDXGIOutput6::CheckHardwareCompositionSupport Methode](https://docs.microsoft.com/windows/desktop/api/dxgi1_6/nf-dxgi1_6-idxgioutput6-checkhardwarecompositionsupport): teilt einer Anwendung, dass Hardware Streckung unterstützt wird. </br> * [DXGI_HARDWARE_COMPOSITION_SUPPORT_FLAGS Enumeration](https://docs.microsoft.com/windows/desktop/api/dxgi1_6/ne-dxgi1_6-dxgi_hardware_composition_support_flags): wird beschrieben, welche Ebenen der Hardware Komposition unterstützt werden.
Erste Schritte | Unsere [Erste Schritte](../get-started/index.md) Inhalte verfügt über neue Themen, Bereitstellen von Informationen und Richtlinien für wie Entwickler auf Windows 10 die folgenden allgemeinen Aufgaben ausführen können mit revitalized wurde: </br> * [Erstellen eines Formulars](../get-started/construct-form-learning-track.md) </br> * [Anzeigen von Kunden in einer Liste](../get-started/display-customers-in-list-learning-track.md) </br> * [Speichern und Laden von Einstellungen](../get-started/settings-learning-track.md) </br> * [Arbeiten mit Dateien](../get-started/fileio-learning-track.md)
Karten-Stylesheet-Editor | Verwenden Sie die neue [Karte Formatvorlagen-Editor](https://www.microsoft.com/p/map-style-sheet-editor/9nbhtcjt72ft?rtc=1#activetab=pivot:overviewtab) -Anwendung interaktiv die Darstellung von Karten anpassen, die Sie für Ihre Anwendung hinzufügen.
Hier erfahren Sie, Microsoft | Die neue [Microsoft-Learn-Website](https://www.microsoft.com/learning/default.aspx) enthält neue praktische Learning und Schulungen Chancen für Microsoft-Entwickler. Derzeit bietet Microsoft Learn Training und Zertifizierungen für Microsoft 365, Microsoft Azure, Office 365 und Windows Server.
Windows-Editor | [Editor aktualisiert wurde](http://aka.ms/ant-man), Zoomen hinzufügen, direktionales suchen und ersetzen und Unterstützung für Unix/Linux (BF) und Mac (CR) Zeilenenden.
Projekt Rome | [Projekt "ROME"](https://docs.microsoft.com/windows/project-rome/) bietet eine einheitliche programmiererfahrung jetzt auf allen unterstützten Plattformen und SDKs. </br>  Neue [Microsoft Graph Benachrichtigungen](https://developer.microsoft.com/graph/docs/concepts/notifications-concept-overview) verwenden Projekt "ROME" eine Plattform Personen-orientierte, plattformübergreifende Benachrichtigungen für Ihre app angeboten werden soll.
Bildschirm snipping | Neue [URI-Schemas](../launch-resume/launch-screen-snipping.md) können Ihre app programmgesteuert öffnen Sie einen neuen Ausschnitt oder der Ausschnitt & Skizze app mit einem bestimmten Image für Anmerkung zu starten.
UWP-Steuerelemente in desktop-Apps | Windows 10 können jetzt UWP-Steuerelemente in WPF-, Windows Forms und C++ Win32-desktopanwendungen verwenden. Dies bedeutet, dass Sie das Erscheinungsbild und Funktionalität Ihrer vorhandenen desktop-Anwendungen mit den neuesten Windows 10-UI-Funktionen, die nur über UWP-Steuerelemente, z. B. Windows Ink und Steuerelemente, die das Fluent Design-System unterstützen verfügbar sind verbessern können. Dieses Feature ist *XAML-Inseln*bezeichnet. </br> Wir bieten verschiedene Möglichkeiten zur XAML-Inseln in Ihren Anwendungen, abhängig von der Anwendungsplattform verwenden, die Sie verwenden werden. WPF- oder Windows Forms-Anwendung können eine Reihe von Steuerelementen im [Windows Community Toolkit](https://docs.microsoft.com/windows/uwpcommunitytoolkit/) verwenden, die ein Designer-orientierte Entwicklung Benutzererlebnis zu bieten. C++ Win32-Anwendungen müssen der *UWP-XAML hosting-API* im [Windows.UI.Xaml.Hosting](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting) -Namespace verwenden. Weitere Informationen finden Sie in [UWP-Steuerelemente in desktop-Apps](../xaml-platform/xaml-host-controls.md). </br> **Hinweis:** Die APIs und Steuerelemente, mit denen XAML-Inseln sind als Entwicklervorschau derzeit verfügbar ist. Obwohl wir Sie diese in Ihrem eigenen Code Prototyp ausprobieren können, jetzt dazu ermutigen, empfehlen wir nicht, dass Sie sie zu diesem Zeitpunkt in Produktionscode verwenden.
Windows Machine Learning | [Windows Machine Learning](https://docs.microsoft.com/windows/ai/) hat jetzt offiziell gestartet Features wie schneller Bewertung und Unterstützung für montierte Machine Learning-Modelle bereitstellen. Um Entwickler zu unterstützen, die sie in ihren Anwendungen integrieren möchten, haben wir eine neue Dokumentation Website mit mehrere neue und aktualisierte Ressourcen erstellt: </br> * [Lernprogramm: Erstellen einer Windows Machine Learning-Desktop-Anwendung (C++)](https://docs.microsoft.com/windows/ai/get-started-desktop): in diesem Lernprogramm zeigt, wie Sie eine einfache Windows ML-Anwendung für den Desktop erstellt. </br> * [Lernprogramm: Erstellen einer Windows Machine Learning-UWP-Anwendung (c#)](https://docs.microsoft.com/windows/ai/get-started-uwp): Erstellen Ihrer ersten UWP-Anwendung mit Windows ML in diesem ausführlichen Lernprogramm. </br> * [Windows.AI.MachineLearning Namespace](https://docs.microsoft.com/uwp/api/windows.ai.machinelearning): die API-Referenz für die neueste Version von Windows 10 SDK wurde aktualisiert und Entwickler können jetzt verwenden Sie diese API für Win32- oder UWP-Anwendung.
Windows Mixed Reality | Entwickler können nun die Anforderung hardwaregeschützten Hintergrundpuffer Texturen, wenn von der Anzeigehardware unterstützt und Anwendungen mit Hardware-geschützten Inhalte aus Quellen wie PlayReady zulassen. Unterstützung der Hardware-Schutz und die Einstellung ist verfügbar, für die primäre Ebene über neue Eigenschaften des [Windows.Graphics.Holographic.HolographicCamera](https://docs.microsoft.com/uwp/api/windows.graphics.holographic.holographiccamera)und Quad-Ebenen über [ Windows.Graphics.Holographic.HolographicQuadLayerUpdateParameters](https://docs.microsoft.com/uwp/api/windows.graphics.holographic.holographicquadlayerupdateparameters).

## <a name="iot-core"></a>IoT Core

Feature | Beschreibung:---| :---AssignedAccessSettings | Die [AssignedAccessSettings Klasse](https://docs.microsoft.com/uwp/api/windows.system.userprofile.assignedaccesssettings) ermöglicht Aufrufe für die verschiedenen Methoden und Eigenschaften, die des Benutzers den Zugriff auf zugewiesen Zugriff Einstellungen für ein bestimmtes Gerät.
Standard-App-Übersicht | Die [Windows 10 IoT Core Standard-App](https://docs.microsoft.com/windows/iot-core/develop-your-app/iotcoredefaultapp) wurde mit neuen Features und Funktionen, z. B. Wetter, Freihandeingabe, aktualisiert und audio.
Dashboard | Das [Windows 10 Iot Core-Dashboard](https://docs.microsoft.com/windows/iot-core/tutorials/quickstarter/devicesetup) können jetzt Entwickler mittels eines Dragonboard 410 C oder NXP flash benutzerdefinierte FFUs auf ihrem Gerät.
Bildschirmtastatur | Die [Bildschirmtastatur für IoT-Geräte](https://docs.microsoft.com/windows/iot-core/develop-your-app/onscreenkeyboard) wird jetzt die gleichen Touch-Bildschirmtastatur Komponenten als die Desktopversion von Windows verwendet. Dadurch können Features wie Diktiermodus, IME-Unterstützung und einen vollständigen Satz von eingabeumfängen.
Titelleisten für Anmeldung Dialogfelder | Windows 10 IoT Core bietet nun die Möglichkeit, [Titelleisten für Systemdialogfelder](https://docs.microsoft.com/windows/iot-core/develop-your-app/signindialogtitlebars)konfigurieren.
Auf Toucheingabe reaktivieren | [Wake-on auf Toucheingabe](https://docs.microsoft.com/windows/iot-core/learn-about-hardware/wakeontouch) ermöglicht Ihrer Gerätebildschirms So deaktivieren Sie den zwar nicht verwendeter Speicher, während Sie schnell einschalten, wenn ein Benutzer den Bildschirm berührt. Windows.System.Update | Der neue [Windows.System.Update-Namespace](https://docs.microsoft.com/uwp/api/windows.system.update) ermöglicht die interaktive Steuerung des System-Updates. Dieser Namespace ist nur für Windows 10 IoT Core verfügbar.

## <a name="web-development"></a>Webentwicklung

Feature | Beschreibung:---| :---EdgeHTML 18 | Die Windows aktualisieren 10 Oktober 2018 Schiffen mit [EdgeHTML 18](https://docs.microsoft.com/microsoft-edge/dev-guide), das neueste Update für Microsoft Edge-Browser und das JavaScript-Modul für UWP-apps. EdgeHTML 18 sorgt dafür, dass modernisierten und erweiterte Unterstützung für die Web-Authentication-API, neue WebView-Steuerelement Features und vieles mehr! Auf der Seite Tools sorgt dafür, dass EdgeHTML 18 neue WebDriver Funktionen und automatische Updates und Verbesserungen für die Edge DevTools und Edge DevTools-Protokoll. Entdecken Sie [Neuigkeiten bei EdgeHTML 18](https://docs.microsoft.com/microsoft-edge/dev-guide) und [DevTools in der aktuellen Windows 10 aktualisieren (EdgeHTML 18)](https://docs.microsoft.com/microsoft-edge/devtools-guide/whats-new) für alle Details.
Progressive Web-Apps | (Windows 10-JavaScript-apps Web apps in einem *WWAHost.exe* Prozess ausgeführt) unterstützen jetzt optionalen [hintergrundskript pro Anwendung](https://docs.microsoft.com/en-us/microsoft-edge/dev-guide#progressive-web-apps) , die gestartet wird, bevor alle Ansichten aktiviert werden und wird für die Dauer des Prozesses ausgeführt. Mit dieser Option können Sie überwachen und Navigationsvorgänge ändern, Zustand über Navigationsvorgänge nachverfolgt, Navigation Fehler zu überwachen und Code ausführen, bevor Ansichten aktiviert werden. Wenn angegeben die [`StartPage`](https://docs.microsoft.com/en-us/uwp/schemas/appxpackage/appxmanifestschema2010-v2/element-application) in Ihrer [app-manifest](https://docs.microsoft.com/en-us/uwp/schemas/appxpackage/appx-package-manifest), einzelnen app Ansichten (Windows) an das Skript als Instanzen des neuen verfügbar gemacht [`WebUIView`](https://docs.microsoft.com/en-us/uwp/api/windows.ui.webui.webuiview) -Klasse, die gleichen Ereignisse, Eigenschaften und Methoden als eine allgemeine (Win32) [WebView](https://docs.microsoft.com/en-us/uwp/api/windows.web.ui.iwebviewcontrol)bereitstellen.
Web-API-Erweiterungen | Eine Liste der [veralteten Microsoft-API-Erweiterungen](https://developer.mozilla.org/docs/Web/API/Microsoft_API_extensions) wurde in der Dokumentation Mozilla Developer Network browserübergreifende Webentwicklung hinzugefügt. Diese API-Erweiterungen sind spezifisch für Internet Explorer oder Microsoft Edge und ergänzt vorhandene Informationen zur Kompatibilität und Browser-Unterstützung in der MDN Web-Dokumentation. Ältere Microsoft [CSS-Erweiterungen](https://developer.mozilla.org/docs/Web/CSS/Microsoft_Extensions) und [JavaScript-Erweiterungen](https://developer.mozilla.org/docs/Web/JavaScript/Microsoft_JavaScript_extensions) sind ebenfalls verfügbar, und finden Sie umfassende Web-API-Informationen aus MDN ermittlungsfunktionen direkt in [Visual Studio Code.](https://code.visualstudio.com/updates/v1_25#_new-css-pseudo-selectors-and-pseudo-elements-from-mdn)
WebVR | Wir haben die wichtigen Updates im [WebVR-Entwicklerhandbuch](https://docs.microsoft.com/microsoft-edge/webvr/), einschließlich der Seite "Home" völlig neu entworfen und Umstrukturierung von Inhaltsverzeichnis gemacht. Wir haben ebenfalls mehrere neue Themen, einschließlich entwickelt: </br> * [Was ist WebVR?](https://docs.microsoft.com/microsoft-edge/webvr/what-is-webvr) Wird erläutert, was WebVR ist, warum Sie sie verwenden sollten und für den Einstieg in die Entwicklung für sie. </br> * [WebVR in Progressive Web-Apps](https://docs.microsoft.com/microsoft-edge/webvr/webvr-in-pwas): erfahren Sie, wie Sie WebVR in eine Progressive Web-App (PWA) hinzufügen. </br> * [WebVR in der Webansicht](https://docs.microsoft.com/microsoft-edge/webvr/webvr-in-webview): enthält Informationen zum Hinzufügen von WebVR zu einem WebView-Steuerelement in einer Windows 10-Anwendung. </br> * [WebVR Demos](https://docs.microsoft.com/microsoft-edge/webvr/demos): sehen Sie sich einige WebVR Demos mit Microsoft Edge und ein immersives Windows Mixed Reality-Kopfhörer.

## <a name="publish--monetize-windows-apps"></a>Veröffentlichen und Monetarisieren von Windows-Apps

Feature | Beschreibung
 :------ | :------
MSIX | [MSIX](https://docs.microsoft.com/windows/msix/overview) ist das neue Windows-app-Paketformat, das eine moderne Packaging-Erlebnis für alle Windows-apps bietet. Open-Source-MSIX-Format behält die Funktionalität der vorhandenen Paketen beim Aktivieren der modernen Bereitstellungsfunktionen.
MSIX-Verpackungstool | Das neue [MSIX-Verpackungstool](https://docs.microsoft.com/windows/msix/mpt-overview)) können Sie die Umpacken von Ihrer vorhandenen desktop-Apps im MSIX-Format, auch wenn Sie keinen Zugriff auf ihren Quellcode haben. Sie können in der Befehlszeile oder über die interaktiven UI ausgeführt werden.
Desktop App Converter-Unterstützung für MSIX | Sie können den [Desktop App Converter](https://docs.microsoft.com/windows/uwp/porting/desktop-to-uwp-root) verwenden, um ein MSIX-Paket mithilfe der Ausgabe der `-MakeMSIX` Parameter.
Unterstützung für MSIX MakeAppx.exe-Tools | Sie können des MakeAppx.exe-Tools zum Erstellen eines MSIX-Pakets für UWP-apps oder traditionellen desktopanwendungen verwenden. Dieses Tool ist im Windows10 SDK enthalten und kann über eine Eingabeaufforderung oder eine Skriptdatei verwendet werden. </br> UWP-apps finden Sie unter [Erstellen eines app-Pakets mit dem MakeAppx.exe-Tool](https://docs.microsoft.com/windows/uwp/packaging/create-app-package-with-makeappx-tool). </br> Desktop-Apps finden Sie unter [Manuelles Verpacken eine desktop-Anwendung](https://docs.microsoft.com/windows/uwp/porting/desktop-to-uwp-manual-conversion).
Paket-Unterstützung Framework | Das [Paket Unterstützung Framework](https://docs.microsoft.com/windows/msix/package-support-framework-overview) ist ein open-Source-Kit, das hilft Ihnen das Anwenden von Updates für Ihre vorhandene desktop-Anwendung, wenn Sie keinen Zugriff auf den Quellcode haben, damit sie in einem MSIX-Container ausgeführt werden kann.
Store-Analyse-API | Die [Microsoft Store-Analyse-API](../monetize/access-analytics-data-using-windows-store-services.md) enthält jetzt die folgenden neuen Methoden: </br> * [Abrufen von internen Daten für Ihre UWP-app](../monetize/get-insights-data-for-your-app.md) </br> * [Abrufen von internen Daten für Ihre desktop-Anwendung](../monetize/get-insights-data-for-your-desktop-app.md) </br>* [Abrufen von Upgrade Blöcke für Ihre desktop-Anwendung](../monetize/get-desktop-block-data.md) </br> * [Abrufen von Details zur Upgrade-Blockierung für Ihre desktop-Anwendung](../monetize/get-desktop-block-data-details.md)

## <a name="videos"></a>Videos

Folgende Videos wurden seit dem Fall Creators Update veröffentlicht. Diese heben neue und verbesserte Features in Windows10 für Entwickler hervor.

### <a name="cwinrt"></a>C++/WinRT

C++ / WinRT ist eine neue Art der Erstellung und Nutzung von Windows-Runtime-APIs. Es verfügt über alleinige in Headerdateien implementiert und bietet Ihnen einen erstklassigen Zugriff auf moderne app-Features. [Das Video ansehen](https://www.youtube.com/watch?v=TLSul1XxppA&feature=youtu.be) , um zu erfahren, wie es funktioniert, dann [Lesen Sie die Entwicklerdokumentation](../cpp-and-winrt-apis/index.md) für Weitere Informationen.

### <a name="get-started-for-devs-create-and-customize-a-form-on-windows-10"></a>Erste Schritte für Entwickler: Erstellen und Anpassen eines Formulars unter Windows 10

Unsere [Erste Schritte-Dokumentation](../get-started/index.md) für Windows-Entwickler bieten jetzt praktische Erfahrung mit grundlegenden app-Entwicklungsaufgabe. In diesem Video führt Sie durch eine diese Themen und werden die Grundlagen der Erstellung eines Formulars Benutzeroberfläche in Ihrer app. [Das Video ansehen](https://www.youtube.com/watch?v=AgngKzq4hKI&feature=youtu.be) , um den Code in Aktion zu sehen, klicken Sie dann anzuzeigen [sehen Sie sich selbst sich das Thema.](http://aka.ms/CreateForms)

### <a name="enhance-your-bot-with-project-personality-chat"></a>Verbessern Sie Ihre Bot mit Projekt Persönlichkeit chat

Projekt Persönlichkeit Chat können Sie Ihre Chat-Bots eine anpassbare Rolle hinzufügen. Durch die Integration mit Microsoft Bot Framework SDK, können Sie kleine sprechen Funktionen für eine mehr gesprochener Möglichkeit zur Interaktion mit den Kunden hinzufügen. Erfahren Sie, wie Sie es, und dann [die interaktive Demo ausprobieren](http://aka.ms/PersonalityChat) für eine praktische Erfahrung implementieren [das Video ansehen](https://www.youtube.com/watch?v=5C_uD8g2QKg&feature=youtu.be) .

### <a name="multi-instance-uwp-apps"></a>UWP-Apps mit mehreren Instanzen

Windows können nun mehrere Instanzen von Ihrer UWP-app mit jeweils in eine eigene getrennten Prozess ausgeführt werden. Erfahren Sie, wie Sie eine neue app erstellen, die dieses Feature, dann [Lesen Sie die Entwicklerdokumentation](../launch-resume/multi-instance-uwp.md) für Weitere Hinweise zur Verwendung unterstützt und warum Sie dieses Feature verwenden [das Video ansehen](https://www.youtube.com/watch?v=clnnf4cigd0&feature=youtu.be) .

### <a name="xbox-live-unity-plugin"></a>Xbox Live Unity-Plug-in

Die Xbox Live-Plug-In für Unity enthält Unterstützung für das Hinzufügen von Xbox Live Signieren, Statistiken, Freunde-Listen, Cloud-Speicher und Bestenlisten auf den Titel. [Das Video ansehen](https://youtu.be/fVQZ-YgwNpY) , um mehr zu erfahren, und dann [das GitHub-Paket herunterzuladen](https://aka.ms/UnityPlugin) , für den Einstieg.

### <a name="one-dev-question"></a>One Dev Frage

In der One Dev Frage Videoserie behandelt seit Microsoft-Entwicklern eine Reihe von Fragen zur Windows-Entwicklung, Teamkultur, und zum Verlauf.

* [Raymond Chen Verlauf und Windows-Entwicklung](https://www.youtube.com/playlist?list=PLWs4_NfqMtoxjy3LrIdf2oamq1coolpZ7)

* [Larry Osterman Verlauf und Windows-Entwicklung](https://www.youtube.com/playlist?list=PLWs4_NfqMtoyPUkYGpJU0RzvY6PBSEA4K)

* [Aaron Gustafson auf Progressive Web-Apps](https://www.youtube.com/playlist?list=PLWs4_NfqMtoyPHoI-CIB71mEq-om6m35I)

* [Chris Heilmann für das Tool, webhint](https://www.youtube.com/playlist?list=PLWs4_NfqMtow00LM-vgyECAlMDxx84Q2v)

## <a name="samples"></a>Beispiele

### <a name="customer-orders-database"></a>Datenbank für Kundenaufträge

Das [Kundenauftragsdatenbank-Beispiel](https://github.com/Microsoft/Windows-appsample-customers-orders-database) wurde aktualisiert, um neue Steuerelemente wie [DataGrid](https://docs.microsoft.com/windows/communitytoolkit/controls/datagrid), [NavigationView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview)und [einer Erweiterung](https://docs.microsoft.com/windows/communitytoolkit/controls/expander)zu verwenden.

### <a name="customer-database-tutorial"></a>Lernprogramm für die Kunden

[Lernprogramm für die Kunden](../enterprise/customer-database-tutorial.md) erstellt eine einfache UWP-app für die Verwaltung von eine Liste der Kunden, und führt Konzepte und Verfahren vorherrschen nützlich. Es führt Sie durch die Implementierung von UI-Elemente und Vorgänge mit einer lokalen SQLite-Datenbank hinzufügen und losen Hinweise zum Herstellen einer Verbindung mit einer REST Remotedatenbank, wenn Sie fortfahren möchten.

### <a name="photo-editor-cwinrt"></a>Foto-Editor C++ / WinRT

Die [Foto-Editor-Beispiel-app](https://github.com/Microsoft/Windows-appsample-photo-editor) werden die Entwicklung mit der [C++ / WinRT](../cpp-and-winrt-apis/intro-to-using-cpp-with-winrt.md) Programmiersprache. Die app können Sie Fotos aus **der Bildbibliothek** abrufen und dann eine markierte Bild mit zugeordneten Fotoeffekte bearbeiten.

### <a name="windows-machine-learning"></a>Windows Machine Learning

[Windows Machine Learning -](https://github.com/Microsoft/Windows-Machine-Learning) Repository wurde aktualisiert, um die Arbeit mit den neuesten Windows 10 SDK, und enthält Beispiele in c#, C++ und JavaScript geschrieben wurden.

### <a name="xaml-hosting-api"></a>XAML-Hosting-API

Das [XAML-Hosting-API-Beispiel](https://github.com/Microsoft/Windows-appsample-Xaml-Hosting) ist eine Win32-desktop-app, die hervorhebt Sortiment Szenarien unter Verwendung der UWP-XAML hosting-API (auch als XAML-Inseln bezeichnet). Das Projekt enthält Windows Ink, Media Player und Navigationsansicht Steuerelemente in einer Galerie-Stil Präsentation. Außerhalb der allgemeinen Nutzung steuert, veranschaulicht das Beispiel auch Umgang mit XAML und systemeigenen Windows-Ereignisse/Nachrichten und grundlegende XAML-Datenbindung.