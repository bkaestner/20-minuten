---
title: Git-Team-Crash-Kurs
author: Benjamin Kästner <benjamin.kaestner@gmail.com>
date: 14.9.2015
links-as-notes: true
---
# Eine kurze Rückblende
## Was ist Git?

- ***verteilt***
- versionsverwaltend
- frei

## Was ist Git nicht?

Git ist

- kein Backup-System,
- aber dennoch besser als gar nichts.

## Die wichtigsten Befehle

Befehl     | Effekt
-----------|---------------------------------
`add`      | Änderungen vormerken
`blame`    | Autor von Änderungen betrachten
`branch`   | Zweiginformationen einholen
`checkout` | HEAD auf Referenzen wechseln
`commit`   | Vorgemerktes anwenden
`diff`     | Änderungen anzeigen
`merge`    | Zwei Zweige zusammenführen
`mv`       | Umbenennen vormerken
`log`      | Änderungsprotokoll einsehen
`rm`       | Löschen vormerken
`show`     | Informationen zu einer Referenz anzeigen
`status`   | Gegenwärtigen Status anzeigen
`tag`      | Tags anzeigen oder erstellen

(Referenz: Commit-Hash, Zweig, Tag,…)

# Git im Team verwenden
## Entfernte Repositories erstellen
### GitLab oder Gitolite verwenden
- GitLab kann selbst eingerichtet werden: https://gitlab.fhrel.fgan.de (Proxy-Ausnahme hinzufügen)
- Gitolite wird von einem Administrator eingerichtet

## Ein Projekt klonen
Ein bereits bestehendes Projekt wird mittels `git clone URI` geklont, beispielsweise


```bash
$ git clone https://github.com/bkaestner/20-minuten
$ cd some/path
$ git clone path/to/repo
```
Ein URI kann auf lokale Dateien verweisen, oder die Protokolle HTTPS, SSH oder git verwenden.
Standardmäßig wird das geklonte Repository in einem neuen Ordner gespeichert.

## Ein bestehendes Projekt verteilen
Diese Schritte sind _nicht_ notwendig, falls das Projekt geklont wurde.

1. Unter Gitolite oder GitLab ein Projekt erstellen (lassen).
2. Git über das entfernte Verzeichnis (*remote*) in Kenntniss setzen:
   `git remote add <name> <URL>`

## Änderungen abrufen
Mittels `git fetch <remote>` werden Änderungen abgerufen:


```bash
$ git fetch
From gitlab.fhrel.fgan.de:kaestner/20-minuten

5fe260f..bd940e5 master -> origin/master
* [new tag]        talk-04     -> talk-04
* [new branch]     git-talk-02 -> origin/git-talk-02
```

## Änderungen einpflegen
Bislang haben wir neue Änderungen heruntergeladen. Diese können wir nun wie gehabt `merge`n:

```bash
$ git checkout master
$ git merge origin/master # Änderungen mergen
```

Da `git fetch` und `git merge` häufig hintereinander ausgeführt werden, gibt es
den kombinierten Befehl `git pull`:

```bash
$ git checkout master
$ git pull origin master # fetch + merge
```

## In Bildern (1)

![Nach fetch, vor merge](talks/git/05-assets/branches-01.png)

## In Bildern (2)
![Nach fetch+merge, bzw. pull](talks/git/05-assets/branches-02.png)

## Änderungen verteilen
Änderungen werden mit `git push` an das entfernte Verzeichnis geschickt:

```bash
$ git checkout <branch-name>
$ git push <remote> <branch-name>
```

Will man vermeiden, jedes Mal `<remote> <branch-name>` angeben zu müssen, so lässt sich
mittels `-u` der entfernte Zweig des lokalen einstellen:

```bash
$ git checkout git-talk-02
$ git push -u origin git-talk-02

```
## Zusammenfassung der Befehle

Befehl     | Effekt
-----------|---------------------------------
`fetch`    | Commits herunterladen
`pull`     | fetch + merge
`push`     | Änderungen verteilen
`remote`   | Entfernte Repositories verwalten


# Übungsaufgaben
## Eigene Projekte auf GitLab, Gitolite oder ähnlichem einpflegen

Letzte Woche hatten Sie die Aufgabe, ein Woche lang mit git zu arbeiten.
Verteilen Sie Ihr Projekt mittels GitLab oder Gitolite (achten Sie
hierbei allerdings auf korrekte Zugriffsrechte!).

Beachten Sie, dass die Benutzung externer Services wie GitHub, BitBucket oder
GitLab.com (nicht gitlab.fhrel.fgan.de) in der Regel nicht zulässig ist.

## Weitere Ressourcen
- https://www.atlassian.com/git/tutorials/syncing/git-fetch/
- https://git-scm.com/book/en/v2/Distributed-Git-Distributed-Workflows
- https://git-scm.com/book/en/v2/Git-on-the-Server-The-Protocols