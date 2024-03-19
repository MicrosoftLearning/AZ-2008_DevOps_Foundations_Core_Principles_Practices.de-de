---
lab:
  title: "Verbessern der Ausfallsicherheit von Workloads mit dem Traffic Manager und Testen der Ausfallsicherheit mit Azure Chaos\_Studio"
  module: Operate with DevOps
---

# Lab 4 – Verbessern der Ausfallsicherheit von Workloads mit dem Traffic Manager und Testen der Ausfallsicherheit mit Azure Chaos Studio

## Geschätzte Dauer: 35 Minuten

## Szenario

Erinnern Sie sich an das Szenario dieses Moduls, in dem Sie für ein Softwareentwicklungsunternehmen in der Einzelhandelsbranche arbeiten, das bei seiner neuen Onlineshop-Anwendung mit häufigen Ausfallzeiten und Leistungsproblemen konfrontiert ist. Da Sie sich entschieden haben, den Workload zu verbessern und Resilienz mithilfe von Traffic Manager und Azure Chaos Studio zu testen, bietet Ihnen dieses Lab die Möglichkeit, ein Traffic Manager-Profil zu implementieren und die Traffic Manager-Funktionen zu testen, eine Azure Chaos Studio-Umgebung zu konfigurieren und ein Experiment zu implementieren und zu validieren.

## Ziele

Dieses Lab deckt Folgendes ab:

- Vorbereiten des Azure-Abonnements für das Lab
- Verbessern der Workload-Resilienz mithilfe von Traffic Manager
- Testen der Workload-Resilienz mithilfe von Azure Chaos Studio
- Entfernen der in den Labs verwendeten Ressourcen

> **Hinweis:** Im vorherigen Lab haben Sie zwei Instanzen einer .NET-Web-App in zwei Azure-Regionen bereitgestellt. In diesem Lab erstellen Sie zunächst eine resiliente Konfiguration, die die Lastenausgleichsfunktionalität von Azure Traffic Manager zwischen den beiden Web-App-Instanzen implementiert. Als Nächstes verwenden Sie Azure Chaos Studio, um einen Ausfall bei einer der Apps auszulösen, um die Resilienz des Lastenausgleichs zu testen.

> **Hinweis:** Verwenden Sie für dieses Lab dasselbe GitHub-Konto, das Sie in allen vorherigen Labs verwendet haben.

## Voraussetzungen

- Abgeschlossen [Lab 01 – Agile Planung und Verwaltung mit GitHub](01-agile-planning-management-using-github.md)
- Abgeschlossen [Lab 02 – Implementieren des Arbeitsablaufs mit GitHub](02-implement-manage-repositories-using-github.md)
- Abgeschlossen [Lab 03 – Implementieren von CI/CD mit GitHub Actions und IaC mit Bicep](03-implement-ci-cd-with-github-actions-and-iac-with-bicep.md)
- Ein Azure-Abonnement, auf das Sie mindestens auf Besitzerebene zugreifen können.

## Übung 0: Vorbereiten des Azure-Abonnements für das Lab

> **Hinweis:** Wenn Sie ein neues Azure-Abonnement verwenden, sind einige der Azure-Ressourcenanbieter möglicherweise nicht registriert. Da der Registrierungsprozess einige Minuten dauern kann, registrieren Sie sie zu Beginn des Labs, um die Wartezeit zu minimieren.

> **Hinweis:** In diesem Lab verwenden Sie Azure Chaos Studio. Wenn Sie Azure Chaos Studio zum ersten Mal in Ihrem Abonnement implementieren, müssen Sie zuerst den entsprechenden Ressourcenanbieter registrieren.

