# Grundlagen

## Erstes Beispiel

PHP-Code wird innerhalb einer Textdatei mit Suffix `.php` geschrieben. PHP-Skripte beginnen mit dem sog. PHP-Tag `<?php`. Falls die Textdatei noch weitere Inhalte (zB HTML) enthält kann PHP-Code auch Blockweise eingefügt werden, dafür müssen Blöcke mit `<?php` geöffnet und `?>` beendet werden.

Jede Anweisungszeile in PHP muss mit einem Semikolon (`;`) abgeschlossen werden. Ausgaben werden über den `echo` Befehl durchgeführt.

```php
<?php
echo "Hallo PHP!";
```

PHP kann als Skript über den PHP-Interpreter auf der Kommandozeile ausgeführt werden. Dazu wird der PHP-Interpreter auf der Kommondozeile ausgeführt und das auszuführende Skript wird als Argument an den Interpreter übergeben:

```bash
> php example.php
Hallo PHP!
```

## PHP in HTML einbetten

PHP kann innerhalb eines HTML-Dokuments eingebettet werden. Die Quellcode-Datei muss mit dem Suffix `.php` abgespeichert werden, sodass der PHP-Interpreter die Datei verarbeitet.

PHP-Code wird innerhalb von PHP-Tags `<?php` und `?>` eingefügt. Der PHP-Code wird am Web-Server verarbeitet und nicht an den Web-Client gesendet.

```php
<html>
    <head>
        <title>Hallo PHP</title>
    </head>
    <body>
        <?php
            // Ein Kommentar: Dieser wird nicht ins HTML geschrieben
            echo "Hallo PHP!";
        ?>
    </body>
</html>
```

## Kommentare

Im PHP-Quellcode können natürlich auch Kommentare eingefügt werden. Es gibt einzeilige- und mehrzeilige Kommentare:

```php
<?php
// einzeiliger Kommentar
# auch ein einzeiliger Kommentar

/*
mehrzeiliger
Kommentar
...
*/
```

## Variablen

Eine Variable in PHP wird mit dem `$`-Zeichen eingeleitet und es folgt ein Variablenbezeichner. Für Variablennamen sind folgende Regeln sind zu beachten:

 - Ein Variablenname muss mit einem Buchstaben oder Unterstrich starten
 - Ab der zweiten Stelle kann ein Variablenname auch Zahlen enthalten
 - Variablennamen sind case-sensitive (`$var` und `$VAR` sind verschiedene Variablen)

```php
<?php
$my_var = 123;
$message = "hello world";
```

## Datentypen

PHP ist eine dynamisch typisierte Sprache. Das bedeutet es müssen keine Datentypen für Variablen angegeben werden, diese werden vom PHP-Interpreter abgeleitet. Dennoch legt PHP 10 verschiedene Datentypen fest, welche einer Variable implizit entsprechen.

 - Einfache Datentypen: `boolean`, `integer`, `float` und `string`
 - Zusammengesetzte Datentypen: `array`, `object`, `iterable` und `callable`
 - Spezielle Datentypen: `resource`, `NULL`

Die Funktion `var_dump` kann verwendet werden um den Wert und den Datentyp einer Variable auszugeben. Dies ist hilfreich für das Debugging:

```php
<?php
$a = 3.1;
$b = true;
var_dump($a);  // float(3.1)
var_dump($b);  // bool(true)
```

!!! info "Hinweis"
    PHP unterstützt Typdeklarationen. Für Funktionsparameter können zum Beispiel Datentypen angegeben werden. Mit sogenannten `strict_types` kann der PHP-Code so konfiguriert werden, dass ein falscher Datentyp einen Fehler erzeugt.

### Boolean

Überall dort wo Bedingungen interpretiert werden (zB Kontrollstrukturen oder  Schleifen), können in PHP beliebige Datentypen vorkommen. PHP interpretiert Variablen mit nicht-boolschem Wert nach definierten Regeln. Generell interpretiert PHP alles als `true` außer:

 - den boolschen Wert `false`
 - die Zahlen `0`, `-0`, `0.0`, `-0.0`
 - den Leerstring `""` und `"0"`
 - ein leeres Array `[]`
 - `null`

