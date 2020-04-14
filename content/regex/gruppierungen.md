# Gruppierungen

Mit Gruppierungen können Teile einer Regex als Teilübereinstimmung ausgezeichnet bzw. auch referenziert werden. Eine Gruppierung wird über die normalen Klammern `(` und `)` spezifiziert. Regex Engines geben die Gruppierungen als separate Teilübereinstimmungen zurück.

!!! info "Merke"
    Eine Wiederholung, welche einer Gruppierung nachfolgt, bezieht sich auf die gesamte Gruppe.

## Normale Gruppierung

Innerhalb einer Regex kann die Gruppierungsfunktion beliebig genutzt werden. Durch die Gruppierung wird der gruppierte Ausdruck zu einer Einheit.

Im folgenden Beispiel sollen Uhrzeitangaben über eine Regex spezifiziert werden. Eine Uhrzeitangabe wird generell über das Format `HH:MM` (also Stunden `:` Minuten) angegeben. Stunden und Minuten werden jeweils über 2 Ziffern angegeben. _Optional_ kann auch das Wort _Uhr_ auf die Zeitangabe folgen:

```
(\d{2}):(\d{2})( Uhr)?
```

Die Stunden- bzw. die Minutenangabe bilden jeweils eine Gruppe mit 2 Ziffern. Die Zeichenkette ` Uhr` wird ebenfalls als Gruppe ausgezeichnet und über den Quantor `?` optional gesetzt. 

## Referenzierung 

Gruppen können innerhalb der Regex referenziert werden. Gruppen bekommen implizit einen Index zugewiesen, dieser wird von links nach rechts mit 1 beginnend erzeugt. Eine Gruppe kann über `\Index` referenziert werden.

Im folgenden Beispiel sollen alle Textteile, welche innerhalb von Anführungszeichen stehen, identifiziert werden. Dabei wird eine Gruppe eingeführt `(["'])`, welche die Zeichenklasse der Anführungszeichen enthält. Diese Gruppe wird am Ende der Regex wieder über `\1` referenziert, dabei wird jedoch die konkrete Übereinstimmung referenziert und nicht die Regex.

```
(["'])[^"']*\1
```

Die oben angegebene Regex würde im Text die unterstrichenen Teile finden. Es ist dabei anzumerken, dass Verschachtelungen ignoriert werden. Bei einem öffnenden `'` werden `"` bis zum `'` ignoriert.

<pre><code>Lorem Ipsum <span style="text-decoration: underline">"is simply dummy text"</span> of the <span style="text-decoration: underline">'printing and typesetting'</span> industry.
Lorem <span style="text-decoration: underline">'Ipsum "has been" the'</span> industry standard dummy text ever since the 1500s.</code></pre>

## Gruppierung ohne Übereinstimmung

Eine normale Gruppierung erfüllt zwei Aufgaben. Einerseits die Funktion der Gruppierung von Ausdrücken und andererseits die Erzeugung einer referenzierbaren Teilübereinstimmung. Es kann gewünscht werden, dass nur die Funktion der Gruppierung ausgeführt werden soll. Dazu wird die Gruppe mit den Zeichen `?:` eingeleitet.

Im Beispiel der Uhrzeit sollen nur die Stunden- bzw. Minutenangaben referenzierbar sein, nicht jedoch die optionale Zeichenkette:

```
(\d{2}):(\d{2})(?: Uhr)?
```

## Benannte Gruppierung

Für Gruppen können auch Bezeichner vergeben werden. Zur Datenextraktion kann dies sehr nützlich sein. Mit `?<name>` kann der Bezeichner `name` für die Gruppe vergeben werden.

Im unten stehenden Beispiel werden die Stunden bzw. Minuten über einen definierten Bezeichner identifiziert.

```
(?<hours>\d{2}):(?<minutes>\d{2})(?: Uhr)?
```