1. Öffnen Sie einen Webbrowser, und navigieren Sie zum Azure-Portal unter `https://portal.azure.com`.
1. Wenn Sie dazu aufgefordert werden, melden Sie sich mit Ihrem Microsoft Entra ID-Konto an, das Besitzerzugriff auf das Azure-Abonnement hat, das Sie in der vorherigen Übung verwendet haben.
1. Geben Sie in der Registerkarte des Webbrowsers, in der das Azure-Portal geöffnet ist, in das Suchfeld oben auf der Seite **Abonnements** ein und wählen Sie in der Ergebnisliste **Abonnements** aus.
1. Wählen Sie auf der Seite „Abonnements“ im vertikalen Menü auf der linken Seite **Ressourcenanbieter** aus.
1. Suchen Sie in der Liste der Ressourcenanbieter nach **Microsoft.Chaos**, und wählen Sie sie aus.
1. Wenn Sie den Ressourcenanbieter **Microsoft.Chaos** ausgewählt haben, wählen Sie in der Symbolleiste **Registrieren** aus.

   > **Hinweis:** Warten Sie nicht, bis die Registrierung abgeschlossen ist, sondern fahren Sie direkt mit der nächsten Übung fort. Die Registrierung kann einige Minuten dauern.

## Übung 1: Verbessern der Workload-Resilienz mithilfe von Traffic Manager

In dieser Übung werden Sie eine belastbare Konfiguration implementieren, die Anfragen zwischen den beiden .NET-Web-App-Instanzen in zwei verschiedenen Azure-Regionen mit Hilfe von Azure Traffic Manager verteilt.

> **Hinweis:** Für den Zweck unseres Labs betrachten wir die Bereitstellung der .NET-basierten Webanwendung eShopOnWeb in der Region USA, Osten als die primäre Instanz. Während diese Überlegung in diesem Fall rein willkürlich ist (und nur zu Demonstrationszwecken dient), könnte es Szenarien geben, in denen es vorteilhaft sein könnte, einen der Endpunkte zu priorisieren.

Die Übung umfasst die folgenden Aufgaben:

- Aufgabe 1: Implementieren eines Traffic Manager-Profils
- Aufgabe 2: Überprüfen der Funktion von Traffic Manager

### Aufgabe 1: Implementieren eines Traffic Manager-Profils

1. Geben Sie in der Registerkarte des Webbrowsers, in der das Azure-Portal geöffnet ist, in das Suchfeld oben auf der Seite **Traffic Manager-Profile** ein und wählen Sie in der Ergebnisliste **Traffic Manager-Profile** aus.
1. Wählen Sie auf der Seite **Lastenausgleich \| Traffic Manager ** **+ Erstellen** aus.
1. Führen Sie auf der Seite **Traffic Manager-Profil erstellen** die folgenden Aktionen durch:

   - Geben Sie im Textfeld **Name** **devopsfoundationstmprofile** ein.

       > **Hinweis:** Der Name des Traffic Manager-Profils muss global eindeutig sein. Wenn Sie eine Fehlermeldung erhalten, die angibt, dass der Name bereits verwendet wird, probieren Sie einen anderen Namen aus, und notieren Sie ihn. Sie benötigen ihn in diesem Lab.

   - Wählen Sie in der Dropdownliste **Routingmethode** die Option **Priorität** aus.

   > **Hinweis:** Wir haben die Methode des Prioritätsroutings gewählt, um die etwas willkürliche Annahme widerzuspiegeln, dass alle Anfragen von der Azure App Service Web-App in der Region USA, Osten verarbeitet werden sollen.

   - Stellen Sie sicher, dass Ihr neues Azure-Abonnement wird in der Dropdownliste **Abonnement** angezeigt wird.
   - Wählen Sie den Link **Neue erstellen** unter der Dropdownliste **Ressourcengruppe**. Geben Sie im Textfeld **Name** **devops-foundations-rg** ein, und wählen Sie dann **OK** aus.
   - Wählen Sie in der Dropdownliste **Ort der Ressourcengruppe** die gleiche Azure-Region aus, die Sie in den vorherigen Labs dieses Kurses ausgewählt haben.

1. Wählen Sie **Erstellen** aus, um den Bereitstellungsprozess zu starten.

   > **Hinweis**: Warten Sie, bis die Bereitstellung abgeschlossen ist. Dieser Vorgang sollte nicht länger als 1 Minute dauern.

