---
description: Verwenden Sie die C#-Codebeispiele in diesem Abschnitt, um mehr über das Einreichen von Spieloptionen und Trailern über die Verwendung der Microsoft Store-Übermittlungs-API zu erfahren.
title: C#-Beispiel – App-Übermittlung mit Spieloptionen und Trailer
ms.date: 07/10/2017
ms.topic: article
keywords: Windows 10, Uwp, Microsoft Store-Übermittlungs-API, Codebeispiele, Spieloptionen, Trailer, erweiterte Angebote, C#
ms.localizationpriority: medium
ms.openlocfilehash: 8ffb11c020eacd687ab72274b04f41406c3df2af
ms.sourcegitcommit: 6a7dd4da2fc31ced7d1cdc6f7cf79c2e55dc5833
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/21/2019
ms.locfileid: "58334638"
---
# <a name="c-sample-app-submission-with-game-options-and-trailers"></a>C\# Beispiel: app-Übermittlung mit Optionen für Spiele und -Nachspänne

Dieser Artikel enthält C#-Codebeispiele zeigt das Verwenden der [Microsoft Store-Übermittlungs-API](create-and-manage-submissions-using-windows-store-services.md) für diese Aufgaben:

* Abrufen eines Azure AD-Zugriffstokens zur Nutzung mit der Microsoft Store-Übermittlungs-API.
* Erstellen einer App-Übermittlung
* Konfigurieren von Store-Eintragsdateien für die App-Übermittlung, einschließlich der erweiterten Eintragsoptionen [Spiele](manage-app-submissions.md#gaming-options-object) und [Trailer](manage-app-submissions.md#trailer-object).
* Hochladen der ZIP-Datei mit den Paketen, Eintragsbildern und Trailerdateien für die App-Übermittlung.
* Ausführen des Commit für eine App-Übermittlung.

Sie können die einzelnen Beispiele durchgehen, um mehr über die jeweilige Aufgabe zu erfahren. Zudem können Sie alle Codebeispiele in diesem Artikel in eine Konsolenanwendung einbinden. Um die Beispiele zu übernehmen, erstellen Sie in Visual Studio eine C#-Konsolenanwendung mit dem Namen **DevCenterApiSample**, kopieren Sie die einzelnen Beispiele in separate Codedateien des Projekts, und erstellen Sie das Projekt.

## <a name="prerequisites"></a>Vorraussetzungen

Für diese Beispiele gelten die folgenden Anforderungen:

* Hinzufügen eines Verweises auf die the System.Web-Assembly zu Ihrem Projekt
* Installieren Sie das [Newtonsoft.Json](https://www.newtonsoft.com/json) NuGet-Paket von Newtonsoft für Ihr Projekt.

<span id="create-app-submission" />

## <a name="create-an-app-submission"></a>Erstellen einer App-Übermittlung

Die ```CreateAndSubmitSubmissionExample```-Klasse definiert eine öffentliche ```Execute```-Methode, die anderen Beispielmethoden aufruft, um die Microsoft Store-Übermittlungs-API zum Erstellen und Ausführen eines Commits einer App-Übermittlung mit Optionen und einem Trailer verwendet. So passen Sie den Code für eigene Zwecke an:

* Weisen Sie die ```tenantId```-Variable zur Mandanten-ID für Ihre App zu und weisen Sie die Variablen ```clientId``` und ```clientSecret``` zur Client-ID und dem Schlüssel für die App zu. Weitere Informationen finden Sie unter [eine Azure AD-Anwendung mit Ihrem Partner Center-Konto zuordnen.](create-and-manage-submissions-using-windows-store-services.md#how-to-associate-an-azure-ad-application-with-your-partner-center-account)
* Weisen Sie die ```applicationId```-Variable zur [Store-ID](in-app-purchases-and-trials.md#store-ids) der App zu, für die eine Übermittlung erstellen möchten.

> [!div class="tabbedCodeSnippets"]
[!code-csharp[SubmissionApi](./code/StoreServicesExamples_SubmissionAdvancedListings/cs/CreateAndSubmitSubmissionExample.cs#CreateAndSubmitSubmissionExample)]

<span id="token" />

## <a name="obtain-an-azure-ad-access-token"></a>Abrufen eines Azure AD-Zugriffstokens

Die ```DevCenterAccessTokenClient```-Klasse definiert eine Hilfsmethode, die Ihre ```tenantId```, ```clientId``` und ```clientSecret``` Werte zum Erstellen eines Azure AD-Zugriffstokens zur Verwendung mit Microsoft Store-Übermittlungs-API verwendet.

> [!div class="tabbedCodeSnippets"]
[!code-csharp[SubmissionApi](./code/StoreServicesExamples_SubmissionAdvancedListings/cs/DevCenterAccessTokenClient.cs#DevCenterAccessTokenClient)]

<span id="utilities" />

## <a name="helper-methods-to-invoke-the-submission-api-and-upload-submission-files"></a>Hilfsmethoden zum Aufrufen von der Übermittlungs-API und zum Hochladen von Übermittlungsdateien

Die ```DevCenterClient``` Klasse definiert Hilfsmethoden, die eine Vielzahl von Methoden in der Microsoft Store-Übermittlungs-API aufrufen und die ZIP-Datei mit den Paketen, Bildern und Trailerdateien für die App-Übermittlung hochladen.

> [!div class="tabbedCodeSnippets"]
[!code-csharp[SubmissionApi](./code/StoreServicesExamples_SubmissionAdvancedListings/cs/DevCenterClient.cs#DevCenterClient)]

## <a name="related-topics"></a>Verwandte Themen

* [Erstellen und Verwalten von Übermittlungen, die mithilfe von Microsoft Store services](create-and-manage-submissions-using-windows-store-services.md)
