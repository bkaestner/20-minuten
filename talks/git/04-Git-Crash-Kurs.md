---
title: Git-Crash-Kurs
author: Benjamin Kästner <benjamin.kaestner@gmail.com>
date: 7.9.2015
links-as-notes: true
---
# Git
## Was ist Git?

Laut Wikipedia:

> Git (engl. _Blödmann_) ist eine freie Software zur verteilten
> Versionsverwaltung von Dateien, die ursprünglich für die Quellcode-Verwaltung
> des Linux-Kernels entwickelt wurde.

. . .

- verteilt
- versionsverwaltend
- frei

## Was ist Git nicht

Git ist

- kein Backup-System
- kein Allheilmittel für Projektstrukturierung

## Warum Versionsverwaltung?

Manuelle Alternativen sind fehleranfällig:

- Backup-Verzeichnisse (`project-2015-08-12-bak/`) geben keine Auskunft darüber, welchen Stand sie (abgesehen von ihrer Zeit) haben.
- Dateinamensendungen wie `_rev1`, `_v2` oder ähnlich geben keine Auskunft, ob `_v2` experimentell war, und ob `_v3` oder `_v4` getestet wurde.
- Team-Arbeit wird durch manuellen Dateiaustausch erschwert.


# Erste Schritte mit Git
## Git initialisieren
Um git verwenden zu können, muss zunächst ein Repository initialisiert oder ein
vorhandenes geklont werden:

```
$ cd path/to/project
$ git init
Initialized empty Git repository in path/to/project.git
```

Git beobachtet zu diesem Zeitpunkt noch keine Dateien.

## Änderungen und Dateien vormerken
Dateien werden mittels `git add` vorgemerkt:
```
$ ls
README.md src/ doc/ build/

$ git add README.md src
```
Das bezieht sich sowohl auf neue, als auch auf alte Dateien. Wenn also `README.md` sich geändert hat, dann wird diese Änderung mittels `git add README.md` vorgemerkt.

Mit `git rm` und `git mv` werden Dateien gelöscht bzw. verschoben.

## Gegenwärtiger Status
Mit `git status` wird der gegenwärtige Stand von git angezeigt:

```bash
$ git status
On branch master

Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

        new file:   README.md
        new file:   src/main.c
        new file:   src/util.h
        new file:   src/util.c
```
Git listet hierbei geänderte, neue, unbeobachtete Dateien und ähnliches auf.

## Änderungen festlegen
Änderungen, die mit `git add/rm/mv` vorgemerkt wurden, werden mit `git commit` zu einem _Commit_ zusammengefügt:

```bash
$ git commit
<es öffnet sich ein Editor zur Eingabe einer Nachricht>
```

. . .

Jede Änderung, die für sich eine logische Einheit bildet (neue Funktion, Bugfix, Dokumentation) kann ein Commit sein. Ein Commit hat hierbei eine eindeutige ID (`git show`).

Alternativ: `git commit -m "Nachricht des Commits"`

Die Nachricht sollte ähnlich gefasst sein wie eine Patch-E-Mail.

## Änderungen nachverfolgen
Mittels `git log` können letzte Änderungen angesehen werden:

. . .

```bash
$ git log
commit 430bbbabb752bf1352f6bdf75d29c5f98465ae8c
Author: Benjamin Kaestner <benjamin.kaestner@gmail.com>
Date:   Tue Aug 18 20:05:52 2015 +0200

    Add Readme

commit bec9a960e844d186637d3da777e648517b1507db
Author: Benjamin Kaestner <benjamin.kaestner@gmail.com>
Date:   Tue Aug 18 19:58:09 2015 +0200

    Add Travis configuration
```

## Bisherige Zusammenfassung

Befehl       | Wirkung
-------------|-----------------------------------------
`git add`    | Datei(änderungen) hinzufügen (vormerken)
`git rm`     | Datei aus git löschen (vormerken)
`git mv`     | Datei umbenennen (vormerken)
-------------|-----------------------------------------
`git status` | Status abfragen
`git commit` | Vorgemerkte Änderungen in git Speichern
`git log`    | Historie der vergangene Commits ansehen


