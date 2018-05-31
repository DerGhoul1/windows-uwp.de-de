---
author: stevewhims
description: Eine Eigenschaft, die effektiv an ein XAML-Steuerelement gebunden werden kann, wird als *Observable*-Eigenschaft bezeichnet. Dieses Thema zeigt, wie man eine Observable-Eigenschaft implementiert und nutzt und wie man ein XAML-Steuerelement daran bindet.
title: XAML-Steuerelemente; Binden an eine C++/WinRT-Eigenschaft
ms.author: stwhi
ms.date: 03/07/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, standard, c++, cpp, winrt, projizierung, XAML, steuerelement, binden, eigenschaft
ms.localizationpriority: medium
ms.openlocfilehash: b54f0dd60a90cd13e5b3586a956b09e30f6d9755
ms.sourcegitcommit: ab92c3e0dd294a36e7f65cf82522ec621699db87
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/03/2018
ms.locfileid: "1832284"
---
# <a name="xaml-controls-bind-to-a-cwinrtwindowsuwpcpp-and-winrt-apisintro-to-using-cpp-with-winrt-property"></a>XAML-Steuerelemente; Binden an eine [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt)-Eigenschaft
> [!NOTE]
> **Einige Informationen beziehen sich auf die Vorabversion, die vor der kommerziellen Freigabe möglicherweise wesentlichen Änderungen unterliegt. Microsoft übernimmt keine Garantie, weder ausdrücklich noch stillschweigend, für die hier bereitgestellten Informationen.**

Eine Eigenschaft, die effektiv an ein XAML-Steuerelement gebunden werden kann, wird als *Observable*-Eigenschaft bezeichnet. Dieses Konzept basiert auf dem Software-Design-Muster, das als *Observer-Pattern* bekannt ist. Dieses Thema zeigt, wie man Observable-Eigenschaften in C++/WinRT implementiert und wie man XAML-Steuerelemente an diese bindet.

> [!IMPORTANT]
> Wichtige Konzepte und Begriffe, die Ihr Verständnis für die Verwendung von Laufzeitklassen mit C++/WinRT unterstützen, finden Sie unter [Verwenden von APIs mit C++/WinRT](consume-apis.md) und [Erstellen von APIs mit C++/WinRT](author-apis.md).

## <a name="what-does-observable-mean-for-a-property"></a>Was bedeutet *observable* für eine Eigenschaft?
Angenommen, eine Laufzeitklasse namens **BookSku** hat eine Eigenschaft namens **Title**. Wenn **BookSku** das Ereignis [**INotifyPropertyChanged::PropertyChanged**](/uwp/api/windows.ui.xaml.data.inotifypropertychanged.PropertyChanged) auslöst, wenn sich der Wert von **Title** ändert, dann ist **Title** eine Observable-Eigenschaft. Es ist das Verhalten von **BookSku** (Auslösen oder nicht Auslösen des Ereignisses), das bestimmt, welche seiner Eigenschaften Observable-Eigenschaften sind.

Ein XAML-Textelement oder -Steuerelement kann sich an diese Ereignisse binden und sie verarbeiten, indem es den/die aktualisierten Wert(e) abruft und dann eine Aktualisierung durchführt, um den neuen Wert anzuzeigen.

> [!NOTE]
> Informationen über die aktuelle Verfügbarkeit der C++/WinRT Visual Studio Extension (VSIX) (die Projektvorlagenunterstützung sowie C++/WinRT MSBuild-Eigenschaften und -Ziele bietet) finden Sie unter [Visual Studio-Unterstützung für C++/WinRT und VSIX](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-and-the-vsix).

## <a name="create-a-blank-app-bookstore"></a>Erstellen einer leeren App (Bookstore)
Erstellen Sie zunächst ein neues Projekt in Microsoft Visual Studio. Erstellen Sie ein **Visual C++ Blank App (C++/WinRT)**-Projekt und nennen Sie es *Bookstore*.

Wir werden eine neue Klasse schreiben, um ein Buch mit einer Observable-Eigenschaft namens „Titel” darzustellen. Wir erstellen und nutzen die Klasse innerhalb derselben Kompilierungseinheit. Aber wir wollen in der Lage sein, aus XAML eine Bindung an diese Klasse zu nutzen. Daher wird es eine Laufzeitklasse sein. Und wir werden C++/WinRT verwenden, um diese zu schreiben und zu nutzen.

