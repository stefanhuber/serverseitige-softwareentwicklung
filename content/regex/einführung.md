# Einführung

Regex (genau _Regulärer Ausdruck_ oder _Regular Expression_) ist eine Sprache um Muster in Zeichenketten zu beschreiben. Reguläre Ausdrücke sind weit verbreitet und in allen Programmiersprachen bzw. direkt in vielen Programmen unterstützt.

Reguläre Ausdrücke werden zum _Pattern Matching_ verwendet. Dabei wird das Muster als Regex spezifiziert und in unterschiedlichen Kontexten auf Zeichenketten angewandt. Folgendes sind einige gängige Anwendungsbereiche von Regulären Ausdrücken:

 - Suche von Textstellen in Dokumenten
 - Validierung von Formulareingaben
 - Textstellen aus Dokumenten oder Webseiten extrahieren
 - Textstellen suchen und ersetzen
 - Konfigurationsparameter

## Erstes Beispiel und wichtige Begriffe

Eine Zeichenkette (im Kontext von Regex wird diese _Subjekt_ genannt) soll nach dem Muster `abc` durchsucht werden und alle Position an denen das Muster vorkommt sollen ermittelt werden:

<pre><code>abc dde abcd aabb bb aa bb
ab9d abd abkk abkabcde</code></pre>

Die _Regex Engine_, welche die Regex auf das Subjekt anwendet, geht das Subjekt Zeichen für Zeichen von links nach recht durch. An jeder Position wird geprüft, ob das Muster an der Position übereinstimmt. Falls es eine Übereinstimmung an einer Position gibt, wird diese markiert. Das Ergebnis der Regex Engine würde wie folgt aussehen:

<pre><code><span style="text-decoration: underline">abc</span> dde <span style="text-decoration: underline">abc</span>d aabb bb aa bb
ab9d abd abkk abk<span style="text-decoration: underline">abc</span>de</code></pre>

Hierbei handelt sich um ein sehr einfaches Beispiel. Ein einfacher Vergleich der Zeichenkette mit dem Muster an jeder Position reicht aus, um die Übereinstimmungen zu finden. Mit Regex können jedoch viel komplexere Muster beschrieben werden. Die entsprechenden Bestandteile dieser Musterbeschreibungssprache finden sich auf den weiteren Unterseiten.

## Regex Metazeichen

Zur Beschreibung einer Regex, also zur Beschreibung eines Musters, werden Metazeichen verwendet. Diese Metazeichen sind im Folgenden aufgeführt:

```
[ ] { } ( ) . ? * + ^ $ | \
```

Diese Zeichen haben eine spezielle Bedeutung zur Musterbeschreibung und können nicht direkt zur Suchübereinstimmung mit der Zeichenkette verwendet werden. Falls ein Metazeichen Teil des Suchstrings sein soll, muss es über den Backslash `\` maskiert (escaped) werden.
