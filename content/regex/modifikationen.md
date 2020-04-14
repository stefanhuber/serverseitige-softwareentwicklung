# Modifikationen

Über unterschiedliche Flags kann das Verhalten der Regex Engine verändert werden. Innerhalb von Programmiersprachen oder Regex-Werkzeugen können diese Modifikationen generell außerhalb der Regular Expression angegeben werden. Zusätzlich gibt es jedoch auch die Möglichkeit einer Inline-Angabe.

## Wichtige Modifikationen

| Modifikation | Bedeutung |
| --- | --- |
| `i` | Case Insensitive: Groß- und Kleinschreibung wird nicht unterschieden |
| `s` | Single Line Mode: Die Zeichenklasse `.` ist so definiert, dass der Zeilenumbruch nicht inkludiert ist (`\n`), dies wird damit aufgebohen  |
| `m` | Multi Line Mode: Die Anker `^` und `$` beziehen sich nicht mehr auf den Start und das Ende der gesamten Zeichenkette, sondern auf den Start und das Ende einer Zeile |
| `x` | Free-spacing Mode: Weißraum (zB Leerraum, Zeilenumbruch, Tabulator, ...) innerhalb der Regular Expression wird ignoriert, damit kann eine Regular Expression mehrzeilig (erhöhung der Lesbarkeit) erstellt werden |

Es gibt sehr viele unterschiedliche Implementierungen von Regex Engines. Regex Engines unterschützen unterschiedliche weitere proprietäre Modifikationen.

## Inline Modifikation

Modifikationen können auch innerhalb einer Regular Expression an beliebigen Positionen integriert werden. Dabei wird zum Beispiel die Case Insensitive Modifikation über folgenden Ausdruck aktiviert: `(?i)`. Eine Deaktivierung der Modifikation wird duch einen Bindestrich angegeben. Die Case Insensitive Modifikation wird mit `(?-i)` deaktiviert.

Die folgende Regular Expression würde die Zeichenketten `test`, `Test`, `TEst` und `tEst` matchen:

```
(?i)te(?-i)st
```

Mit der Inline-Angabe können auch mehrere Modifkationen gleichzeitig aktiviert oder deaktiviert werden. Die Angabe `(?is-m)` würde Case Insensitive und Single Line Mode aktivieren und Multi Line Mode deaktivieren.  