!!! warning "Vorsicht"
    Es gerade Vorsicht geboten bei der Interpretation mancher Ergebnisse von Funktionen. Die Ausdrücke `false == 0`, `null = 0` oder `"" == 0` sind jeweils wahr.

### Casting

PHP unterstützt für jeden Datentyp und weitere Fälle eine `is_*` Funktion. Es kann zB mit der Funktion `is_int` geprüft werden, ob eine Variable dem Datentyp `integer` entspricht.

```php
<?php

$a = "abc";
$b = 123;

echo is_null($a);   // false
echo is_string($a); // true
echo is_float($b)   // false
```

Es kann der Datentyp einer Variable durch Casting explizit verändert werden:

```php
<?php

$var1 = 10;
$var2 = (bool) $var1;
var_dump($var2)  // bool(true)
```

Folgende Casting Operationen sind definiert:

 - `(int)`, `(integer)` um Wert in Integer zu casten
 - `(bool)`, `(boolean)` um Wert in Boolean zu casten
 - `(float)`, `(double)`, `(real)` um Wert in Float zu casten
 - `(string)` um Wert in String zu casten
 - `(array)` um Wert in Array zu casten
 - `(object)` um Wert in Objekt zu casten

## Operatoren

PHP verfügt über alle gängigen Operatoren, wie sie aus anderen Programmiersprachen bekannt sind. Einige wichtige Ergänzungen:

| Operator | Bezeichnung                           |
| -------- | ------------------------------------- |
| `.`      | String Konkatenation                  |
| `??`     | Null-Coalesing Operator               |
| `?:`     | Elvis Operator                        |
| `<=>`    | Spaceship Operator                    |
| `===`    | Vergleichsoperator: Wert und Datentyp |

#### String Konkatenation

```php
<?php
$str = "abc";
$res = $str . "-" . "def";
echo $res;  // abc-def
```

!!! warning "Vorsicht"
    In anderen Programmiersprachen wirder dre `+`-Operator für String Konkatenation verwendet. Verwendet man den `+`-Operator mit einem String versucht PHP die Strings in Zahlen zu casten und dieses Ergebnis zu konkatenieren:

    ```php
    <?php
    echo "123" + "123"; // 246    
    ```

#### Null-Coalesing Operator

Zuweisung eines Default-Wertes für eine Variable. Der erste Wert der existiert und nicht `NULL` ist. Im Beispiel ist `$z` nicht definiert, `$a` hat den Wert `NULL` und `$b` den Wert `10`. Die Null-Coalesing Kette wird solange durchgeführt bis ein gültiger Wert gefunden wird.

```php
<?php

$a = NULL;
$b = 10;
$c = $z ?? $a ?? $b;
echo $c;  // 10
```

#### Elvis Operator

Der Elvis Operator ist eine Vereinfachung des bekannten ternären Operators: `linker Ausdruck ?: rechter Ausdruck`. Die Logik ist dabei so gestaltet, dass wenn der linke Ausdruck als wahr interpretiert wird, dieser zurückgegeben wird ansonsten der rechte Ausdruck (als Alternative).

```php
<?php
$a = 5 ?: 10;
$b = 0 ?: 10;

echo $a;  // 5
echo $b;  // 10
```

#### Spaceship Operator

Der Spaceship Operator ist ein spezieller Operator um Ausdrücke zu vergleichen. Dieser gibt die Werte `-1`, `0` oder `1` zurück, je nachdem ob der erste Ausdruck kleiner, gleich oder größer als der zweite Ausdruck ist. Der Spaceship Operator kann auch für Strings verwendet werden:

```php
<?php

echo 1 <=> 1; // 0
echo 1 <=> 0; // 1
echo 0 <=> 1; // -1

echo "a" <=> "a"; // 0
echo "z" <=> "a"; // 1
echo "a" <=> "z"; // -1
```

Dieser Operator ist zum Beispiel für eigene Sortierfunktionen nützlich.

#### Vergleichsoperator

