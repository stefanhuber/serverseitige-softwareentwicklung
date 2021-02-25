# Funktionen

PHP hat über 1000 build-in Funktionen, es können auch eigene Funktionen definiert werden. Eine Funktion ist ein Block aus Befehlen, welcher über einen Funktionsaufruf ausgeführt werden kann.

## Definition und Nutzung

Im unten angeführten Beispiel wird eine Funktion `add` definiert. Diese Funktion kann 2 Parameter erhalten, wobei der zweite Parameter `$b` einen Defaultwert besitzt. Die Funktion kann also ohne Angabe des zweiten Parameters genutzt werden. Funktionen können über das Schlüsselwort `return` ein Ergebnis zurückgeben.

```php
<?php

function add($a, $b=0)
{
    return $a + $b;
}

echo add(3, 5);         // 8
echo add(3);            // 3
echo add(3, add(4, 5)); // 12 
```

## Typdeklaration

Es kann für Funktionsparameter und Rückgabeparameter eine Typdeklaration durchgeführt werden. Die PHP-Laufzeitumgebung versucht die entsprechenden Parameter in den gewünschten Typ zu überführen.

```php
<?php

function add(int $a, int $b=0): int
{
	return $a + $b;
}

echo add(2.555, "5"); // 7
```

Über die Konstante `strict_types` kann festgelegt werden, dass PHP eine strenge Typisierung durchsetzt. Falls ein Parameter mit falschem Datentyp an eine Funktion übergeben wird, wird ein `TypeError` geworfen.

```php
<?php
declare(strict_types=1);

function add(int $a, int $b=0): int
{
	return $a + $b;
}

echo add(2.555, "5"); // ERROR: TypeError
```

## Pass by reference (Referenzparameter)

!!! info "Wichtig"
	Einfache Datentypen und Arrays werden als Kopien an eine Funktion übergeben (`Pass by value`). Objekte werden immer als Referenz an eine Funktion übergeben. Mit vorangestelltem `&` kann jeglicher Parameter als Referenz übergeben.

Da einfache Datentypen als Kopien an eine Funktion übergeben werden, ändert sich durch den Funktionsaufruf der Wert der äußeren Variable `$x` nicht.

```php
<?php

function multiply($a, $multiplier=10) {
	$a = $a * $multiplier;
}

$x = 10;
multiply($x);

echo $x; // 10
```

Durch das vorangestellte `&` wird der Parameter `$a` zu einer Referenz. Die Variable `$x` wird als Referenz übergeben und durch den Funktionsaufruf wird der Wert der äußeren Variable `$x` abgeändert:

```php
<?php

function multiply(&$a, $multiplier=10) {
	$a = $a * $multiplier;
}

$x = 10;
multiply($x);

echo $x; // 100
```

## Scope

Funktionen definieren einen _lokalen Scope_ der alle Variablen beinhaltet, welche innerhalb der Funktion definiert werden. Alle äußeren (globalen) Variablen sind nicht Teil des inneren (lokalen) Scopes und deshalb innerhalb der Funktion nicht nutzbar (default).

Die globale Variable `$a` ist innerhalb des lokalen Scopes der Funktion nicht nutzbar. Somit produziert die Addition den Wert `10`, da `$a` im inneren Scope nicht initialisiert wurde:

```php
<?php
$a = 10;

function sum()
{
    echo $a + 10;
} 

sum(); // Ausgabe: 10
```

Alle Variablen, welche innerhalb des inneren Scopes in einer Funktion genutzt werden müssen und im äußeren Scope definiert wurden, müssen über das Schlüsselwort `global` eingebunden werden.

```php
<?php
$a = 10;

function sum()
{
	global $a;
    echo $a + 10;
} 

sum(); // Ausgabe: 20
```

## Variablen als Funktion ausführen

Variablen die einen String enthalten, welcher einen gültigen Funktionsnamen wiederspiegelt, können ähnlich wie eine Funktion aufgerufen werden. Dies kann vorallem hilfreich sein, wenn dynamisch festgelegt werden soll, welche Funktion ausgeführt werden soll.

```php
<?php

function add($a, $b) {
    return $a + $b;
}

function sub($a, $b) {
    return $a - $b;
}

$func = 'add';
echo $func(20, 10) . "\n"; // 30

$func = 'sub';
echo $func(20, 10) . "\n"; // 10
```

## Anonyme Funktion als Parameter

Funktionen können als Parameter an andere Funktionen übergeben werden. Dabei muss es sich jedoch um anonyme Funktionen handeln. Anonyme Funktionen können entweder in eine Variable gespeichert werden oder direkt als Funktionsparameter definiert werden.

```php
<?php

function calc($a, $b, $func) {
    return $func($a, $b);
}

$add = function ($a, $b) {
    return $a + $b;
}; // WICHTIG: ; muss hier angegeben werden, Variablendeklaration

echo calc(20, 10, $add) . "\n"; // 30
echo calc(20, 10, function ($a, $b) { return $a - $b; }) . "\n"; // 10
```
