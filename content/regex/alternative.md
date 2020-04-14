# Alternative

Die Alternative wird über den senkrechten Strich `|` angegeben. Die Alternative hat die Funktion eines logischen `XOR`.

## Einfaches Beispiel

Um eine Datumsangabe entweder mit Punkt oder Bindestrich getrennt zu akzeptieren, können beide gültigen Varianten mit der Alternative getrennt angegeben werden.

```
\d{2}\.\d{2}\.\d{4}|\d{2}-\d{2}-\d{4}
```

Die Regex würde Datumsangaben der Form `12.12.2004` und `31-10-2016` erlauben.

## Geltungsbereich

Der Geltungsbereich der Alternative ist generell global. Durch Nutzung der Gruppierungsfunktion kann der Geltungsbereich jedoch auf die Gruppe eingeschränkt werden.

```
\d{2}\(?:-|\.)\d{2}\(?:-|\.)\d{4}
```

## Reihenfolge

In der Verarbeitung der Alternative geht die Regex Engine so vor, dass bei der ersten erfolgreichen Übereinstimmung einer Alternative die Verarbeitung gestoppt wird. Dieses Verhalten nennt man _eager_.

```
(e|ea|eag)
```

Die angegebene Regex würde folgende Übereinstimmungen erzeugen:

<pre><code><span style="text-decoration: underline">e</span>ag<span style="text-decoration: underline">e</span>r</code></pre>

Da `e` bereits als Übereinstimmung gilt, werden die anderen Alternativen nicht mehr untersucht (obwohl sie ebenfalls übereinstimmen würden).