# Arbeiten mit Entwicklungszweigen
## Was ist ein Commit?
Jeder Commit ist ein Snaphsot des gegenwärtigen Standes. Jeder Commit hat dabei
einen (oder zwei) vorangehende Commits:

. . .

![Kind-Vater-Beziehung: ein jeder Commit zeigt auf seinen Vorgänger](talks/git/04-assets/commits.png)

## Was ist ein Zweig?
Ein Zweig ist im Grunde genommen ein Label, der auf einen Commit zeigt und sich bei einem `git commit` mitbewegt.

## Was ist ein Zweig? (2)

![master branch vor Commit](talks/git/04-assets/branches-01.png)

. . .

![master branch nach Commit](talks/git/04-assets/branches-02.png)

## Informationen über Zweige
Der Befehl `git branch` zeigt die gegenwärtigen lokalen Zweige und welcher Zweig gerade aktiv ist:

```bash
$ git branch
* git-talk
  master
```

In diesem Fall Es gibt zwei Zweige: `git-talk` und `master`. `git-talk` ist aktiv.

## Wechseln zwischen Zweigen
Um zu einem anderen Zweig zu wechseln, wird `git checkout <branch>` verwendet:

```bash
$ git checkout master
Switched to branch master

$ git branch
  git-talk
* master
```

## Neuen Zweig erstellen
Um einen neuen Zweig zu erstellen, wird `git branch <name>` verwendet:

```bash
$ git branch temp
$ git branch
  git-talk
* master
  temp
```
Um im neuen Zweig zu arbeiten kann `git checkout <name>` verwendete werden. Alternativ können diese Aktionen über

```bash
git checkout -b <neuer-zweig>
```

zusammengefasst werden.

## Beispiel
```bash
$ git checkout -b fancy-feature
$ <mehrere commits>
```

. . .

![Tree nach Branching und Commits](talks/git/04-assets/branches-03.png)

## Merge
Um Zweige zusammenzuführen verwendet man `git merge`:

```bash
$ git checkout <zielbranch>
$ git merge <branch-mit-aenderungen>
```

## Merge-Beispiel
```bash
$ git checkout master
$ git merge fancy-feature
```

. . .

![Tree nach Merge und Commits](talks/git/04-assets/branches-04.png)


## Bisherige Zusammenfassung
Befehl                 | Wirkung
-----------------------|-----------------------------------------
`git branch`           | Branches anzeigen
`git branch name`      | Neuen Branch `name` erzeugen
`git branch -d name`   | Branch `name` löschen
-----------------------|-----------------------------------------
`git checkout name`    | Auf Branch `name` wechseln
`git checkout -b name` | Branch `name` erstellen und auf ihn wechseln
-----------------------|-----------------------------------------
`git merge name`       | Commits von Branch `name` zusammenführen


# Übungsaufgaben
## Überprüfen Sie Ihre gegenwärtige Versionskontrolle
Verwenden Sie gegenwärtig `_v2` oder ähnliche Konstrukte? Hatten Sie damit mehr
als einmal Probleme? Verwenden Sie kein anderes SCM? Dann installieren Sie git.

Für binäre Dateien (Word, Excel, Bilder, PDF etc.) ist git eher nicht geeignet,
es ist dennoch möglich. Allerdings bringen Word und Excel ihre eigene
Änderungsverfolgung, und der LaTeX-Quellcode einer PDF kann ohne Probleme von
git verwaltet werden.

## Installieren Sie git
Unter den meisten Linux-Distributionen ist git vorinstalliert. Unter Windows
muss selbst Hand angelegt werden. Anleitungen für Windows, Mac OS X und Linux
finden Sie auf http://git-scm.com/.

## Arbeiten Sie eine Woche lang mit git
Stellen Sie ein bereits bestehendes oder ein neues Projekt unter git. Verwenden
Sie zur Verwaltung die Befehle `init`, `add`, `rm`, `mv`, `branch`, `merge`,
`checkout` und `commit`.

. . .

Lesen Sie bei Fragen die Dokumentation: `git <befehl> --help` öffnet unter Linux
eine man-Page, unter Windows den Webbrowser mit zusätzlichen Informationen.

. . .

Werden Sie in dieser Woche mit git vertraut, zumindest auf lokaler Basis.