1. Wählen Sie auf der Seite **Lastenausgleich \| Traffic Manager** ggf. **Aktualisieren** aus, und wählen Sie dann **devopsfoundationstmprofile** aus.
1. Kopieren Sie auf der Seite **devopsfoundationstmprofile** im Abschnitt **Essentials** den Wert der Einstellung **DNS-Namens**, und notieren Sie ihn. Sie benötigen ihn in diesem Lab.
1. Wählen Sie auf der Seite **devopsfoundationstmprofile** im linken Navigationsmenü im Abschnitt **Einstellungen** **Konfiguration** aus.
1. Sehen Sie sich den Inhalt der Seite **devopsfoundationstmprofile \| Konfiguration** an. Beachten Sie, dass **DNS-Zeit bis Live (TTL)** standardmäßig auf **60** Sekunden festgelegt ist. Ändern Sie den Wert auf **5** Sekunden.

   > **Hinweis:** Wenn Sie den Wert von TTL verringern, wird die Failoverzeit beschleunigt. 

   > **Hinweis:** Neben dem TTL-Wert beeinflussen auch das Intervall der Integritätstests, die Zeitüberschreitung der Tests und die tolerierte Anzahl von Ausfällen die Zeitspanne, in der der Ausfall eines Endpunkts erkannt wird.

1. Wählen Sie auf der Seite **devopsfoundationstmprofile \| Konfiguration** in der Dropdownliste **Protokoll** **HTTPS** aus, und geben Sie im Textfeld **Port** **443** ein.

   > **Hinweis:** Beide Azure App Service Web-Apps, die als Endpunkte konfiguriert werden, sind über HTTPS auf dem Standard-TCP-Port 443 zugänglich.

1. Wählen Sie auf der Seite **devopsfoundationstmprofile \| Konfiguration** **Speichern** aus.
1. Wählen Sie auf der Seite **devopsfoundationstmprofile** im linken Navigationsmenü im Abschnitt **Einstellungen** **Endpunkte** aus.
1. Wählen Sie auf der Seite **devopsfoundationstmprofile \|Endpunkte**** **+ Hinzufügen** aus.

   > **Hinweis:** Sie fügen zuerst einen Endpunkt für die Azure App Service-Web-App hinzu.

1. Führen Sie im Bereich **Endpunkt hinzufügen** die folgenden Aktionen aus:

   - Stellen Sie sicher, dass **Azure-Endpunkt** in der Dropdownliste **Typ** angezeigt wird.
   - Geben Sie im Textfeld **Name** **DevOps Foundations-Web-App – USA, Osten**ein.
   - Stellen Sie sicher, dass das Kontrollkästchen **Endpunkt aktivieren** aktiviert ist.
   - Wählen Sie in der Dropdownliste **Zielressourcengruppe** die Option **App Service** aus.
   - Wählen Sie in der Dropdownliste **Zielressource** im Abschnitt **rg-eshoponweb-eastus** den Namen der App Service-Web-App in der Azure-Region USA, Osten aus.
   - Geben Sie im Textfeld **Priorität** **1** ein.

   > **Hinweis:** Je niedriger der Zahlenwert, desto höher die Priorität. In diesem Fall werden alle Anfragen an die Web-App-Instanz in der Region USA, Osten gesendet, solange diese Instanz verfügbar ist.

   - Stellen Sie sicher, dass die **Integritätsprüfungen** aktiviert sind.

1. Wählen Sie **Hinzufügen** aus.

1. Wählen Sie auf der Seite **devopsfoundationstmprofile \|Endpunkte**** erneut **+ Hinzufügen** aus.

   > **Hinweis:** Als Nächstes fügen Sie einen Endpunkt hinzu, der die andere Azure App Service-Web-App repräsentiert, die in der Azure-Region Europa, Westen eingesetzt wird.

1. Führen Sie im Bereich **Endpunkt hinzufügen** die folgenden Aktionen aus:

   - Stellen Sie sicher, dass **Azure-Endpunkt** in der Dropdownliste **Typ** angezeigt wird.
   - Geben Sie im Textfeld **Name** **DevOps Foundations-Web-App – Europa, Westen**ein.
   - Stellen Sie sicher, dass das Kontrollkästchen **Endpunkt aktivieren** aktiviert ist.
   - Wählen Sie in der Dropdownliste **Zielressourcengruppe** die Option **App Service** aus.
   - Wählen Sie in der Dropdownliste **Zielressource** im Abschnitt **rg-eshoponweb-westeurope** den Namen der App Service-Web-App in der Azure-Region Europa, Westen aus.
   - Geben Sie im Textfeld **Priorität** **2** ein.
   - Stellen Sie sicher, dass die **Integritätsprüfungen** aktiviert sind.

