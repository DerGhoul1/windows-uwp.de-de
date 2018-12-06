---
Description: In order to add and manage account users, you must first associate your Partner Center account with your organization's Azure Active Directory.
title: Zuordnen von Azure Active Directory mit Ihrem Partner Center-Konto
ms.date: 10/31/2018
ms.topic: article
keywords: Windows10, UWP, Azure Ad, Azure-Mandant, AAD-Mandant, Azure AD-Mandant, Mandantenverwaltung, Mandanten
ms.localizationpriority: medium
ms.openlocfilehash: 9f807799740d7e832da2f6a6fa3ea63e00deaee4
ms.sourcegitcommit: d7613c791107f74b6a3dc12a372d9de916c0454b
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 12/05/2018
ms.locfileid: "8748079"
---
# <a name="associate-azure-active-directory-with-your-partner-center-account"></a>Zuordnen von Azure Active Directory mit Ihrem Partner Center-Konto

Damit [Hinzufügen und Verwalten von Kontobenutzern](add-users-groups-and-azure-ad-applications.md)müssen Sie zunächst Ihre Organisation Azure Active Directory Ihr Partner Center-Konto zuordnen. 

[Partner Center](https://partner.microsoft.com/dashboard) nutzt Azure AD zum Verwalten mehrerer Benutzer Konto und zum Zugriff auf. Wenn in Ihrer Organisation bereits mit Office365 oder anderen Unternehmensdiensten von Microsoft gearbeitet wird, verfügen Sie bereits über Azure AD. Andernfalls können erstellen Sie ein neues Azure AD-Mandanten aus im Partner Center ohne zusätzliche Kosten.

> [!TIP]
> Dieses Thema gilt speziell für das Entwicklerprogramm für Windows-apps im [Partner Center](https://partner.microsoft.com/dashboard), aber zuordnen ein Mandanten und Verwalten von Benutzern für Konten im Windows-Desktopanwendungsprogramm verhält (finden Sie unter [Windows-Desktopanwendungsprogramm](https://docs.microsoft.com/windows/desktop/appxpkg/windows-desktop-application-program#add-and-manage-account-users) für Weitere Informationen) und im Windows-Hardware-Entwicklerprogramm (, in denen Verweise auf die **Manager** -Rolle gelten auch für Hardware-Konten mit der Rolle " **Administrator** "; finden Sie weitere Informationen unter [Dashboard-Verwaltung](https://docs.microsoft.com/windows-hardware/drivers/dashboard/dashboard-administration) ).

Eine einzelne Azure AD-Mandanten kann mehrere Partner Center-Konten zugeordnet werden. Sie müssen nur ein Azure AD-Mandanten mit Ihrem Partner Center-Konto verknüpft ist, um mehrere Kontobenutzer hinzuzufügen, aber Sie haben auch die Möglichkeit, ein einzelnes Partner Center-Konto mehrere Azure AD-Mandanten hinzuzufügen. Jeder Benutzer mit der **Manager-** Rolle im Partner Center-Konto wird die Option zum Hinzufügen und Entfernen von Azure AD-Mandanten von diesem Konto verfügen.

> [!IMPORTANT]
> Nachdem Sie Ihr Partner Center-Konto mit Ihrem Azure AD-Mandanten zugeordnet haben, müssen Sie zum Hinzufügen und Verwalten von Kontobenutzern Mandanten, in das Partner Center als Anwender im gleichen Mandanten anmelden, der über die **Manager** -Rolle verfügt.


## <a name="associate-your-partner-center-account-with-your-organizations-existing-azure-ad-tenant"></a>Ordnen Sie Ihr Partner Center-Konto mit vorhandenen Azure AD-Mandanten Ihrer Organisation

Gehen folgendermaßen Sie vor, um Ihr Partner Center-Konto verknüpfen, wenn Ihr Unternehmen bereits Azure AD verwendet, haben.

1.  [Partner Center](https://partner.microsoft.com/dashboard) wählen Sie das Zahnradsymbol (in der Nähe der oberen rechten Ecke des Dashboards), und wählen Sie dann **entwicklereinstellungen**. Wählen Sie im Menü " **Einstellungen** " **Mandanten**aus.
2.  **Zuordnen von Azure AD mit Ihrem Partner Center-Konto**auswählen.
3.  Geben Sie Ihre Azure AD-Anmeldeinformationen für den Mandanten ein, den Sie zuordnen möchten.
4.  Überprüfen Sie den Organisations- und den Domänennamen für den Azure AD-Mandant. Wählen Sie zum Abschließen der Zuordnung **Bestätigen** aus.
5.  Wenn die Zuordnung erfolgreich ist, dann werden Sie zum Hinzufügen und Verwalten von Kontobenutzern im Abschnitt " **Benutzer** " im Partner Center bereit.

> [!IMPORTANT]
> Um neue Benutzer zu erstellen oder andere Änderungen an Ihrem Azure AD vorzunehmen, müssen Sie sich mit einem Konto beim Azure AD-Mandanten anmelden, das über [über globale Administratorberechtigungen](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles) für den Mandanten verfügt. Sie benötigen keinen globale Administratorberechtigungen um Mandanten zuzuordnen oder bereits vorhandene Benutzer im Mandanten zu Ihrem Partner Center-Konto hinzufügen.


## <a name="create-a-brand-new-azure-ad-to-associate-with-your-partner-center-account"></a>Erstellen eines völlig neues Azure AD mit Ihrem Partner Center-Konto zuordnen

Wenn Sie ein neues Azure AD einrichten, um mit Ihrem Partner Center-Konto verknüpfen möchten, gehen Sie wie folgt vor.

1.  [Partner Center](https://partner.microsoft.com/dashboard)wählen Sie das Zahnradsymbol (in der Nähe der oberen rechten Ecke des Dashboards), und wählen Sie dann **entwicklereinstellungen**. Wählen Sie im Menü " **Einstellungen** " **Mandanten**aus.
2.  Wählen Sie **Neues Azure AD erstellen**.
3.  Geben Sie die Verzeichnisinformationen für das neue Azure AD ein:
    - **Domänenname**: Der eindeutige Name, der für Ihre Azure AD-Domäne verwendet wird, zusammen mit „.onmicrosoft.com“. Wenn Sie beispielsweise „beispiel“ eingegeben haben, wäre Ihre Azure AD-Domäne „beispiel.onmicrosoft.com“.
    - **Kontakt-E-Mail-Adresse**: Eine E-Mail-Adresse, unter der wir Sie hinsichtlich Ihres Kontos erreichen können, wenn notwendig.
    - **Benutzerkontoinformationen für den globalen Administrator**: Vorname, Nachname, Benutzername und Kennwort, die Sie für das neue globale Administratorkonto verwenden möchten.
4.  Klicken Sie auf **Erstellen**, um die neue Domäne und die Kontoinformationen zu bestätigen.
5.  Melden Sie sich mit dem neuen Azure AD-Benutzernamen und -Kennwort als globaler Administrator an, um [zusätzliche Kontobenutzer hinzuzufügen und zu verwalten](add-users-groups-and-azure-ad-applications.md).


## <a name="manage-azure-ad-tenant-associations"></a>Verwalten von Azure AD-Mandantenzuordnungen

Nachdem Sie Ihr Partner Center-Konto einen Azure AD-Mandanten zugeordnet haben, können Sie neue Mandanten hinzufügen oder Entfernen von vorhandenen Mandanten aus der **Mandanten** -Seite.


### <a name="add-multiple-azure-ad-tenants-to-your-partner-center-account"></a>Mehrere Azure AD-Mandanten zu Ihrem Partner Center-Konto hinzufügen

Jeder Benutzer die **Manager** -Rolle für ein Partner Center-Konto kann Azure AD-Mandanten das Konto zuordnen.

Wählen Sie zum Zuordnen eines neuen Mandanten **Associate another Azure AD tenant** aus und befolgen Sie die oben angegebenen Schritte. Beachten Sie, dass Sie zur Angabe Ihrer Anmeldeinformationen in dem Azure AD-Mandanten aufgefordert werden, den Sie verknüpfen möchten.


### <a name="remove-an-azure-ad-tenant-from-your-partner-center-account"></a>Entfernen von Azure AD-Mandanten aus Ihrem Partner Center-Konto

Jeder Benutzer die **Manager** -Rolle für ein Partner Center-Konto kann Azure AD-Mandanten von diesem Konto entfernen.

> [!IMPORTANT]
> Wenn Sie einen Mandanten zu entfernen, werden alle Benutzer, die das Partner Center-Konto über diesen Mandanten hinzugefügt wurden, nicht mehr auf das Konto anmelden. 

Um einen Mandanten zu entfernen, suchen Sie den Namen auf der Seite " **Mandanten** " (in den **kontoeinstellungen**), und wählen Sie dann **Entfernen**. Sie werden aufgefordert, zu bestätigen, dass Sie den Mandanten entfernen möchten. Nachdem Sie dies tun, keine Benutzer im Mandanten werden in der Lage, sich im Partner Center-Konto anmelden, und alle Berechtigungen, die Sie für diesen Benutzer konfiguriert haben, entfernt werden.

> [!TIP]
> Sie können einen Mandanten entfernen, wenn Sie über ein Konto im gleichen Mandanten derzeit in Partner Center angemeldet sind. Um einen Mandanten zu entfernen, müssen Sie sich beim Partner Center als **Manager** für einen anderen Mandanten anmelden, die dem Konto zugeordnet ist. Wenn nur ein Mandanten dem Konto zugeordnet ist, kann dieser Mandant nur nach der Anmeldung mithilfe des Microsoft-Kontos entfernt werden, das das Konto eröffnete.

