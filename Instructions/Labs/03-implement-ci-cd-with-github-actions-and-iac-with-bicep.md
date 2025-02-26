---
lab:
  title: Implementieren von CI/CD mit GitHub Actions und IaC mit Bicep
  module: Deliver with DevOps
---

# Lab 03 – Implementieren von CI/CD mit GitHub Actions und IaC mit Bicep

## Geschätzte Zeitdauer: 40 Minuten

## Szenario

Erinnern Sie sich an das Szenario dieses Moduls, in dem Sie für ein Softwareentwicklungsunternehmen in der Einzelhandelsbranche arbeiten, das sicherstellen möchte, dass der Releaseprozess einer neuen Version ihrer Online-Store-Anwendung effizient und zuverlässig ist und gleichzeitig das Risiko von Fehlern minimiert. Da Sie sich entschieden haben, GitHub für die Lebenszyklusverwaltung von Anwendungen zu verwenden, bietet Ihnen dieses Lab die Möglichkeit, das GitHub-Repository zu forken und zu überprüfen, das den Quellcode einer Web-App, einen GitHub Actions-Workflow und eine Bicep-Vorlage enthält. Darüber hinaus können Sie die Zielumgebung konfigurieren und die Infrastruktur als Code (IaC) und CI/CD-Funktionalität überprüfen.

## Ziele

Dieses Lab deckt Folgendes ab:

- Vorbereiten des Azure-Abonnements für das Lab
- Implementieren von Infrastructure as Code (IaC) und CI/CD mit GitHub Actions und einer Bicep-Vorlage

> **Hinweis:** Im vorherigen Lab haben Sie GitHub Pages konfiguriert. Das bedeutet, dass Sie bereits Continuous Deployment implementiert haben, ohne es zu bemerken. GitHub Pages verwendet GitHub Actions (im Hintergrund), um die automatisierte Bereitstellung auszuführen, die auf Commits auf den Mainbranch folgen. Sie können dies überprüfen, indem Sie zur Registerkarte **Aktionen** auf der Hauptseite des geforkten Repository **Spoon-Knife** navigieren. Jetzt erweitern Sie diese Funktionalität. Insbesondere verwenden Sie einen benutzerdefinierten GitHub Actions-Workflow mit einer Bicep-Vorlage, um Web-Apps von Azure App Service in verschiedenen Azure-Regionen bereitzustellen und eine benutzerdefinierte .NET-Web-App in beiden Regionen bereitzustellen.

> **Hinweis:** Verwenden Sie für dieses und nachfolgen Labs dasselbe GitHub-Konto, das Sie für den Zweck des ersten Lab erstellt haben.

## Voraussetzungen