1. Wählen Sie **Hinzufügen** aus.
1. Stellen Sie auf der Seite **devopsfoundationstmprofile \| Endpoints**** erneut sicher, dass beide Endpunkte mit dem Eintrag **Online** in der Spalte **Statusüberwachung** aufgelistet sind.

   > **Hinweis:** Möglicherweise müssen Sie einige Minuten warten und **Aktualisieren** auswählen, bevor der Statuseintrag auf dem neuesten Stand ist.

### Aufgabe 2: Überprüfen der Funktion von Traffic Manager

1. Öffnen Sie ein Webbrowserfenster, und navigieren Sie zum DNS-Namen, der dem Traffic Manager-Profil zugeordnet ist.
1. Stellen Sie sicher, dass der Webbrowser die vertraute Seite der Website anzeigt, die von der Azure App Service-Web-App gehostet wird, die Sie im vorherigen Lab bereitgestellt haben.
1. Schließen Sie das neu geöffnete Webbrowserfenster.
1. Gehen Sie zurück zum Webbrowserfenster, in dem das Azure-Portal angezeigt wird.
1. Wählen Sie im Azure-Portal das Symbol **Azure Cloud Shell** rechts neben dem Suchtextfeld aus.
1. Wenn nötig, wählen Sie oben links im Cloud Shell-Bereich im Dropdownmenü der Eintrag **Bash** aus.
1. Führen Sie in der Bash-Sitzung im Bereich Cloud Shell den folgenden Befehl aus, um den DNS-Namen des Traffic Manager-Profils, das Sie in der vorherigen Aufgabe erstellt haben, in einen der beiden entsprechenden Endpunkte aufzulösen (ersetzen Sie den Platzhalter `<tm_profile>` durch den DNS-Namen des Traffic Manager-Profils, z. B. `devopsfoundationstmprofile.trafficmanager.net`), aber stellen Sie sicher, dass Sie das führende `https:\\` entfernen:

   ```cli
   nslookup <tm_profile>
   ```

1. Führen Sie denselben Befehl ein paar Mal aus und warten Sie zwischen den einzelnen Aufrufen jeweils etwas mehr als 5 Sekunden (um die Möglichkeit des DNS-Cachings auszuschließen). Beachten Sie, dass die Ausgabe in jedem Fall auf den DNS-Namen der Web-App in der Azure-Region USA, Osten verweist.

## Übung 2: Testen der Workload-Resilienz mithilfe von Azure Chaos Studio

In dieser Übung testen Sie die Workload-Resilienz mithilfe von Azure Chaos Studio.

> **Hinweis:** In dieser Übung wird die Verwendung von Azure Chaos Studio veranschaulicht. Azure Chaos Studio dient dazu, die Widerstandsfähigkeit von Anwendungen und Diensten zu messen, zu verstehen und aufzubauen. Dies wird durch die absichtliche Unterbrechung von Workloads erreicht, um Ausfallsicherheitslücken zu identifizieren und entsprechende Abhilfemaßnahmen proaktiv statt reaktiv zu implementieren.

Die Übung umfasst die folgenden Aufgaben:

- Aufgabe 1: Konfigurieren der Azure Chaos Studio-Umgebung
- Aufgabe 2: Implementieren eines Experiments
- Aufgabe 3: Überprüfen des Experiments

### Aufgabe 1: Konfigurieren der Azure Chaos Studio-Umgebung

