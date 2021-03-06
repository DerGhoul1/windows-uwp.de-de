---
ms.assetid: 76776b0f-3163-48c9-835b-3f4213968079
title: Datenzugriff
description: In diesem Abschnitt wird das Speichern von Daten auf dem Gerät in einer privaten Datenbank und die Verwendung der objektrelationalen Zuordnung in UWP-Apps (Universelle Windows-Plattform) erläutert.
ms.date: 11/13/2017
ms.topic: article
keywords: Windows 10, UWP, Daten, Datenbank, relational, Tabellen, SQLite
ms.localizationpriority: medium
ms.openlocfilehash: 80cf97a7336fe912baf5cda151560dacfa12c2e2
ms.sourcegitcommit: f727b68e86a86c94eff00f67ed79a1c12666e7bc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/29/2020
ms.locfileid: "71339715"
---
# <a name="data-access"></a>Datenzugriff

Mithilfe einer SQLite-Datenbank können Sie Daten auf dem Gerät des Benutzers speichern. Sie können Ihre App auch direkt mit einer SQL Server-Datenbank verbinden, ohne eine Dienstebene verwenden zu müssen.

| Thema | Beschreibung|
|-------|------------|
| [Verwenden einer SQLite-Datenbank in einer UWP-App](sqlite-databases.md) | Hier wird gezeigt, wie Sie SQLite verwenden, um Daten in einer einfachen Datenbank auf dem Gerät des Benutzers zu speichern und abzurufen. SQLite ist ein eingebettetes Datenbankmodul ohne Server. |
| [Verwenden einer SQL Server-Datenbank in einer UWP-App](sql-server-databases.md) | Hier wird erläutert, wie Sie eine direkte Verbindung mit einer SQL Server-Datenbank herstellen und dann Daten über Klassen im Namespace [System.Data.SqlClient](https://docs.microsoft.com/dotnet/api/system.data.sqlclient) speichern und abrufen. Es ist keine Dienstebene erforderlich. |

## <a name="related-topics"></a>Verwandte Themen

* [Beispieldatenbank für Kundenbestellung](https://github.com/Microsoft/Windows-appsample-customers-orders-database)