Der erste Schritt beim Erstellen einer neuen Laufzeitklasse besteht darin, dem Projekt ein neues **Midl-Datei (.idl)**-Element hinzuzufügen. Nennen Sie es `BookSku.idl`. Löschen Sie den Standardinhalt von `BookSku.idl` und fügen Sie diese Laufzeitklassendeklaration ein.

```idl
// BookSku.idl
namespace Bookstore
{
    runtimeclass BookSku : Windows.UI.Xaml.DependencyObject, Windows.UI.Xaml.Data.INotifyPropertyChanged
    {
        String Title;
    }
}
```

> [!IMPORTANT]
> Damit eine Anwendung die Tests des [Zertifizierungskits für Windows-Apps](../debug-test-perf/windows-app-certification-kit.md) bestehen und erfolgreich in den Microsoft Store aufgenommen werden kann, muss die ultimative Basisklasse jeder *in der Anwendung deklarierten* Laufzeitklasse ein Typ sein, der aus einem Windows.*-Namespace stammt.

Um diese Anforderung zu erfüllen, leiten Sie Ihre Ansichtsmodell-Klassen von [**Windows.UI.Xaml.DependencyObject**](/uwp/api/windows.ui.xaml.dependencyobject) ab. Alternativ können Sie eine von **DependencyObject** abgeleitete bindbare Basisklasse deklarieren und daraus Ihre Ansichtsmodelle ableiten. Sie können Ihre Datenmodelle als C++ Strukturen deklarieren. Sie müssen nicht in MIDL deklariert werden (solange Sie sie nur in Ihren Ansichtsmodellen nutzen und XAML nicht direkt an sie binden – in diesem Fall wären sie allerdings per Definition sowieso Ansichtsmodelle).

Speichern Sie die Datei, und erstellen Sie das Projekt. Während des Buildprozesses wird das `midl.exe`-Tool ausgeführt, um eine Windows-Runtime-Metadaten-Datei (`\Bookstore\Debug\Bookstore\Unmerged\BookSku.winmd`) zu erstellen, die die Laufzeitklasse beschreibt. Dann wird das `cppwinrt.exe`-Tool ausgeführt, um Quellcodedateien zu erzeugen, die Sie bei der Erstellung und Nutzung Ihrer Laufzeitklasse unterstützen. Diese Dateien enthalten Stubs, um mit der Implementierung der **BookSku**-Laufzeitklasse zu beginnen, die Sie in Ihrer IDL deklariert haben. Diese Stubs sind `\Bookstore\Bookstore\Generated Files\sources\BookSku.h` und `BookSku.cpp`.

Kopieren Sie die Stub-Dateien `BookSku.h` und `BookSku.cpp` von `\Bookstore\Bookstore\Generated Files\sources\` in den Projektordner `\Bookstore\Bookstore\`. Stellen Sie im **Projektmappen-Explorer** sicher, dass **Alle Dateien anzeigen** aktiviert ist. Klicken Sie mit der rechten Maustaste auf die kopierten Stub-Dateien und klicken Sie auf **In Projekt aufnehmen**.

## <a name="implement-booksku"></a>Implementieren von **BookSku**
Nun öffnen wir `\Bookstore\Bookstore\BookSku.h` und `BookSku.cpp` und implementieren unsere Laufzeitklasse. Fügen Sie in `BookSku.h` einen Konstruktor hinzu, der ein [**winrt::hstring**](/uwp/cpp-ref-for-winrt/hstring)-Argument (ein privates Mitglied zum Speichern des Titels) und eine weiteres für das bei einer Titeländerung ausgelöste Ereignis entgegen nimmt. Nachdem Sie diese hinzugefügt haben, sieht Ihr `BookSku.h` so aus.

```cppwinrt
// BookSku.h
#pragma once

#include "BookSku.g.h"

namespace winrt::Bookstore::implementation
{
    struct BookSku : BookSkuT<BookSku>
    {
        BookSku() = delete;
        BookSku(hstring const& title);

        hstring Title();
        void Title(hstring const& value);
        event_token PropertyChanged(Windows::UI::Xaml::Data::PropertyChangedEventHandler const& value);
        void PropertyChanged(event_token const& token);
    
    private:
        hstring title;
        event<Windows::UI::Xaml::Data::PropertyChangedEventHandler> propertyChanged;
    };
}
```

Implementieren Sie in `BookSku.cpp` die Funktionen wie folgt.

```cppwinrt
// BookSku.cpp
#include "pch.h"
#include "BookSku.h"

namespace winrt::Bookstore::implementation
{
    BookSku::BookSku(hstring const& title)
    {
        Title(title);
    }

    hstring BookSku::Title()
    {
        return title;
    }

