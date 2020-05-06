# Anker und Wortgrenzen

Mit Anker und Wortgrenzen sind Übereinstimmungen der _Länge 0_ definiert. Diese können verwendet werden um eine Regex innerhalb eines Kontexts zu verankern.

## Wortgrenzen

Eine Wortgrenze ist eine definierte Kurzform `\b`, welche eine Zeichenlänge 0 aufweist. An folgenden Positionen kommt eine Wortgrenze vor:

 - Vor dem ersten Zeichen eines Wortes
 - Nach dem letzten Zeichen eines Wortes
 - Innerhalb eines Wortes, falls zB ein Bindestrich verwendet wird

Mit dem folgenden Beispiel werden alle Wörter identifiziert, welche ein scharfes ß in der Schreibweise enthalten.

```
\b([a-zA-Z]*ß[a-z]*)\b
``` 

Dabei werden tatsächlich die Wörter identifiziert, da die Regeln der Wortgrenzen durch `\b` eintreten:

<pre><code>Eine <span style="text-decoration: underline">Straße</span> in <span style="text-decoration: underline">Straßburg</span>.</code></pre>

## Anker

Anker werden verwendet um eine Regular Expression an oder innerhalb definierter Positionen zu verankern. So kann mit `^` der Beginn der zu suchenden Zeichenkette und mit `$` das Ende der zu suchenden Zeichenkette angegeben werden.

Im unten angeführten Beispiel wird das letzte Wort eines Textes gesucht:

```
([A-Za-zß]*)\W?$
```