Generell sind 2 Vergleichsoperatoren zu unterscheiden `==` und `===`. `==` untersucht nur ob zwei Werte gleich sind, so wäre `"5" == 5` wahr. Der `===` Vergleichoperator untersucht jedoch auch, ob die 2 Operanden den selben Datentyp haben, somit wäre `"5" === 5` falsch und `5 === 5` wahr.

## Kontrollstrukturen

PHP verfügt natürlich auch über die klassische `IF-ELSE` Konstrollstruktur.

```php
<?php
$var = 10

if ($var == 5) {
    echo "Der Wert ist 5";
} elseif ($var < 5) {
    echo "Der Wert ist kleiner als 5";
} else {
    echo "Der Wert ist größer als 5";
}
```

PHP verfügt auch über einen `switch`-Ausdruck, dieser kann in der [PHP-Dokumentation](https://www.php.net/manual/en/control-structures.switch.php) nachgelesen werden.

## Schleifen

### For-Schleife

Die `For-Schleife` besteht im Schleifenkopf aus 3 Bestandteilen, welche mit `;` getrennt sind: 

 - Die Initialisierung wird vor dem ersten Schleifendurchlauf ausgeführt (im Beispiel `$i = 1`).
 - Die Bedingung wird vor jedem Schleifendurchlauf geprüft. Nur solange die Bedingung wahr ist, wird der Schleifenkörper ausgeführt (im Beispiel `$i < 5`).
 - Der Iterator wird nach jedem Schleifendurchlauf ausgeführt (im Beispiel `$i++`).

```php
<?php
for ($i = 1; $i < 5; $i++) {
    echo $i . " Schleifendurchlauf\n";
}
```

Das Programm führt zu folgender Ausgabe:

```
1. Schleifendurchlauf
2. Schleifendurchlauf
3. Schleifendurchlauf
4. Schleifendurchlauf
```

### While-Schleife

PHP besitzt natürlich auch die klassische `While-Schleife`. Dabei wird der Schleifenkörper nur solange durchgeführt bis die Bedingung im Schleifenkopf nicht mehr wahr ist.

```php
<?php
$i = 1;
while($i<5) {
    echo $i++ . " Schleifendurchlauf\n";
}
```

Das Programm führt zu folgender Ausgabe:

```
1. Schleifendurchlauf
2. Schleifendurchlauf
3. Schleifendurchlauf
4. Schleifendurchlauf
```

### Do-While Schleife

PHP unterstützt auch die sogenannte `Do-While Schleife`. Diese Schleife wird eher selten verwendet und manche Programmiersprachen unterstützen dieses Konstrukt erst garnicht (zum Beispiel `Python`). Bei diesem Schleifentyp wird die Bedingung erst am Ende des ersten Schleifendurchlaufs evaluiert, sodass die Schleife zumindest immer einmalig ausgeführt wird.

```php
<?php
$i = 1;
do {
    echo $i . " Schleifendurchlauf\n";
} while (++$i < 5);
```

Das Programm führt zu folgender Ausgabe:

```
1. Schleifendurchlauf
2. Schleifendurchlauf
3. Schleifendurchlauf
4. Schleifendurchlauf
```

### Schlüsselwörter break und continue

Mit den Schlüsselwörtern `break` und `continue` kann der Schleifendurchlauf innerhalb des Schleifenkörpers gesteuert werden:

 - Mit dem Aufruf des `break` Schlüsselwortes wird eine Schleife umgehend beendet.
 - Mit dem Aufruf des `continue` Schlüsselwortes springt die Schleife umgehend zum nächsten Schleifendurchlauf. Vor dem nächsten Schleifendurchlauf wird jedoch die entsprechende Schleifenbedingung evaluiert.

```php
<?php
for ($i = 1; $i < 999999; $i++) {
    echo $i . " Schleifendurchlauf\n";
    
    if ($i > 4) {
        break;
    }
}
```

Das Programm führt zu folgender Ausgabe:

```
1. Schleifendurchlauf
2. Schleifendurchlauf
3. Schleifendurchlauf
4. Schleifendurchlauf
```