    void BookSku::Title(hstring const& value)
    {
        if (title != value)
        {
            title = value;
            propertyChanged(*this, Windows::UI::Xaml::Data::PropertyChangedEventArgs{ L"Title" });
        }
    }

    event_token BookSku::PropertyChanged(Windows::UI::Xaml::Data::PropertyChangedEventHandler const& handler)
    {
        return propertyChanged.add(handler);
    }

    void BookSku::PropertyChanged(event_token const& token)
    {
        propertyChanged.remove(token);
    }
}
```

In der **Title**-Zugriffsfunktion prüfen wir, ob ein anderer Wert gesetzt wird, und wenn ja, aktualisieren wir den Titel und lösen außerdem das Ereignis [**INotifyPropertyChanged::PropertyChanged**](/uwp/api/windows.ui.xaml.data.inotifypropertychanged.PropertyChanged) mit einem Argument aus, das dem Namen der geänderten Eigenschaft entspricht. Dies dient dazu, dass die Benutzeroberfläche (UI) erkennt, welcher Eigenschaftswert erneut abgefragt werden muss.

## <a name="declare-and-implement-bookstoreviewmodel"></a>**BookstoreViewModel** deklarieren und implementieren
Unsere XAML-Hauptseite wird sich an ein Hauptansichtsmodell gebunden. Dieses Ansichtsmodell wird mehrere Eigenschaften haben, darunter eine vom Typ **BookSku**. In diesem Schritt deklarieren und implementieren wir unsere Hauptansichtsmodell-Laufzeitklasse.

Fügen Sie eine neue **Midl-Datei (.idl)** mit dem Namen `BookstoreViewModel.idl` hinzu.

```idl
// BookstoreViewModel.idl
import "BookSku.idl";

namespace Bookstore
{
    runtimeclass BookstoreViewModel : Windows.UI.Xaml.DependencyObject
    {
        BookSku BookSku{ get; };
    }
}
```

Speichern und erstellen Sie das Projekt. Kopieren Sie `BookstoreViewModel.h` und `BookstoreViewModel.cpp` aus dem `Generated Files`-Ordner in den Projektordner und nehmen Sie sie in das Projekt auf. Öffnen Sie diese Dateien und implementieren Sie die Laufzeitklasse wie folgt.

```cppwinrt
// BookstoreViewModel.h
#pragma once

#include "BookstoreViewModel.g.h"
#include "BookSku.h"

namespace winrt::Bookstore::implementation
{
    struct BookstoreViewModel : BookstoreViewModelT<BookstoreViewModel>
    {
        BookstoreViewModel();
        Bookstore::BookSku BookSku();

    private:
        Bookstore::BookSku m_bookSku{ nullptr };
    };
}
```

```cppwinrt
// BookstoreViewModel.cpp
#include "pch.h"
#include "BookstoreViewModel.h"

namespace winrt::Bookstore::implementation
{
    BookstoreViewModel::BookstoreViewModel()
    {
        m_bookSku = make<Bookstore::implementation::BookSku>(L"Atticus");
    }

    Bookstore::BookSku BookstoreViewModel::BookSku()
    {
        return m_bookSku;
    }
}
```

> [!NOTE]
> Der Typ von `m_bookSku` ist der projizierte Typ (**winrt::Bookstore::BookSku**), und der Template-Parameter, den Sie mit **make** verwenden, ist der Implementierungstyp (**winrt::Bookstore::implementation::BookSku**). Dennoch gibt **make** eine Instanz des projizierten Typs zurück.

## <a name="add-a-property-of-type-bookstoreviewmodel-to-mainpage"></a>Hinzufügen einer Eigenschaft vom Typ **BookstoreViewModel** zu **MainPage**
Öffnen Sie `MainPage.idl` (unsere Haupt-UI-Seite) mit der Deklaration der Laufzeitklasse. Fügen Sie eine Import-Anweisung zum Import von `BookstoreViewModel.idl` hinzu und fügen Sie eine schreibgeschützte Eigenschaft namens MainViewModel vom Typ **BookstoreViewModel** hinzu.

```idl
// MainPage.idl
import "BookstoreViewModel.idl";

