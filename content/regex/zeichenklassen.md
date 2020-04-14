# Zeichenklassen

Mit einer Zeichenklasse kann eine Menge von Zeichen spezifiziert werden. Genau ein beliebiges Zeichen dieser Definition kann an der entsprechenden Position vorkommen.

!!! info "Merke"
    Genau ein Zeichen der Zeichenklasse darf an der entsprechenden Position vorkommen.

## Definition einer Zeichenklasse

Innerhalb der eckigen Klammern `[` und `]` kann eine Zeichenklasse definiert werden. Die einfachste Form der Definition ist dabei das bloße Aufzählen von gültigen Zeichen der Zeichenklasse. 

Die folgende Zeichenklasse würde die Zeichen `a`, `b`, `A`, `B`, `0`, `1`, `2` und `3` umfassen.

```
[aAbB0123]
```

## Invertierte Definition

Ein vorangestelltes Dach-Symbol `^` (auch Zirkumflex oder Caret-Symbol) kann verwendet werden um eine invertierte Klassendefinition durchzuführen. Dabei werden alle Zeichen aufgeführt die __nicht__ an der entsprechenden Position vorkommen dürfen.

Die folgende Zeichenklasse würde aufgrund des vorangestellten Dach-Symbols __alle Zeichen__ außer der Ziffern von `0` bis `9` als Teil der Klasse definieren.

```
[^0123456789]
```

## Bereichsdefinitionen

Mit dem Bindestrich `-` können gültige Bereiche innerhalb einer Zeichenklasse definiert werden. Dabei kann spezifiziert werden, dass alle Zeichen von einem beliebigen Startzeichen bis zu einem beliebigen Endzeichen gültig sind. Der Bindestrich kann auch mehrmals innerhalb einer Klassendefinition verwendet werden.

Die folgende Zeichenklasse würde dabei alle Klein- und Großbuchstaben, also alle Zeichen von `a` bis `z` und von `A` bis `Z`, definieren:

```
[a-zA-Z]
```

Eine wichtige Anmerkung dabei ist, dass Zeichen anhand des entsprechenden Zeichensatzes geordnet sind. Für Regular Expressions kommt hierbei grundsätzlich der `ASCII-Zeichensatz` zum tragen. Der ASCII-Zeichensatz besteht dabei aus 128 druckbaren und nichtdruckbaren Steuerzeichen. Die druckbaren Zeichen umfassen das lateinische Alphabet in Groß- und Kleinschreibung, die zehn arabischen Ziffern sowie einigen Sonderzeichen. Der Zeichenvorrat entspricht weitgehend dem einer Tastatur für die englische Sprache.

Die durckbaren Zeichen beginnen dabei mit dem Leerzeichen und folgen der unten angeführten Ordnung:

```
 !"#$%&'()*+,-./0123456789:;<=>?
@ABCDEFGHIJKLMNOPQRSTUVWXYZ[\]^_
`abcdefghijklmnopqrstuvwxyz{|}~
```

Dabei würde die Zeichenklasse `[ -~]` alle druckbaren Zeichen inkl. Leerraum des ASCII-Zeichensatzes umfassen.

!!! danger "Vorsicht"
    Um die Zeichenklasse aller Buchstaben anzugeben sollte `[a-zA-Z]` verwendet werden. Die Zeichenklasse `[A-z]` würde auch Zeichen wie `[`, `\` oder `]` enthalten. Des weiteren würde die Zeichenklasse `[a-Z]` eine leere Menge definieren, da `a` nach `Z` in der Ordnung des ASCII-Zeichensatzes auftritt.

## Vordefinierte Zeichenklassen

Für gängige Zeichenklassen wurden Kurzformen definiert, welche anstelle einer Zeichenklassendefinition verwendet werden können.

| Kurzform | Bedeutung | Zeichenklasse |
| --- | --- | --- |
| `\d` | Ziffern | `[0-9]` |
| `\D` | Alles außer `\d` | `[^0-9]`, `[^\d]` |
| `\w` | Alphanumerische Zeichen inkl. dem Unterstrich | `[a-zA-Z0-9_]` |
| `\W` | Alles außer `\w` | `[^a-zA-Z0-9_]` |
| `\s` | Weißraum (zB Leeraum, Tabulator, Zeilenumbruch, ...) | `[ \f\n\r\t\v]` und alle Unicodevarianten zB `\u00a0` |
| `\S` | Alles außer `\s` | `[^\s]` |

## Beliebiges Zeichen

Mit dem Punkt `.` wird spezifiziert, dass ein beliebiges Zeichen an der entsprechenden Position vorkommen darf. Dabei ist der Punkt ähnlich zu den vordefinierten Zeichenklassen zu verstehen. Diese Zeichenklasse enthält alle Zeichen mit Ausnahme des Zeilenumbruchs. Somit könnte der Punkt auch über die Zeichenklasse `[^\n]` definiert werden.

## Escaping

Innerhalb einer Zeichenklassendefinition, also innerhalb der Klammern `[` und `]`, müssen nur die Zeichen `]`, `-`, `\` und `^` escaped werden.
