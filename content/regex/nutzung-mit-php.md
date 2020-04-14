# Nutzung mit PHP

PHP bietet die Funktion `preg_match`, welche zum Ausführen von Regex verwendet werden kann. Das `p` steht dabei für `Perl-compatible Regular Expression (PCRE)`.

Eine Regex wird in PHP immer als String angegeben. Wichtig dabei ist, dass der String den definierten Aufbau `/regex/modifier` haben muss.

## Beispiel: Prüfung auf Übereinstimmung

`preg_match` gibt `1` als Rückgabewert falls eine Übereinstimmung mit dem Muster gegeben ist und `0` falls keine Übereinstimmung gegeben ist.

```php
<?php

$regex = '/\d{2}\.\d{2}\.\d{4}/';

echo preg_match($regex, "12-23-2004"); // 0
echo preg_match($regex, "12.12.2004"); // 1
```

## Beispiel: Extraktion der Übereinstimmungen

Durch die Angabe des Referenzparameters `&$match` werden alle Übereinstimmungen bzw. Teilübereinstimmungen aus der Regex in der übergebenen Variable gespeichert.

```php
<?php

$regex = '/(?<hours>\d{2}):(?<minutes>\d{2})(?: Uhr)?/';
$subject = 'Ab 07:30 Uhr gibt es Frühstück.';
preg_match($regex, $subject, $matches);
print_r($matches);
```

Die Ausgabe des PHP-Skriptes sieht folgendermaßen aus:

```
Array
(
    [0] => 07:30 Uhr
    [hours] => 07
    [1] => 07
    [minutes] => 30
    [2] => 30
)
```

## Beispiel: Alle Übereinstimmungen

Die Funktion `preg_match` stoppt nach auffinden der ersten Übereinstimmung im Subjekt. Durch die Funktion `preg_match_all` werden alle Übereinstimmungen im Subjekt gesucht.

```php
<?php

$regex = '/(?<hours>\d{2}):(?<minutes>\d{2})(?: Uhr)?/';
$subject = 'Von 07:30 bis 10:00 Uhr gibt es Frühstück und von 12:00 bis 13:30 Uhr wird das Mittagessen serviert.';
preg_match_all($regex, $subject, $matches);
print_r($matches);
```

Das Array `$matches` hat dabei folgenden Inhalt. Die Strukturierung des Arrays ist dabei anhand der Regex vorgenommen und nicht entlang der einzelnen Übereinstimmungen:

```
Array
(
    [0] => Array
        (
            [0] => 07:30
            [1] => 10:00 Uhr
            [2] => 12:00
            [3] => 13:30 Uhr
        )

    [hours] => Array
        (
            [0] => 07
            [1] => 10
            [2] => 12
            [3] => 13
        )

    [1] => Array
        (
            [0] => 07
            [1] => 10
            [2] => 12
            [3] => 13
        )

    [minutes] => Array
        (
            [0] => 30
            [1] => 00
            [2] => 00
            [3] => 30
        )

    [2] => Array
        (
            [0] => 30
            [1] => 00
            [2] => 00
            [3] => 30
        )

)
```