1. Geben Sie in der Registerkarte des Webbrowsers, in der das Azure-Portal geöffnet ist, in das Suchfeld oben auf der Seite **Chaos Studio** ein und wählen Sie in der Ergebnisliste **Chaos Studio** aus.
1. Wählen Sie auf der Seite **Chaos Studio** **Ziele** aus.
1. Wählen Sie auf der Seite **Chaos Studio \| Ziele** die Azure App Service Web App-Instanz in der Ressourcengruppe **rg-eshoponweb-eastus** in der Region USA, Osten aus, die Sie im vorherigen Lab bereitgestellt haben.
1. Wählen Sie in der Symbolleiste den Header der Dropdownliste **Ziele aktivieren** aus, und wählen Sie in der Dropdownliste **Dienst-direkte Ziele aktivieren (Alle Ressourcen)** aus.
1. Stellen Sie auf der Seite **Dienst-direkte Ziele aktivieren** sicher, dass die richtige App-Web-App-Instanz von App Service aufgeführt ist, wählen Sie **Überprüfen + Aktivieren** aus, und wählen Sie dann **Aktivieren** aus.

   > **Hinweis:** Warten Sie, bis das Ziel aktiviert ist. Das sollte weniger als eine Minute dauern.

### Aufgabe 2: Implementieren eines Experiments

1. Geben Sie im Azure-Portal in das Suchfeld oben auf der Seite **Chaos Studio** ein und wählen Sie in der Ergebnisliste **Chaos Studio** aus.
1. Wählen Sie auf der Seite **Chaos Studio** **Experimente** aus.
1. Wählen Sie auf der Seite **Experimente** **+ Erstellen** aus, und wählen Sie dann in der Dropdownliste **Neues Experiment** aus.
1. Führen Sie auf der Registerkarte **Grundlagen** der Seite **Experiment erstellen** die folgenden Aktionen aus:

   - Stellen Sie sicher, dass Ihr Azure-Abonnement wird in der Dropdownliste **Abonnement** angezeigt wird.
   - Wählen Sie den Link **Neue erstellen** unter der Dropdownliste **Ressourcengruppe**. Geben Sie im Textfeld **Name** **devops-foundations-rg** ein, und wählen Sie dann **OK** aus.
   - Geben Sie im Abschnitt **Experimentdetails** im Textfeld **Name** **DevOps_Foundations_Labs_Experiment_01** ein.
   - Wählen Sie aus der Dropdownliste **Region** die Azure-Region **Europa, Westen** aus.

   > **Hinweis:** Sie könnten jede beliebige Azure-Region wählen, aber da Sie Ausfälle bei einer Ressource in der Region Europa, Westen testen, scheint eine andere Region als USA, Osten angemessener.

1. Wählen Sie auf der Registerkarte **Grundeinstellungen** der Seite **Experiment erstellen** die Option **Weiter: Berechtigungen >** aus.
1. Übernehmen Sie auf der Registerkarte **Berechtigungen** die Standardoption **Systemseitig zugewiesene Identität**, und wählen Sie dann **Weiter: Experiment-Designer >** aus.
1. Führen Sie auf der Registerkarte **Experiment-Designer** die folgenden Aktionen aus:

   - Geben Sie im Textfeld **Schritt** **Schritt 1: Failover für eine App Service-Web-App** ein.
   - Geben Sie im Textfeld **Branch** **Branch 1: Emulieren eines App Service-Ausfalls** ein.
   - Wählen Sie **+ Aktion hinzufügen** aus, und wählen Sie in der Dropdownliste **Ausfall hinzufügen** aus.

1. Wählen Sie auf der Registerkarte **Ausfalldetails** im Bereich **Ausfall hinzufügen** in der Dropdownliste **Ausfalloptionen** **Beenden von App Service** aus, und legen Sie den Wert **Dauer (Minuten)** auf 10 fest.
1. Wählen Sie auf der Registerkarte **Ausfalldetails** im Bereich **Ausfall hinzufügen** die Option **Weiter: Zielressourcen >** aus.
1. Stellen Sie auf der Registerkarte **Zielressourcen** des Bereichs **Ausfall hinzufügen** sicher, dass die Option **Manuell aus einer Liste auswählen** ausgewählt ist. Wählen Sie die App Service-Web-App aus, die Sie als Ziel der Chaos Studio-Umgebung hinzugefügt haben, und wählen Sie dann **Hinzufügen** aus.
1. Wählen Sie auf der Registerkarte **Experiment-Designer** der Seite **Experiment erstellen** **Überprüfen + Erstellen** aus.
1. Wählen Sie auf der Registerkarte **Überprüfen + Erstellen** der Seite **Experiment erstellen** **Erstellen** aus.

   > **Hinweis:** Warten Sie, bis das Experiment erstellt ist. Das sollte weniger als eine Minute dauern.

   > **Hinweis:** Damit das Experiment erfolgreich ist, müssen Sie dem neu erstellten verwalteten Dienstkonto auch ausreichende Berechtigungen zum Anhalten der Azure App Service-Web-App erteilen. Wir werden zu diesem Zweck die in Azure vorhandene Mitwirkendenrolle verwenden, aber Sie können auch eine benutzerdefinierte Rolle erstellen, wenn Sie dem Prinzip des geringsten Privilegs folgen möchten.

