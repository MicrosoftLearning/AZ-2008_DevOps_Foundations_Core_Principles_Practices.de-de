---
lab:
  title: Agile Planung und Verwaltung mit GitHub
  module: Plan with DevOps
---

# Lab 01 – Agile Planung und Verwaltung mit GitHub

## Geschätzte Zeitdauer: 40 Minuten

## Szenario

Erinnern Sie sich an das Szenario dieses Moduls, in dem Sie für ein Softwareentwicklungsunternehmen in der Einzelhandelsbranche arbeiten, das plant, seinen Online-Store zu einer neuen App zu migrieren, aber aufgrund der mangelnden Zusammenarbeit und Kommunikation zwischen den Entwicklungs- und Betriebsteams Schwierigkeiten bei der Planung des Projekts hat. Da Sie sich für die Verwendung von GitHub für eine agile Planung und Verwaltung entschieden haben, bietet Ihnen dieses Lab die Möglichkeit, ein GitHub-Repository, zugeordnete Meilensteine und Issues, ein Projekt und ein Projektboard zu erstellen. Darüber hinaus können Sie basierend auf einem Issue dem Projektboard und einem Element ein Entwurfselement hinzufügen und die Automatisierungseinstellungen überprüfen.

## Ziele

Dieses Lab deckt Folgendes ab:

- Erstellen eines GitHub-Repositorys, Projekts und Projektboards
- Erstellen und Verwalten von Projektboard-Elementen