- Abgeschlossen [Lab 01 – Agile Planung und Verwaltung mit GitHub](01-agile-planning-management-using-github.md)
- Abgeschlossen [Lab 02 – Implementieren des Arbeitsablaufs mit GitHub](02-implement-manage-repositories-using-github.md)
- Ein Azure-Abonnement, auf das Sie mindestens als Mitwirkender zugreifen können. Falls Sie nicht über ein Azure-Abonnement verfügen, können Sie sich [für eine kostenlose Testversion registrieren](https://azure.microsoft.com/free).

## Übung 0: Vorbereiten des Azure-Abonnements für das Lab

> **Hinweis:** Wenn Sie ein neues Azure-Abonnement verwenden, sind einige der Azure-Ressourcenanbieter möglicherweise nicht registriert. Da der Registrierungsprozess einige Minuten dauern kann, registrieren Sie sie zu Beginn des Labs, um die Wartezeit zu minimieren.

> **Hinweis:** In diesem Lab verwenden Sie Azure Cloud Shell. Wenn Sie Azure Cloud Shell zum ersten Mal in Ihrem Abonnement verwenden, müssen Sie zuerst den entsprechenden Ressourcenanbieter registrieren. 

1. Öffnen Sie einen Webbrowser, und navigieren Sie zum Azure-Portal unter `https://portal.azure.com`.
1. Wenn Sie dazu aufgefordert werden, melden Sie sich mit Ihrem Microsoft Entra ID-Konto an, das Besitzerzugriff auf das Azure-Abonnement hat, das Ihnen zur Verfügung steht.
1. Geben Sie in der Registerkarte des Webbrowsers, die das Azure-Portal anzeigt, in das Suchfeld oben auf der Seite **`Subscriptions`** ein und wählen Sie in der Ergebnisliste **Abonnements**.
1. Wählen Sie auf der Seite **Abonnements** das Abonnement aus, das Sie für dieses Lab verwenden wollen.
1. Wählen Sie auf der Seite „Abonnements“ im vertikalen Menü auf der linken Seite **Ressourcenanbieter** aus.
1. Suchen Sie in der Liste der Ressourcenanbieter nach **Microsoft.CloudShell**, und wählen Sie sie aus.
1. Wenn Sie den Ressourcenanbieter **Microsoft.CloudShell** ausgewählt haben, wählen Sie in der Symbolleiste **Registrieren** aus.

   > **Hinweis:** Warten Sie nicht, bis die Registrierung abgeschlossen ist, sondern fahren Sie direkt mit der nächsten Übung fort. Die Registrierung kann einige Minuten dauern.

## Übung 1: Implementieren von Infrastructure as Code (IaC) und CI/CD mit GitHub Actions und einer Bicep-Vorlage

In dieser Übung implementieren Sie IaC mit CI/CD für Azure App Service-Web-Apps mit GitHub Actions und einer Bicep-Vorlage.

> **Hinweis:** Um die erste Einrichtung zu vereinfachen, werden Sie einen Fork eines bestehenden GitHub-Repositorys verwenden, der den Quellcode der .NET-App, den GitHub Actions-Workflow und die Bicep-Vorlage enthält.

Die Übung umfasst die folgenden Aufgaben:

- Aufgabe 1: Forken und überprüfen des GitHub Repositorys, das den Quellcode einer Web-App, einen GitHub Actions Workflow und eine Bicep-Vorlage enthält
- Aufgabe 2: Einrichten der Zielumgebung
- Aufgabe 3: Überprüfen der IaC- und CI/CD-Funktionalität

### Aufgabe 1: Forken und überprüfen des GitHub Repositorys, das den Quellcode einer Web-App, einen GitHub Actions Workflow und eine Bicep-Vorlage enthält

1. Wechseln Sie zu dem Browserfenster, das Ihr GitHub-Konto anzeigt, und vergewissern Sie sich, dass Sie noch authentifiziert sind (falls nicht, melden Sie sich mithilfe Ihres GitHub-Benutzerkontos an).
1. Öffnen Sie eine andere Registerkarte im selben Browserfenster, und navigieren Sie zum [eShopOnWeb](https://github.com/MicrosoftLearning/eShopOnWeb) Repository, das die .NET-Version einer Beispielwebsite, einen GitHub Actions-Workflow und eine Bicep-Vorlage hosten.
1. Wählen Sie auf der Seite des Repositorys **eShopOnWeb** **Forken** aus.
1. Vergewissern Sie sich auf der Seite **Neuen Fork erstellen**, dass der Eintrag in der Dropdownliste **Besitzer** Ihren GitHub-Benutzernamen anzeigt, übernehmen Sie den Standardeintrag **eShopOnWeb** im Textfeld **Repositoryname**, stellen Sie sicher, dass das Kontrollkästchen **Nur Mainbranch kopieren** aktiviert ist, und wählen Sie dann **Fork erstellen** aus.

   > **Hinweis:** Ihre Browsersitzung wird automatisch zum neu erstellten geforkten Repository umgeleitet.

1. Bestätigen Sie auf der Seite **eShopOnWeb**, dass in der Dropdownliste der aktuelle Branch **main** angezeigt wird.
1. Überprüfen Sie die Repositorystruktur. Sie enthält die folgenden Komponenten:

   - das Verzeichnis **.github/workflows** mit mehreren YAML-basierten Workflows, einschließlich eines mit dem Namen **eshoponweb-cicd.yml**
   - das Verzeichnis **infra** mit der Bicep-Vorlage mit dem Namen **webapp.bicep**
   - das Verzeichnis **src/Web** mit dem .NET-Code der Quell-Web-App

1. Öffnen Sie das Verzeichnis **.github/workflows**, und wählen Sie dann die Datei **eshoponweb-cicd.yml** aus, um deren Inhalt anzuzeigen. Beachten Sie, dass dieser GitHub Actions-Workflow die Aufträge **buildandtest** und **deploy** enthält, die aus folgenden Schritten bestehen:

   - buildandtest
      - Auschecken des Repositorys
      - Vorbereiten des Runners für das gewünschte .NET-Version SDK
      - Erstellen, Testen und Veröffentlichen des .NET-Projekts
      - Hochladen der veröffentlichten Website-Codeartefakte
      - Hochladen der Bicep-Vorlage als Artefakt für den nächsten Auftrag
   - Bereitstellen
      - Herunterladen der veröffentlichten Artefakte, die in einem vorherigen Auftrag erstellt wurden
      - Herunterladen der im vorherigen Auftrag hochgeladenen Bicep-Vorlage
      - Anmelden beim Azure-Zielabonnement mithilfe eines Dienstprinzipals
      - Bereitstellen einer Azure App Service-Web-App mithilfe der Bicep-Vorlage
      - Veröffentlichen der Websiteartefakte in der Azure App Service-Web-App

   > **Hinweis:** Lassen Sie sich nicht von Details der einzelnen Aufträge ablenken. An diesem Punkt ist es viel wichtiger, einfach den Zweck des Workflows zu verstehen.

1. Beachten Sie, dass der Prozess der Bereitstellung von Azure-Ressourcen auf eine einzelne Ressourcengruppe abzielt, die zu Beginn des Workflows als Umgebungsvariable `RESOURCE-GROUP` definiert wird.

   > **Hinweis:** Sie müssen die Ressourcengruppe erstellen, bevor Sie den Workflow ausführen.

1. Beachten Sie, dass sich der Workflow auf ein Geheimnis verlässt, um sich während des Schritts „Azure CLI einrichten“ beim Ziel-Azure-Abonnement zu authentifizieren, was durch die Anweisung `creds: ${{ secrets.AZURE_CREDENTIALS }}` belegt wird.

   > **Hinweis:** Sie müssen diesen geheimen Schlüssel einrichten, bevor Sie den Workflow ausführen.

1. Beachten Sie außerdem, dass dieser Workflow so konfiguriert werden kann, dass er manuell ausgelöst wird, wie durch die Syntax `on: workflow_dispatch:` angegeben.

### Aufgabe 2: Einrichten der Zielumgebung

> **Hinweis:** Sie beginnen mit dem Erstellen der Ressourcengruppe. Sie werden den Workflow zweimal ausführen, um zwei Instanzen der Website in zwei verschiedenen Azure-Regionen bereitzustellen.

1. Wechseln Sie zur Webbrowserregisterkarte, in der das Azure-Portal unter `https://portal.azure.com` anzeigt wird.
1. Geben Sie im Azure-Portal in das Suchtextfeld oben auf der Seite **`Resource groups`** ein und wählen Sie in der Ergebnisliste **Ressourcengruppen**.
1. Wählen Sie auf der Seite **Ressourcengruppen** **+ Erstellen** aus.
1. In das Textfeld **Ressourcengruppen** geben Sie **`rg-eshoponweb-westeurope`** ein.
1. Wählen Sie in der Dropdownliste **Region** die Region **(Europa) Europa, Westen** aus.
1. Wählen Sie **Überprüfen + Erstellen** aus, und wählen Sie dann im Bereich **Überprüfen + Erstellen** **Erstellen** aus.
1. Wählen Sie auf der Seite **Ressourcengruppen** **+ Erstellen** aus.
1. In das Textfeld **Ressourcengruppen** geben Sie **`rg-eshoponweb-eastus`** ein.
1. Wählen Sie in der Dropdownliste **Region** die Region **(USA) USA, Osten** aus.
1. Wählen Sie **Überprüfen + Erstellen** aus, und wählen Sie dann im Bereich **Überprüfen + Erstellen** **Erstellen** aus.

    > **Hinweis:** Als Nächstes erstellen Sie einen Dienstprinzipal, der für die Authentifizierung vom GitHub Actions-Workflow zum Ziel-Azure-Abonnement verwendet wird, und weisen ihm die Mitwirkendenrolle im Abonnement zu.

1. Wählen Sie im Azure-Portal das Symbol **Cloud Shell** rechts neben dem Suchtextfeld aus.
1. Wenn nötig, wählen Sie oben links im Cloud Shell-Bereich im Dropdownmenü der Eintrag **Bash** aus.
1. Führen Sie innerhalb der Bash-Sitzung im Cloud Shell-Bereich den folgenden Befehl aus, um Ihre Abonnement-ID in einer Variable zu speichern:

   ```cli
   SUBSCRIPTION_ID=$(az account show --query id --output tsv) 
   echo $SUBSCRIPTION_ID
   ```

1. Kopieren Sie den Wert der Abonnement-ID, die vom zweiten Befehl zurückgegeben wird, und notieren Sie ihn. Sie werden sie später in dieser Übung benötigen.
1. Führen Sie in der Bash-Sitzung im Cloud Shell-Bereich den folgenden Befehl aus, um einen Microsoft Entra ID-Dienstprinzipal zu erstellen und ihm die Mitwirkendenrolle im Geltungsbereich des Abonnements zuzuweisen:

   ```cli
   az ad sp create-for-rbac --name "devopsfoundationslabsp" --role contributor --scopes /subscriptions/$SUBSCRIPTION_ID --json-auth
   ```

1. Kopieren Sie die gesamte JSON-formatierte Ausgabe des Befehls, und notieren Sie sie. Sie benötigen ihn in Kürze. Das Format der Ausgabe sollte dem des folgenden Textes ähneln:

   ```json
   {
     "clientId": "cccccccc-cccc-cccc-cccc-cccccccccccc",
     "clientSecret": "xxxxx~xxxxxxxxxxxx-xxxxxxxxxxxxx_xxxxxxx",
     "subscriptionId": "yyyyyyyy-yyyy-yyyy-yyyy-yyyyyyyyyyyy",
     "tenantId": "zzzzzzzz-zzzz-zzzz-zzzz-zzzzzzzzzzzz",
     "activeDirectoryEndpointUrl": "https://login.microsoftonline.com",
     "resourceManagerEndpointUrl": "https://management.azure.com/",
     "activeDirectoryGraphResourceId": "https://graph.windows.net/",
     "sqlManagementEndpointUrl": "https://management.core.windows.net:8443/",
     "galleryEndpointUrl": "https://gallery.azure.com/",
     "managementEndpointUrl": "https://management.core.windows.net/"
   }
   ```

1. Wechseln Sie zum Webbrowserfenster, in dem die Seite des geforkten GitHub-Repositorys **eShopOnWeb** angezeigt wird, und wählen Sie auf der Symbolleiste **Einstellungen** aus.
1. Wählen Sie im vertikalen Menü auf der linken Seite im Abschnitt **Sicherheit** **Geheimnisse und Variablen** aus, und wählen Sie in der Dropdownliste **Aktionen** aus.
1. Wählen Sie im Bereich **Aktionsgeheimnisse und -variablen** die Option **Neues Repositorygeheimnis** aus.
1. Geben Sie im Bereich **Aktionsgeheimnisse / Neues Geheimnis** in das Textfeld **Name** **`AZURE_CREDENTIALS`** ein.
1. Fügen Sie im Textfeld **Geheimnis** den JSON-formatierten Text ein, den Sie zuvor in dieser Aufgabe aufgezeichnet haben, und wählen Sie **Geheimnis hinzufügen** aus.
1. Wechseln Sie zurück zum Webbrowser, der die Bash-Sitzung im Bereich Cloud Shell anzeigt, und führen Sie den folgenden Befehl aus, um den Namen der ersten App Service-Web-App zu generieren, die Sie bereitstellen werden:

   ```cli
   echo devops-webapp-westeurope-$RANDOM$RANDOM
   ```

1. Kopieren Sie den vom Befehl zurückgegebenen Wert, und notieren Sie ihn. Sie werden sie später in dieser Übung verwenden.
1. Führen Sie in der Bash-Sitzung im Bereich „Cloud Shell“ den folgenden Befehl aus, um den Namen der zweiten App Service-Web-App zu generieren, die Sie bereitstellen werden:

   ```cli
   echo devops-webapp-eastus-$RANDOM$RANDOM
   ```

1. Kopieren Sie den vom Befehl zurückgegebenen Wert, und notieren Sie ihn. Sie werden sie später in dieser Übung verwenden.

### Aufgabe 3: Überprüfen der IaC- und CI/CD-Funktionalität

1. Wechseln Sie zum Webbrowserfenster, in dem die Seite des geforkten GitHub-Repositorys **eShopOnWeb** angezeigt wird, und wählen Sie ggf. auf der Seite **eShopOnWeb** die Registerkarte **Code** aus, und stellen Sie in der Dropdownliste sicher, dass aktuell der Branch **main** verwendet wird.
1. Navigieren Sie im Webbrowserfenster, in dem die Seite des Branchs **main** des geforkten GitHub-Repositorys **eShopOnWeb** angezeigt wird, zum Ordner **.github/workflows**, und wählen Sie **eshoponweb-cicd.yml** aus.
1. Wählen Sie im Bereich **.github/workflows/eshoponweb-cicd.yml** das Bleistiftsymbol aus, um den Workflow zu bearbeiten.
1. Ersetzen Sie im Bereich **Bearbeiten** Zeile 4 durch den folgenden Text:

   ```yaml
   on: workflow_dispatch
   ```

    >**Hinweis:** Stellen Sie sicher, dass Sie den richtigen Einzug verwenden.

1. Ersetzen Sie im Bereich **Bearbeiten** Zeile 8 durch den folgenden Text:

   ```yaml
    RESOURCE-GROUP: rg-eshoponweb-westeurope
   ```

1. Ersetzen Sie im Bereich **Bearbeiten** den Platzhalter `YOUR-SUBS-ID` in Zeile 11 durch den Wert der Azure Abonnement-ID, die Sie weiter oben in dieser Übung aufgezeichnet haben:
1. Ersetzen Sie im Bereich **Bearbeiten** den Platzhalter `eshoponweb-webapp-NAME` in Zeile 11 durch den Namen der **ersten** Azure App Service-Web-App, die Sie zuvor in dieser Übung generiert haben.
1. Wählen Sie im Bereich **.github/workflows/eshoponweb-cicd.yml** **Änderungen committen** aus, und wählen Sie dann erneut **Änderungen committen** aus.
1. Navigieren Sie im Webbrowserfenster, in dem die Seite des Branchs **main** des geforkten GitHub-Repositorys **eShopOnWeb** angezeigt wird, zum Ordner **infra**, und wählen Sie **webapp.bicep** aus.
1. Wählen Sie im Bereich **infra/webapp.bicep** das Bleistiftsymbol aus, um den Workflow zu bearbeiten.
1. Ersetzen Sie im Bereich **Bearbeiten** Zeile 2 durch den folgenden Text:

   ```yaml
   param sku string = 'S1' // The SKU of App Service Plan
   ```

1. Wählen Sie im Bereich **infra/webapp.bicep** **Änderungen committen** aus, und wählen Sie dann erneut **Änderungen committen** aus.
1. Wählen Sie im Webbrowserfenster, in dem die Seite des geforkten GitHub-Repositorys **eShopOnWeb** angezeigt wird, **Einstellungen** aus.
1. Wenn Sie aufgefordert werden, GitHub-Aktionen zu aktivieren, wählen Sie **Ich verstehe meine Workflows, fahren Sie fort und aktivieren Sie sie**.
    >**Hinweis:** Das ist normal, da GitHub standardmäßig zu Ihrem eigenen Schutz Workflows in einem geforketen Reposiroty deaktiviert.
1. Wählen Sie im Abschnitt **Alle Workflows** auf der linken Seite **eShopOnWeb builden und testen**aus.
1. Wählen Sie im Bereich **eShopOnWeb builden und testen** **Workflow ausführen** aus. Vergewissern Sie sich, dass in der Dropdownliste **Branch: main** ausgewählt ist, und wählen Sie erneut **Workflow ausführen** aus.

    >**Hinweis:** Dadurch sollte eine Workflowausführung ausgelöst werden.

1. Wählen Sie die Workflowausführung **eShopOnWeb builden und testen** aus.

    >**Hinweis:** Aktualisieren Sie bei Bedarf die Seite, um die neueste Workflowausführung anzuzeigen.

1. Wählen Sie im Bereich **eShopOnWeb builden und testen #1** **buildandtest**aus.
1. Überwachen Sie den Fortschritt des Workflows, und überprüfen Sie, ob alle Schritte des Auftrags **buildandtest** erfolgreich abgeschlossen wurden.
1. Nachdem dieser Auftrag abgeschlossen ist, wählen Sie im Abschnitt **Zusammenfassung** **deploy** aus.
1. Überwachen Sie den Fortschritt des Workflows, und überprüfen Sie, ob alle Schritte des Auftrags **deploy** erfolgreich abgeschlossen wurden.

    >**Hinweis:** Falls einer der Schritte fehlschlägt, wählen Sie auf derselben Seite, auf der der Workflow-Fortschritt angezeigt wird, in der oberen rechten Ecke die Option **Alle Aufträge erneut ausführen** und dann im Bereich **Alle Aufträge erneut ausführen** die Option **Aufträge erneut ausführen** aus.

1. Wechseln Sie zum Webbrowserfenster, in dem die Seite des geforkten GitHub-Repositorys **eShopOnWeb** angezeigt wird, und stellen Sie ggf. auf der Seite **eShopOnWeb** in der Dropdownliste sicher, dass aktuell der Branch **main** verwendet wird.
1. Navigieren Sie im Webbrowserfenster, in dem die Seite des Branchs **main** des geforkten GitHub-Repositorys **eShopOnWeb** angezeigt wird, zum Ordner **.github/workflows**, und wählen Sie **eshoponweb-cicd.yml** aus.
1. Wählen Sie im Bereich **.github/workflows/eshoponweb-cicd.yml** das Bleistiftsymbol aus, um den Workflow zu bearbeiten.
1. Ersetzen Sie im Bereich **Bearbeiten** Zeile 8 durch den folgenden Text:

   ```yaml
    RESOURCE-GROUP: rg-eshoponweb-eastus
   ```

1. Ersetzen Sie im Bereich **Bearbeiten** die Variable `location` in Zeile 9 durch die Region, die Ihrem Standort am nächsten liegt. 
1. Ersetzen Sie im Bereich **Bearbeiten** den Platzhalter `eshoponweb-webapp-NAME` in Zeile 11 durch den Namen der **zweiten** Azure App Service-Web-App, die Sie zuvor in dieser Übung generiert haben.
1. Wählen Sie im Bereich **.github/workflows/eshoponweb-cicd.yml** **Änderungen committen** aus, und wählen Sie dann erneut **Änderungen committen** aus.
1. Wählen Sie im Webbrowserfenster, in dem die Seite des geforkten GitHub-Repositorys **eShopOnWeb** angezeigt wird, **Einstellungen** aus.
1. Wählen Sie im Abschnitt **Alle Workflows** auf der linken Seite **eShopOnWeb builden und testen**aus.
1. Wählen Sie im Bereich **eShopOnWeb builden und testen** **Workflow ausführen** aus. Vergewissern Sie sich, dass in der Dropdownliste **Branch: main** ausgewählt ist, und wählen Sie erneut **Workflow ausführen** aus.

    >**Hinweis:** Dadurch sollte eine Workflowausführung ausgelöst werden.

1. Wählen Sie die Workflowausführung **eShopOnWeb builden und testen** aus.
1. Wählen Sie im Bereich **eShopOnWeb builden und testen #2** **buildandtest**aus.
1. Überwachen Sie den Fortschritt des Workflows, und überprüfen Sie, ob alle Schritte des Auftrags **buildandtest** erfolgreich abgeschlossen wurden.
1. Nachdem dieser Auftrag abgeschlossen ist, wählen Sie im Abschnitt **Zusammenfassung** **deploy** aus.
1. Überwachen Sie den Fortschritt des Workflows, und überprüfen Sie, ob alle Schritte des Auftrags **deploy** erfolgreich abgeschlossen wurden.

    >**Hinweis:** Falls einer der Schritte fehlschlägt, wählen Sie auf derselben Seite, auf der der Workflow-Fortschritt angezeigt wird, in der oberen rechten Ecke die Option **Alle Aufträge erneut ausführen** und dann im Bereich **Alle Aufträge erneut ausführen** die Option **Aufträge erneut ausführen** aus.

1. Geben Sie im Webbrowser-Fenster, das das Azure-Portal anzeigt, in das Suchtextfeld oben auf der Seite **`App Services`** ein und wählen Sie **App Services** in der Ergebnisliste aus.
1. Wählen Sie auf der Seite **App Services** in der Liste der App Service den App Service **devops-webapp-westeurope-** aus, den Sie zuvor in dieser Übung erstellt haben.
1. Überprüfen Sie auf der Seite **devops-webapp-westeurope-** im Abschnitt **Essentials**, ob der Wert **Standarddomäne** angezeigt wird. Wählen Sie ihn aus, um die Web-App in einer neuen Browserregisterkarte zu öffnen.
1. Überprüfen Sie in der neuen Registerkarte des Browsers, ob die Web-App angezeigt wird und ob sie funktioniert. Sie können die zweite Web-App in der Region **USA, Osten** auf die gleiche Weise überprüfen.
    >**Hinweis:** Beenden Sie die bereitgestellten Azure-Ressourcen nicht. Sie benötigen sie im nächsten Lab.