1. Geben Sie im Azure-Portal oben auf der Seite im Suchtextfeld **App Services** ein, und wählen Sie in der Ergebnisliste **App Services** aus.
1. Wählen Sie auf der Seite **App Services** die Azure App Service-Web-App in der Region USA, Osten aus, die Sie im vorherigen Lab bereitgestellt haben.
1. Wählen Sie auf dem Blatt für das Speicherkonto im vertikalen Menü links **Zugriffssteuerung (IAM)** aus.
1. Wählen Sie auf der Seite **Zugriffssteuerung (IAM)** der Web-App **+ Hinzufügen** und dann in der Dropdownliste die Option **Rollenzuweisung hinzufügen** aus.
1. Wählen Sie auf der Registerkarte **Rolle** der Seite **Rollenzuweisung hinzufügen** **Privilegierte Administratorrollen** aus. Wählen Sie aus der Liste der Rollen **Mitwirkender** und dann **Weiter** aus.
1. Wählen Sie auf der Registerkarte **Mitglieder** der Seite **Rollenzuweisung hinzufügen** für die Option **Zugewiesener Zugriff auf** **Verwaltete Identität** aus, und wählen Sie dann **+ Mitglieder auswählen** aus.
1. Wählen Sie im Bereich **Verwaltete Identitäten auswählen** in der Dropdownliste **Verwaltete Identitäten** **Chaos Experiment**aus. Wählen Sie in der Liste der verwalteten Identitäten die zuvor in dieser Aufgabe erstellte Identität aus, und wählen Sie dann **Auswählen** aus.
1. Wählen Sie auf der Registerkarte **Mitglieder** der Seite **Rollenzuweisung hinzufügen** die Option **Überprüfen + Zuweisen** und anschließend erneut die Option **Überprüfen + Zuweisen** aus.

### Aufgabe 3: Überprüfen des Experiments

1. Navigieren Sie im Azure-Portal zurück zur Seite **Chaos Studio**, und wählen Sie im vertikalen Menü auf der linken Seite **Experimente** aus.
1. Wählen Sie aus der Liste der Experimente **DevOps_Foundations_Labs_Experiment_01** aus.
1. Wählen Sie auf der Seite **DevOps_Foundations_Labs_Experiment_01** **Starten** aus, und wählen Sie zur Bestätigung **OK** aus.
1. Warten Sie, bis sich der Status des Experiments in **Wird ausgeführt** ändert.
1. Wählen Sie auf der Seite **DevOps_Foundations_Labs_Experiment_01** im Abschnitt **Verlauf** bei dem Eintrag der aktuellen Experimentausführung den Link **Details** aus.
1. Überprüfen Sie auf der Detailseite des Experiments **DevOps_Foundations_Labs_Experiment_01** den Eintrag, der den Schritt, den Branch und die Aktion enthält, die dem Experiment zugeordnet ist.

   > **Hinweis:** Möglicherweise müssen Sie in der Symbolleiste **Aktualisieren** wählen, um den Status des Eintrags zu aktualisieren.