> **Hinweis:** Für diese und nachfolgende Labs benötigen Sie ein GitHub-Konto. Es wird dringend empfohlen, ein separates Konto zu erstellen, das Sie für die Labs verwenden. Zum Erstellen eines GitHub-Kontos folgen Sie den Anweisungen im Artikel [Erstellen eines Kontos auf GitHub](https://docs.github.com/get-started/quickstart/creating-an-account-on-github).

## Übung 1: Erstellen eines GitHub-Repositorys, Projekts und Projektboards

In dieser Übung erstellen Sie ein GitHub-Repository, ein Projekt und ein Projektboard.

> **Hinweis:** Laut GitHub-Dokumentation bieten Projekte *ein anpassungsfähiges, flexibles Werkzeug für die Planung und Verfolgung der Arbeit auf GitHub*. Projekte bieten Zugriff auf *Boards*, die als *Kanban*-Boards dienen. Kanban ist ein gängiges und oft verwendetes Framework in einer Agile-Umgebung, um den aktuellen Stand der Arbeit an einem Projekt darzustellen.

Die Übung umfasst die folgenden Aufgaben:

- Aufgabe 1: Erstellen eines GitHub-Repositorys
- Aufgabe 2: Erstellen von GitHub-Meilensteinen und -Issues
- Aufgabe 3: Erstellen eines GitHub-Projekts
- Aufgabe 4: Erstellen eines GitHub-Projektboards

### Aufgabe 1: Erstellen eines GitHub-Repositorys

1. Öffnen Sie einen Webbrowser und navigieren Sie zur [GitHub](https://github.com)-Startseite.
1. Wenn Sie zur Authentifizierung aufgefordert werden, melden Sie sich mit Ihrem GitHub-Benutzerkonto an.
1. Wählen Sie auf der GitHub-Homepage die Registerkarte **Repositorys** und dann die Option **Neu** aus.
1. Führen Sie auf der Seite **Neues Repository erstellen** die folgenden Aktionen aus:

   - Wählen Sie in der Dropdownliste **Besitzer** Ihren GitHub-Benutzernamen aus.
   - Geben Sie in das Textfeld **Repositoryname** **`DevOpsCoreIntroRepo`** ein.
   - Ändern Sie die Sichtbarkeit des Repositorys auf **Privat**.
   - Aktivieren Sie das Kontrollkästchen **README-Datei hinzufügen**.
   - Wählen Sie in der Dropdownliste **.gitignore hinzufügen** **Visual Studio**aus.

   > **Hinweis:** Weitere Informationen zu gitignore finden Sie unter [Ignorieren von Dateien](https://docs.github.com/get-started/getting-started-with-git/ignoring-files).

   - Wählen Sie in der Dropdownliste **Lizenz auswählen** **MIT** aus.

   > **Hinweis:** Weitere Informationen zur Auswahl von Lizenzen finden Sie unter [Lizenzierung eines Repositorys](https://docs.github.com/repositories/managing-your-repositorys-settings-and-features/customizing-your-repository/licensing-a-repository).

1. Klicken Sie auf **Create repository** (Repository erstellen).

### Aufgabe 2: Erstellen von GitHub-Meilensteinen und -Issues

1. Wählen Sie auf der Seite **DevOpsCoreIntroRepo** die Registerkarte **Issues** aus.

   > **Hinweis:** Sie erstellen einen Meilenstein und ein Problem im Zusammenhang mit dem neu erstellten Repository, das Sie später bei der Arbeit mit dem Projektboard verwenden werden.

1. Wählen Sie links auf der Seite **Issues** neben der Schaltfläche **Neues Issue** **Meilensteine** aus.
1. Wählen Sie auf der Seite **Meilensteine** **Neuer Meilenstein** aus.
1. Führen Sie auf der Seite **Neuer Meilenstein** die folgenden Schritte aus:

   - Geben Sie **`alpha release`** in das Textfeld **Titel** ein.
   - Geben Sie im Textfeld **Fälligkeitsdatum (optional)** das Datum ein, das eine Woche nach dem aktuellen Datum liegt.
   - In das Textfeld **Beschreibung** geben Sie **`Completion of the alpha release`** ein.

1. Wählen Sie **Neuer Meilenstein** aus.
1. Wiederholen Sie die letzten drei Schritte, um einen Meilenstein für die **Beta-Release** mit dem Fälligkeitsdatum zwei Wochen nach dem aktuellen Datum zu erstellen. In das Textfeld **Beschreibung** geben Sie **`Completion of the beta release`** ein.
1. Navigieren Sie zurück zur Seite **Issues** und wählen Sie **Neues Issue** aus.
1. Geben Sie in das Textfeld **Titel hinzufügen** **`Repo README page is empty`** ein.
1. In das Textfeld **Beschreibung hinzufügen** geben Sie **`Brevity might be a virtue, but this README page can really use some text`** ein.
1. Wählen Sie das Zahnradsymbol neben dem Eintrag **Meilenstein** aus, und wählen Sie in der Dropdownliste **Alpha-Release** aus.
1. Wählen Sie das Zahnradsymbol neben dem Eintrag **Bezeichnungen** aus, und wählen Sie in der Dropdownliste **Fehler** aus.
1. Klicken Sie auf **Erstellen**. Beachten Sie, dass dem Issue automatisch **#1** zugewiesen wurde.

### Aufgabe 3: Erstellen eines GitHub-Projekts

1. Wählen Sie auf der GitHub-Hauptseite auf der rechten Seite der Symbolleiste das Avatarsymbol aus, und wählen Sie im Dropdownmenü **Ihre Projekte** aus.
1. Wählen Sie **Neues Projekt** aus.
1. Wählen Sie im Bereich **Vorlage auswählen** **Teamplanung** aus, und wählen Sie dann **Projekt erstellen** aus.  

   > **Hinweis:** Sie können aber auch ganz von vorne anfangen und das Projekt im Tabellen-, Board- oder Roadmap-Format anzeigen.

1. Wählen Sie auf der Seite „Neues Projekt“ den automatisch generierten Projektnamen. Dadurch wird automatisch die Seite **Projekteinstellungen** angezeigt.
1. Geben Sie in das Textfeld **Projektname** **`DevOps Core Intro Project`** ein.
1. In das Textfeld **Kurzbeschreibung** geben Sie **`Introduction to GitHub Projects`** ein und wählen **Speichern**.
1. Geben Sie im Bereich **README** den folgenden Text ein

   > **Hinweis:** Im Abschnitt **README** finden Sie einen vereinfachten Markdown-Editor, mit dem Sie eine visuell ansprechende README-Seite für das Projekt erstellen können. Sie können die Symbole in der Symbolleiste verwenden, um den Text zu formatieren und die Änderungen in der Registerkarte **Vorschau** überprüfen. Kopieren Sie den folgenden Text, und fügen Sie ihn in den README-Editor ein:

   ```text
   ### Welcome to DevOps Core Intro Project ###

   **Projects are a customizable, flexible tool for planning and tracking your work.**

   To find out more, refer to GitHub documentation [about Projects](https://docs.github.com/issues/planning-and-tracking-with-projects/learning-about-projects/about-projects).
   ```

1. Wählen Sie **Vorschau** aus, um sich die resultierende Seite anzusehen.
1. Wählen Sie **Speichern**.
1. Scrollen Sie nach unten zur **Gefahrenzone** und **sehen Sie sich** die Option an, mit der Sie die Sichtbarkeit des Projekts zwischen **Privat** und **Öffentlich** ändern, das Projekt schließen und löschen können.

   > **Hinweis:** Schließen oder löschen Sie das Projekt jetzt nicht. Sehen Sie sich nur an, welche Optionen Sie haben.

1. Wenn Sie nach oben scrollen, werden Sie feststellen, dass Sie die Möglichkeit haben, den **Zugriff auf das Projekt zu verwalten** und benutzerdefinierte Felder in der Projektschnittstelle zu erstellen.
1. Wählen Sie im Feld **<- Einstellungen** den Pfeil nach links, um die Seite **Einstellungen** zu verlassen.

### Aufgabe 4: Erstellen eines GitHub-Projektboards

1. Wählen Sie auf der Seite **DevOps Core Intro-Projekt** das nach unten zeigende Caret neben der Registerkarte **Backlog** und wählen Sie im Abschnitt **Layout** die Option **Board**.

   > **Hinweis:** Mit dieser Option können Sie ganz einfach zwischen dem Tabellen-, Board- und Roadmap-Format wechseln.

1. Sehen Sie sich die resultierende Seite an, die aus drei vordefinierten Spalten besteht, die beschriftet sind:

   - **ToDo** – Hier finden Sie Aufgaben, die noch nicht begonnen wurden.
   - **In Bearbeitung** – Hier finden Sie Aufgaben, die aktuell bearbeitet werden.
   - **Erledigt** – Hier finden Sie abgeschlossene Aufgaben.

   > **Hinweis:** Dieses Layout stellt ein sehr einfaches Kanban-Board dar. In jeder Spalte können Sie einzelne Aufgaben hinzufügen. Außerdem können Sie neue Spalten hinzufügen.

1. Um eine zusätzliche Spalte hinzuzufügen, wählen Sie das Symbol **+** rechts neben der Spalte **Erledigt** und dann **+ Neue Spalte**.
1. Geben Sie im Fenster **Neue Option** in das Textfeld **Beschriftungstext** **`Review In Progress`** ein und wählen Sie eine Farbe, die Sie der Spalte zuweisen möchten. Geben Sie in das Textfeld **Beschreibung** **`This item is being reviewed`** ein und wählen Sie dann **Speichern**.
1. Wählen Sie den kleinen Kreis neben der Beschriftung der neu hinzugefügten Spalte **In Review** aus, und ziehen Sie ihn zwischen die Spalten **In Bearbeitung** und **Erledigt**.

## Übung 2: Erstellen und Verwalten von Projektboard-Elementen

In dieser Übung erstellen und verwalten Sie Aufgaben auf dem Projektboard

> **Hinweis:** Es gibt zwei grundlegende Möglichkeiten, um Aufgaben zu einem Projektboard hinzuzufügen. Sie können einen Entwurf für eine Aufgabe erstellen oder eine Aufgabe für ein vorhandenes Issue in einem GitHub-Repository hinzufügen.

Die Übung umfasst die folgenden Aufgaben:

- Aufgabe 1: Hinzufügen eines Aufgabenentwurfs
- Aufgabe 2: Hinzufügen einer Aufgabe für ein Issue
- Aufgabe 3: Überprüfen der Automatisierungseinstellungen

### Aufgabe 1: Hinzufügen eines Aufgabenentwurfs

1. Wählen Sie auf der Seite **DevOps Core Intro-Projekt** in der Spalte **ToDo** **+ Aufgabe hinzufügen** aus.

   > **Hinweis:** Im automatisch angezeigten Textfeld können Sie entweder mit der Eingabe beginnen, um einen Entwurf zu erstellen, oder Sie geben **#** ein, um auf ein vorhandenes Issue in einem Ihrer GitHub-Repositorys zu verweisen. Wir beginnen mit der ersten dieser beiden Möglichkeiten.

1. Geben Sie in das Textfeld **`Missing Wiki`** ein und drücken Sie dann **Strg + Enter** auf der Tastatur. Dadurch wird der Spalte **ToDo** ein neuer Aufgabenentwurf hinzugefügt.
1. Wählen Sie im neu hinzugefügten Aufgabenentwurf das Auslassungszeichen (…) aus, und wählen Sie im Dropdownmenü **Zu Issue konvertieren** aus.
1. Wählen Sie in der Dropdownliste **Element auswählen** **DevOpsCoreIntroRepo** aus, um die Aufgabe zu dem Repository hinzuzufügen, das Sie in der vorherigen Übung erstellt haben. Beachten Sie, dass dem Issue automatisch die Bezeichnung **#2** zugewiesen wurde.
1. Wählen Sie das Issue **Wiki fehlt** aus.
1. Beachten Sie im Bereich **Fehlendes Wiki #2**, dass Ihnen an dieser Stelle weitere Konfigurationseinstellungen zur Verfügung stehen, einschließlich Bezeichnungen und Meilensteinen.
1. Wählen Sie **Bezeichnungen hinzufügen** und wählen Sie in der Dropdownliste **Element auswählen** **Verbesserung** aus.
1. Wählen Sie **Meilenstein hinzufügen** aus, und wählen Sie in der Dropdownliste**Element auswählen** **Alpha-Release** aus.
1. Schließen Sie den Bereich **Wiki fehlt #2**.

   > **Hinweis:** Jetzt fügen Sie einen weiteren Aufgabenentwurf hinzu und konvertieren ihn zu einem Issue.

1. Wählen Sie auf der Seite **DevOps Core Intro-Projekt** in der Spalte **ToDo** **+ Aufgabe hinzufügen** aus.
1. Geben Sie in das Textfeld **`Additional collaborators needed`** ein und drücken Sie dann **STRG + Eingabetaste** auf der Tastatur. Dadurch wird der Spalte **ToDo** ein neuer Aufgabenentwurf hinzugefügt.
1. Markieren Sie in dem neu hinzugefügten Entwurf das Auslassungszeichen und wählen Sie im Dropdownmenü **In Issue konvertieren** und wählen Sie **DevOpsCoreIntroRepo** aus, um den Artikel zum Repository hinzuzufügen.

### Aufgabe 2: Hinzufügen einer Aufgabe für ein Issue

1. Wählen Sie auf der Seite **DevOps Core Intro-Projekt** in der Spalte **ToDo** **+ Aufgabe hinzufügen** aus.
1. Geben Sie im Textfeld **#** ein, um eine Liste vorhandener Repositorys anzuzeigen. Wählen Sie in der Liste **DevOpsCoreIntroRepo** aus.
1. Wählen Sie als Nächstes in der Liste der vorhandenen Issues **README-Seite des Repository ist leer #1** aus. Dadurch wird dieses Problem automatisch in der Spalte **ToDo** hinzugefügt.
1. Wählen Sie **README-Seite des Repository ist leer**, um den entsprechenden Bereich anzuzeigen.
1. Wählen Sie im Bereich **README-Seite des Repository ist leer** im Abschnitt **Zugewiesene Person hinzufügen …** aus. Wählen Sie dann in der Dropdownliste **Elemente auswählen** Ihren GitHub-Benutzernamen aus, und schließen Sie den Bereich **README-Seite des Repository ist leer**.
1. Ziehen Sie die Aufgabe **README-Seite des Repository ist leer** aus der Spalte **ToDo** in die Spalte **In Bearbeitung**.

### Aufgabe 3: Überprüfen der Automatisierungseinstellungen

1. Wählen Sie auf der Seite **DevOps Core Intro-Projekt** direkt unter dem Avatarsymbol das Auslassungszeichen (…) aus.
1. Wählen Sie in der Dropdownliste **Workflows** aus.
1. Sehen Sie sich auf der Seite **Workflows** im Menü auf der linken Seite die Liste der Standardworkflows an. '

   > **Hinweis:** Standardmäßig sind die Workflows **Aufgabe geschlossen** und **Pull Request gemerged** aktiviert.

1. Wählen Sie **Aufgabe geschlossen** aus, und sehen Sie sich den Workflow an. Der Workflow legt automatisch den Status eines Issues auf **Erledigt** fest, wenn dieses Element geschlossen wird.
1. Wählen Sie das Avatarsymbol in der oberen rechten Ecke der Seite aus, und wählen Sie im Dropdownmenü **Ihre Repositorys** aus.
1. Wählen Sie in der Liste der Repositorys **DevOpsCoreIntroRepo** aus.
1. Wählen Sie im Abschnitt **README** das Bleistiftsymbol aus, um die Datei im Bearbeitungsmodus zu öffnen.
1. Ersetzen Sie den vorhandenen Inhalt in der Datei durch den folgenden Text:

   ```text
   ### Welcome to DevOps Core Intro Project Repository ###

   **Projects are a customizable, flexible tool for planning and tracking your work.**

   To find out more, refer to GitHub documentation [about Projects](https://docs.github.com/issues/planning-and-tracking-with-projects/learning-about-projects/about-projects).
   ```

1. Wählen Sie **Commit changes** (Änderungen committen) aus.
1. Übernehmen Sie im Fenster **Änderungen committen** die Standardeinstellungen, und wählen Sie erneut **Änderungen committen** aus.
1. Wählen Sie die Registerkarte **Issues** aus.
1. Wählen Sie **README-Seite des Repository ist leer** aus.
1. Wählen Sie auf der Seite **README-Seite des Repository ist leer #1** **Issue schließen** aus.
1. Beachten Sie, dass das Schließen der Aufgabe die folgenden Aktionen zur Folge hatte:

   - Der Status des Elements wurde automatisch auf **Erledigt** geändert, was durch einen zusätzlichen Kommentar angezeigt wird. Dieser besagt, dass der Bot **github-project-automation** das Element von **In Bearbeitung** auf **Erledigt** im **DevOps Core Intro-Projekt** verschoben hat.
   - Der Meilenstein **Alpha-Release** wurde zu 50 % abgeschlossen, wie durch den grünen horizontalen Balken im Abschnitt **Meilenstein** auf der Seite angezeigt.

   > **Hinweis:** Wenn die Änderungen nicht angezeigt werden, aktualisieren Sie die Seite.

1. Gehen Sie zurück zur Boardansicht von **DevOps Core Intro-Projekt** zurück und überprüfen Sie, dass die Aufgabe **README-Seite des Repository ist leer** in der Spalte **Erledigt** angezeigt wird.
