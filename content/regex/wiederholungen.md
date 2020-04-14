# Wiederholungen

Für einzelne Bestandteile einer Regex können beliebige Wiederholungen (auch als Quantoren bezeichnet) definiert werden. Eine Wiederholung bezieht sich immer auf den voranstehenden Teil der Regex. Eine Wiederholungsangabe spezifiziert dabei nicht zwingend die Länge der Zeichenkette, sondern die Wiederholungsrate des voranstehenden Ausdrucks.

## Wiederholungsangabe

Wiederholungen werden innerhalb von geschweiften Klammern `{` und `}` definiert.

| Wiederholungsangabe | Bedeutung |
| --- | --- |
| `{n}` | Exakt `n` Wiederholungen |
| `{min,max}` | Mindestens `min` und maximal `max` Wiederholungen |
| `{min,}` | Beliebig viele Wiederholungen, aber mindestens `min` Wiederholungen |

Das folgende Beispiel würde angeben, dass sich der Inhalt der Zeichenklasse mindestens 2-mal und maximal 5-mal wiederholen darf:

```
[0123456789]{2,5}
```

## Kurzformen

Ähnlich zu den Kurzformen für Zeichenklassen, sind für Wiederholungen Kurzformen für häufige Konfigurationen definiert.

| Kurzform | Bedeutung | Alternative |
| --- | --- | --- |
| `+` | Beliebig viele Wiederholungen, aber mindestens `1` | `{1,}` |
| `*` | Beliebig viele Wiederholungen (auch `0`) | `{0,}` |
| `?` | Keine oder eine Wiederholung (Optionaler Ausdruck) | `{0,1}` |