namespace BookstoreCPPWinRT
{
    runtimeclass MainPage : Windows.UI.Xaml.Controls.Page
    {
        MainPage();
        BookstoreViewModel MainViewModel{ get; };
    }
}
```

Erstellen Sie das Projekt neu, um die Quellcodedateien, in denen die **MainPage**-Laufzeitklasse implementiert ist (`\Bookstore\Bookstore\Generated Files\sources\MainPage.h` und `MainPage.cpp`), neu zu generieren. Kopieren Sie die Zugriffs-Stubs für die ViewModel-Eigenschaft aus den generierten Dateien in `\Bookstore\Bookstore\MainPage.h` und `MainPage.cpp`.

Fügen Sie zu `\Bookstore\Bookstore\MainPage.h` ein privates Mitglied hinzu, um das Ansichtsmodell zu speichern. Beachten Sie, dass die Zugriffsfunktion für die Eigenschaft (und das Mitglied m_mainViewModel) als **Bookstore::BookstoreViewModel** (dem projizierten Typ) implementiert ist. Der Implementierungstyp befindet sich im selben Projekt (Kompilierungseinheit), so dass wir m_mainViewModel über die Konstruktorüberladung mit `nullptr` konstruieren.

```cppwinrt
// MainPage.h
...
namespace winrt::Bookstore::implementation
{
    struct MainPage : MainPageT<MainPage>
    {
        MainPage();

        Bookstore::BookstoreViewModel MainViewModel();

    private:
        void ClickHandler(Windows::Foundation::IInspectable const& sender, Windows::UI::Xaml::RoutedEventArgs const& args);

        Bookstore::BookstoreViewModel m_mainViewModel{ nullptr };

        friend class MainPageT<MainPage>;
    };
}
...
```

Nehmen Sie `BookstoreViewModel.h` (Deklaration des Implementierungstyps) in `\Bookstore\Bookstore\MainPage.cpp` auf. Rufen Sie [**winrt::make**](/uwp/cpp-ref-for-winrt/make) auf (mit dem Implementierungstyp), um m_mainViewModel eine neue Instanz des projizierten Typs zuzuweisen. Weisen Sie einen Initialwert für den Titel des Buches zu. Implementieren Sie die Zugriffsfunktion für die MainViewModel-Eigenschaft. Aktualisieren Sie zum Schluss den Titel des Buches im Event-Handler der Schaltfläche.

```cppwinrt
// MainPage.cpp
#include "pch.h"
#include "MainPage.h"
#include "BookstoreViewModel.h"

using namespace winrt;
using namespace Windows::UI::Xaml;

namespace winrt::Bookstore::implementation
{
    MainPage::MainPage()
    {
        m_mainViewModel = make<Bookstore::implementation::BookstoreViewModel>();
        InitializeComponent();
    }

    Bookstore::BookstoreViewModel MainPage::MainViewModel()
    {
        return m_mainViewModel;
    }

    void MainPage::ClickHandler(IInspectable const&, RoutedEventArgs const&)
    {
        MainViewModel().BookSku().Title(L"To Kill a Mockingbird");
    }
}
```

## <a name="bind-the-button-to-the-title-property"></a>Binden Sie die Schaltfläche an die **Title**-Eigenschaft.
Öffnen Sie `MainPage.xaml` mit dem XAML-Markup für unsere UI-Hauptseite. Entfernen Sie den Namen von der Schaltfläche und ändern Sie den Wert der **Content**-Eigenschaft von einem Literal in einen Bindungsausdruck. Beachten Sie die Eigenschaft `Mode=OneWay` des Bindungsausdrucks (einseitig vom Ansichtsmodell zum UI). Ohne diese Eigenschaft reagiert die Benutzeroberfläche nicht auf Ereignisse zu Eigenschaftsänderungen.

```xaml
<Button Click="ClickHandler" Content="{x:Bind MainViewModel.BookSku.Title, Mode=OneWay}"/>
```

Erstellen Sie nun das Projekt und führen Sie es aus. Klicken Sie auf die Schaltfläche, um den **Click**-Ereignis-Handler auszuführen. Dieser Handler ruft die Zugriffsfunktion des Buches auf; diese Zugriffsfunktion löst ein Ereignis aus, um die Benutzeroberfläche wissen zu lassen, dass sich die **Title**-Eigenschaft geändert hat; die Schaltfläche fragt den Wert dieser Eigenschaft erneut ab, um ihren eigenen **Content**-Wert zu aktualisieren.

## <a name="important-apis"></a>Wichtige APIs
* [INotifyPropertyChanged::PropertyChanged](/uwp/api/windows.ui.xaml.data.inotifypropertychanged.PropertyChanged)
* [winrt::make Funktionsvorlage](/uwp/cpp-ref-for-winrt/make)

## <a name="related-topics"></a>Verwandte Themen
* [Verwenden von APIs mit C++/WinRT](consume-apis.md)
* [Erstellen von APIs mit C++/WinRT](author-apis.md)