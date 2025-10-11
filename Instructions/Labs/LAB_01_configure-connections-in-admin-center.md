# Praktische Übung 1: Konfigurieren Sie die Verbindungen für Ihren Microsoft Graph-Connector

## WWL-Mandanten – Nutzungsbedingungen

Wenn Ihnen im Rahmen einer Schulung unter Anleitung ein Mandant zur Verfügung gestellt wird, beachten Sie bitte, dass der Mandant zur Unterstützung der praktischen Übungen in der Schulung unter Anleitung bereitgestellt wird.

Mandanten sollten nicht für Zwecke außerhalb von praktischen Übungen freigegeben oder verwendet werden. Der in diesem Kurs verwendete Mandant ist ein Testmandant und kann nach Abschluss des Kurses nicht mehr verwendet oder aufgerufen werden und kann nicht erweitert werden.

Mandanten dürfen nicht in ein kostenpflichtiges Abonnement konvertiert werden. Die im Rahmen dieses Kurses erworbenen Mandanten verbleiben im Eigentum der Microsoft Corporation, und wir behalten uns das Recht vor, jederzeit auf Mandanten zuzugreifen und diese zurückzuziehen.

## Zusammenfassung

In dieser Übung verwenden Sie das Microsoft 365 Admin Center, um mithilfe des Microsoft File Share Connectors eine Verbindung zu Kundendateien herzustellen.

Stellen Sie sich vor, Sie sind Kundendienstmanager bei Contoso, einem mittelständischen Unternehmen. Ihr Team hat mit Reaktionszeiten und gesamter Effizienz bei der Bearbeitung von Kundenanfragen zu kämpfen. Sie beschließen, das Microsoft 365 Admin Center zu nutzen, um eine FileShare-Verbindung zu erstellen, die es Ihren Kundendienstmitarbeitern ermöglicht, schnell auf Kundendateien zuzugreifen und diese zu referenzieren.

## Übung 1: Konfigurieren Sie die Verbindungen im Microsoft 365 Admin Center

### Anmelden am virtuellen Computer

Ihr Lab-Hosting-Anbieter stellt ein Kennwort für das MOD-Administratorkonto bereit, bei dem es sich um den Standardmandantenadministrator handelt. Aus Sicherheitsgründen hat Microsoft Ihren Testmandanten so konfiguriert, dass alle vordefinierten Benutzer ihr Kennwort bei der nächsten Anmeldung ändern müssen. Melden Sie sich bei **LON-CL1** als lokales **Administratorkonto** an, das von Ihrem Lab-Hostinganbieter mit dem Kennwort **Pa55w.rd** erstellt wurde.

### Aufgabe 1: Erteilen von Berechtigungen im Azure-Portal

1. Navigieren Sie zum Azure-Portal **https://www.portal.azure.com** und melden Sie sich mit Ihren Administrator-Anmeldedaten an. Speichern Sie das Passwort nicht und wählen Sie **Ja**, um **angemeldet bleiben?** aus.
2. Wählen Sie auf dem Bildschirm „**Willkommen bei Microsoft Azure**“ die Option „**Abbrechen“** aus.
1. Wählen Sie das Dropdown-Menü-Symbol auf der linken Seite des Bildschirms aus, um das Menü „Portal“ anzuzeigen. Klicken Sie auf „**Microsoft Entra ID auswählen -> Verwalten -> App-Registrierungen**.
1. Wählen Sie in der Menüleiste **Neue Registrierung** aus Die Seite **Antrag registrieren** wird angezeigt. Geben Sie auf dieser Seite einen Namen für die App ein. Nennen wir diese App **Contoso Fileshare**. Behalten Sie die Standardoption für unterstützte Kontotypen bei, d. h. **Nur Konten in diesem Organisationsverzeichnis (nur Contoso – Einzelmandant).** Wählen Sie keine optionale **Umleitungs-URI** aus.
1. Wählen Sie **Registrieren** aus. Ihre App wird erstellt, und ihr wird eine App-ID zugewiesen. Sie werden diese Informationen verwenden, wenn Sie ihren Graph Connector Agent (GCA) in den folgenden Schritten erstellen. Bevor wir jedoch den GCA erstellen, sollten wir die erforderlichen Einstellungen konfigurieren.

Der nächste Schritt besteht darin, Berechtigungen für den Graph-Connector-Agent im Azure-Portal zu erteilen:

1. Wählen Sie im linken Menü die Option **Verwalten -> API-Berechtigungen** aus.
1. Wählen Sie **Berechtigung hinzufügen -> Microsoft Graph -> Anwendungsberechtigungen** aus und erteilen Sie Berechtigungen für die folgenden APIs:

    - Directory -> Directory.Read.All
    - ExternalConnection -> ExternalConnection.ReadWrite.OwnedBy
    - ExternalItem -> ExternalItem.ReadWrite.All
      
1. Wählen Sie die Schaltfläche **Berechtigungen hinzufügen** aus.
1. Wählen Sie die **Administratorzustimmung für Contoso gewähren** aus und bestätigen Sie durch Auswahl von **Ja**.

**Hinweis:** Schließen Sie diese Edge-Browserregisterkarte nicht. Für die folgenden Aufgaben müssen Sie Informationen aus dem Azure-Portal kopieren und einfügen.

### Aufgabe 2: GCA installieren

1. Öffnen Sie eine neue Registerkarte im Microsoft Edge-Browser. Rufen Sie die folgende URL auf, um den Graph Connector Agenten herunterzuladen: **https://www.microsoft.com/en-us/download/details.aspx?id=104045**. Wählen Sie die Schaltfläche **Herunterladen**. 
1. Öffnen Sie die **GCAInstaller_3.1.1.0.msi** Datei, und folgen Sie den Anweisungen im Setup-Assistenten. 
2. Geben Sie in der Leiste **Suchen** am unteren Bildschirmrand **Graph connector agent config** ein und wählen Sie die App aus dem Menü aus, sobald sie angezeigt wird.
3. Wählen Sie **Ja** aus, um der App das Vornehmen von Änderungen zu erlauben.
4. Melden Sie sich an, und registrieren Sie die GCA mit dem **MOD-Verwaltungs**-Konto. Im Edge-Browser wird eine Bestätigung angezeigt, dass die Authentifizierung abgeschlossen ist. Sie können dieses Fenster schließen.
5. Öffnen Sie die GCA-App, indem Sie unten auf dem Bildschirm das Symbol auswählen.
1. Nennen Sie diesen Agenten **ContosoFiles**.
1. Wählen Sie die Registerkarte „Microsoft Edge“ im Azure-Portal aus (diese sollte „**Contoso Fileshare – Microsoft Azure**“ lauten), navigieren Sie zum Bildschirm **Übersicht** und kopieren Sie die **App-ID (Client)**. Fügen Sie sie in die GCA-Installations-App ein.

Als Nächstes müssen Sie im Azure-Portal ein Client-Secret für diese App festlegen.

1. Kehren Sie zur Edge-Registerkarte „Azure-Portal“ zurück und navigieren Sie zu **Zertifikate und Geheimnisse**.
1. Wählen Sie **+ Neuer Client** Geheimnis. Fügen Sie eine **Beschreibung** hinzu, indem Sie **Contoso-Dateien** eingeben. Sie können das Ablauffeld auf dem Standardwert „180 Tage" belassen.
2. Wählen Sie **Hinzufügen**.
3. Kopieren Sie das Feld **Wert** des stillen Geheimnisses.
1. Kehren Sie zur Graph Connector Agenten-App zurück und fügen Sie den Wert in das Feld **Anwendungskennwort (Clientgeheimnis)** der GCA-Installations-App ein.
1. Wählen Sie **Registrieren** aus.
1. Schließen Sie nach Abschluss der Registrierung die Installierungs-App.

### Aufgabe 3: Herunterladen der Ressourcendateien von Github

Um den Connector mithilfe der GCA zu konfigurieren, werden Sie Dateien benötigen, die sich lokal auf Ihrem System befinden. 

1. Öffnen Sie einen neuen Edge-Browser und geben Sie **https://github.com/MicrosoftLearning/MS-4014-Build-a-foundation-to-extend-Microsoft-365-Copilot/tree/master/ResourceFiles** in die Adressleiste ein.
2. Wählen Sie die erste Datei im Ordner aus, **Contoso Chai Tee-Markttrends 2023.xls** , um sie zu öffnen.
3. Wählen Sie die Schaltfläche **Auslassungspunkte (weitere Dateiaktionen)** oben rechts auf dem Bildschirm und dann **Herunterladen** aus.
4. Wiederholen Sie Schritt 3 für jede der verbleibenden Dateien.
5. Sie können dieses Fenster schließen, sobald die Downloads abgeschlossen sind.
6. Navigieren Sie im Datei-Explorer zu C:\ directory. Erstellen Sie einen neuen Ordner mit dem Namen **ResourceFiles**.
7. Öffnen Sie ein neues Datei-Explorer-Fenster, und navigieren Sie zum Ordner **Herunterladen** um auf die Contoso-Dateien zuzugreifen, die Sie von GitHub heruntergeladen haben. Kopieren Sie diese Dateien in das Verzeichneis **C:\ResourceFiles**.