1. Wählen Sie den Eintrag **Aktion** aus, um den Bereich **Ausfalldetails** anzuzeigen.
1. Wählen Sie im Abschnitt **Ausgeführte Ziele** den Eintrag der Azure App Service-Web-App aus.
1. Beachten Sie auf der Webseite im Abschnitt **Essentials**, dass der Status der Web-App als **Beendet**aufgeführt wird.

   > **Hinweis:** Möglicherweise müssen Sie in der Symbolleiste **Aktualisieren** wählen, um den Status der Web-App zu aktualisieren.

   > **Hinweis:** Lassen Sie uns nun überprüfen, ob Traffic Manager Anforderungen, die auf sein Profil abzielen, erfolgreich an den Endpunkt der App Service Web-App in der Region Europa, Westen umleitet.

1. Öffnen Sie einen Webbrowser und navigieren Sie zum DNS-Namen, der dem Traffic Manager-Profil zugeordnet ist.
1. Stellen Sie sicher, dass der Webbrowser die vertraute Seite der Website anzeigt, die von der Azure App Service-Web-App gehostet wird, die Sie im vorherigen Lab bereitgestellt haben.
1. Gehen Sie zurück zum Webbrowserfenster, in dem das Azure-Portal angezeigt wird.
1. Wählen Sie im Azure-Portal das Symbol **Azure Cloud Shell** rechts neben dem Suchtextfeld aus.
1. Führen Sie in der Bash-Sitzung im Bereich Cloud Shell den folgenden Befehl aus, um den DNS-Namen des Traffic Manager-Profils, das Sie in der vorherigen Übung erstellt haben, in einen der beiden entsprechenden Endpunkte aufzulösen (ersetzen Sie den Platzhalter `<tm_profile>` durch den DNS-Namen des Traffic Manager-Profils, aber stellen Sie sicher, dass Sie das führende `https:\\` entfernen):

   ```cli
   nslookup <tm_profile>
   ```

1. Führen Sie denselben Befehl ein paar Mal aus und warten Sie zwischen den einzelnen Aufrufen jeweils etwas mehr als 5 Sekunden (um die Möglichkeit des DNS-Cachings auszuschließen). Überprüfen Sie, dass die Ausgabe in jedem Fall auf den DNS-Namen der Web-App in der Azure-Region Europa, Westen verweist.

> **Hinweis:** Dieses Beispiel mag zwar etwas simpel erscheinen, aber das Hauptziel der Übung war es, den Prozess der Implementierung von Azure Chaos Studio-basierten Tests und deren Hauptkomponenten zu veranschaulichen. Sie würden den gleichen Ansatz verwenden, wenn eine Azure App Service-Web-App Teil einer komplexeren Umgebung wäre, in der die Auswirkungen eines Ausfalls viel schwieriger vorherzusagen wären.

### Übung 3: Entfernen der in den Labs verwendeten Ressourcen

In dieser Übung entfernen Sie die in den Labs verwendeten Ressourcen.

1. Geben Sie in der Registerkarte des Webbrowsers, in der das Azure-Portal geöffnet ist, in das Suchfeld oben auf der Seite **Ressourcengruppen** ein, und wählen Sie in der Ergebnisliste **Ressourcengruppen**aus.
1. Wählen Sie auf der Seite **Ressourcengruppen** in der Liste der Ressourcengruppen die von Ihnen in diesem Lab erstellte Ressourcengruppe aus.
1. Wählen Sie auf der Ressourcengruppenseite die Option **Ressourcengruppe löschen** aus.
1. Geben Sie im Textfeld **Ressourcengruppennamen eingeben, um den Löschvorgang zu bestätigen** den Namen der Ressourcengruppe ein, die Sie löschen wollen, und wählen Sie **Löschen** aus.

   > **Hinweis:** Warten Sie, bis die Ressourcengruppe gelöscht wurde. Das sollte weniger als eine Minute dauern.

1. Wiederholen Sie den vorherigen Schritt für die Ressourcengruppe, die Sie in den vorherigen Übungen dieses Kurses erstellt haben.

    > **Hinweis:** Durch das Löschen der Ressourcengruppe werden alle damit verbundenen Ressourcen entfernt, einschließlich des Traffic Manager-Profils und der Azure Chaos Studio-Umgebung.

    > **Hinweis:** Das GitHub-Konto und die Repositorys, die Sie in den vorherigen Labs dieses Kurses erstellt haben, müssen nicht gelöscht werden. Sie können sie weiterhin für Ihr Arbeitsportfolio verwenden.
