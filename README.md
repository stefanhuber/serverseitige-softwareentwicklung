# Skriptum Serverseitige Softwareentwicklung

Dieses Skriptum enthält einen Teil der Inhalte der Lehrveranstaltung [Serverseitige Softwareentwicklung](https://www.fh-kufstein.ac.at/studieren/Bachelor/Web-Business-Technology-VZ/Curriculum/technologie/serverseitige-softwareentwicklung-data-management). Ergänzend zu diesem Skriptum gibt es Videos zur Lehrveranstaltung, welche sich auf [Youtube](https://www.youtube.com/playlist?list=PL9QmSesKWE_haMXPuDkKgrjwxF1UmtVBy) befinden. Alle Übungsaufgaben befinden sich auf [Gitlab](https://gitlab.web.fh-kufstein.ac.at/).

## Verwendete Technologien

Die Inhalte des Skriptums werden als [Markdown Dokumente](https://en.wikipedia.org/wiki/Markdown) geführt und finden sich im Ordner [content](https://github.com/stefanhuber/Serverseitige-Softwareentwicklung/tree/master/content) des Git-Repositories.

Mit dem Programm [MkDocs](https://www.mkdocs.org/) wird aus den Markdown Dokumenten eine statische Website generiert. Als Theme wurde [Material for MkDocs ](https://squidfunk.github.io/mkdocs-material/) gewählt. Die generierten Inhalte finden sich Ordner [docs](https://github.com/stefanhuber/Serverseitige-Softwareentwicklung/tree/master/content) des Git-Repositories. Der Ordner `docs` wird mittels [GithubPages](https://pages.github.com/) gehostet und ist über diesen [Link](https://stefanhuber.github.io/serverseitige-softwareentwicklung) öffentlich verfügbar.

## Website generieren

 - Python 3 muss installiert sein (pip muss über die Kommandozeile verfügbar sein)
 - `pip install mkdocs` wird verwendet um [MkDocs](https://www.mkdocs.org/) zu installieren
 - `pip install mkdocs-material` wird verwendet um [Material for MkDocs ](https://squidfunk.github.io/mkdocs-material/) zu installieren
 - Mit dem Befehl `mkdocs server` kann die Website live am Localhost betrachtet werden (Änderungen in den Markdown Dateien führen zu einem automatischen Rebuild der Website)
 - Mit dem Befehl `mkdocs build` kann die Website neu generiert werden, sodass die Änderungen über `git push` veröffentlicht werden können

## Lizenzierung
![Creative Commons Lizenzvertrag](https://i.creativecommons.org/l/by-sa/4.0/88x31.png "Creative Commons Lizenzvertrag")

Die Inhalte sind lizenziert unter einer [Creative Commons Namensnennung - Weitergabe unter gleichen Bedingungen 4.0 International Lizenz](https://github.com/stefanhuber/sem/blob/master/LICENSE.md)