Sie werden diese Dateien als Quellinhalt für die Verbindung verwenden, die Sie im Microsoft Admin Center erstellen.

### Aufgabe 4: Öffnen Sie das Microsoft Admin Center.

1. Öffnen Sie eine neue Registerkarte im Microsoft Edge-Browser. Rufen Sie in Ihrem Microsoft Edge-Browser die Seite **Microsoft 365 Home** auf, indem Sie die folgende URL in die Adressleiste eingeben: **https://portal.office.com**
1. Wenn Sie aufgefordert werden, sich anzumelden, geben Sie den LON-CL1 **Benutzernamen** und das **Passwort** ein, die Sie von Ihrem Übungs-Hosting-Anbieter für Ihren Microsoft 365-Testmandanten erhalten haben. Der Benutzername sollte das Format **<admin@xxxxxZZZZZZ.onmicrosoft.com>** haben, wobei xxxxxZZZZZZ das Mandantenpräfix ist, das Ihrem Lab-Hostinganbieter zugewiesen ist. 
1. Die Seite **Willkommen bei Microsoft 365** wird in Ihrem Edge-Browser auf der Registerkarte **Home | Microsoft 365** angezeigt. Dies ist die Microsoft 365-Startseite des MOD-Administrators.
1. Wählen Sie **Admin** aus der Liste der Anwendungssymbole aus, die im Navigationsbereich angezeigt wird. Dadurch wird das **Microsoft 365 Admin Center** in einem neuen Browser-Tab geöffnet.
1. Klicken Sie auf **... Alle anzeigen**, um das vollständige Navigationsmenü anzuzeigen. Wählen Sie **Einstellungen** -> **Search & Intelligence** aus.
1. Scrollen Sie auf der angezeigten Registerkarte **Übersicht** nach unten. Wählen Sie **Fügen Sie Ihre erste Verbindung** hinzu aus dem Feld **Connectors** aus.

Von hier aus wird eine Liste der verfügbaren Datenquellen aufgelistet. Wir konfigurieren eine Verbindung mit dem File Share Connector. Dieser Connector ermöglicht es der Microsoft 365 Copilot-Suche, Kundendateien innerhalb einer Copilot-Suche zu referenzieren, und ermöglicht schnellere Reaktionszeiten und genauere Antworten, als wenn Kundendienstmitarbeitende Antworten finden.

1. Klicken Sie auf **File Share** und dann auf **weiter**.
1. Geben Sie den **Display-Namen** ein. Der Display-Name ist ein eindeutiger Name, der Benutzenden angezeigt wird. Geben wir **Contoso-Dateien** in das Feld ein.
1. **Ordnerpfad** eingeben. Wir haben Beispieldateien im folgenden Verzeichnis konfiguriert:

   **C:\ResourceFiles**

Dieser Pfad ist das Verzeichnis, in dem die Dateien gespeichert werden.

1. Wählen Sie im Feld **Graph Connector Agent** den GCA aus, den wir in den vorherigen Schritten erstellt haben (ContosoFiles), falls dies nicht die Standardoption ist.
1. Wählen Sie **Windows** als Authentifizierungstyp aus und geben Sie dann den **Benutzernamen** und das **Passwort** für die Windows-Authentifizierung ein.
1. Aktivieren Sie das Kontrollkästchen, um Microsoft zu autorisieren, einen Index der Daten von Drittanbietern in Ihrem Mandanten zu erstellen.
1. Klicken Sie auf **Erstellen**.  Eine Erfolgsanzeige erscheint und Ihre Verbindung beginnt zu synchronisieren. Sie können den spezifischen Inhaltstyp, die Abteilungen in Ihrer Organisation, die von seiner Nutzung profitieren könnten, oder Beispiele für Arbeitsabläufe in das Feld **Connector-Beschreibung** eingeben. Zum Beispiel:

    **Dieser Konnektor enthält Informationen, die auf dem lokalen Dateifreigabeserver gespeichert sind. Er umfasst Kundenprofile und Kundenfragen, die dem Support-Desk bei der Beantwortung von Kundenanfragen helfen können.**
1. Wählen Sie **Fertig** aus. Ihre Verbindung wird nun auf der Registerkarte **Search & Intelligence** des Microsoft 365 Admin Centers angezeigt und kann in Ihren Suchergebnissen oder beim Erstellen eines Microsoft 365 Copilot-Agent referenziert werden.
