# Sessions und Cookies

!!! info "Wichtig"
    Das Erzeugen von Cookies bzw. einer Session muss vor jeglicher Ausgabe des PHP-Programms passieren (Zum Beispiel vor jedem `echo` Statement). Die Funktionen `setcookies` oder `session_start` sollten demnach so früh wie möglich im PHP-Programm aufgerufen werden.
    
## Cookies

### Cookies erzeugen

PHP bietet die Funktion `setcookie`, damit können Cookies am Client erzeut werden. Die `Set-Cookie` Header werden dadurch über den HTTP-Response gesetzt. Alle wichtigen Cookie-Parameter können dadurch gesetzt werden:

 1. Parameter: Cookie Name
 2. Parameter (optional): Cookie Wert; default als `""`
 3. Parameter (optional): Angabe eines Zeitstempels in Sekunden mit einem Ablaufdatum bis dahin das Cookie gültig ist; Keine Angabe lässt das Cookie bis zum Schließen des Web-Browsers gültig sein (`0`)
 4. Parameter (optional): Angabe eines URI Pfades innerhalb welchem das Cookie gültig ist; Default ist der String `""` für alle Pfade
 5. Parameter (optional): Angabe einer Subdomain, welche in die Gültigkeit eines Cookie gehört; Default ist der String `""` für alle Subdomains
 6. Parameter (optional): Boolean der angibt, ob das Cookie nur über HTTPS übertragen wird; default ist `false`
 7. Parameter (optional): Boolean der angibt, ob das Cookie `HTTP-only` ist, also nicht über JavaScript im Client veränderbar; default ist `false`

```php
<?php

setcookie(
  "MyCookie" ,              // Cookie Name
  "some value" ,            // Cookie Wert
  time() + 3600 ,           // 1h Lebensdauer
  "" ,                      // Pfad Gültigkeit   "" = default
  "subdomain.example.com" , // Domain Gültigkeit "" = default
  true ,                    // Secure, HTTPS
  true                      // httpOnly, kein Ändern am Client
);
```

### Cookies auslesen

Innerhalb der Superglobal `$_COOKIES` sind alle Cookies als Schlüssel/Wert Paare hinterlegt. Alle Cookies welche vom Client gesendet wurden, können dadurch ausgelesen werden.

Im folgenden Beispiel wird das assoziative Superglobal Array `$_COOKIES` mit einer `foreach` Schleife ausgelesen:

```php
<?php
foreach($_COOKIE as $key => $value) {
    echo $key . ": " . $value;
}
```

## Sessions

PHP Sessions werden entweder automatisch oder manuell gestartet. Falls in der `php.ini` die Konfiguration `session.auto_start` mit `1` belegt ist, werden Sessions automatisch für jeden Client erzeugt. Andernfalls muss eine Session explizit über die Methode `session_start` erzeugt werden.

Clients einer Session werden über Cookies identifiziert. Der Name des Cookies ist default mit `PHPSESSID` spezifiziert (siehe `php.ini`). Per default werden Sessions am Dateisystem des Servers gespeichert. Dies kann natürlich geändert werden, sodass Sessions zB auch in Datenbanken gespeichert werden können.

Mit der Funktion `session_start` wird eine bestehende Session (über Cookie übergeben) weitergeführt oder eine neue gestartet. In der Superglobal `$_SESSION` können Schlüssel/Wert Paare abgelegt, gelesen und verändert werden, welche zur entsprechenden Session gehören. Die Gültigkeit der Session hängt mit der Gültigkeit des Cookies zusammen. Default ist die Gültigkeit bis zum Schließen des Web-Browsers definiert.

```php
<?php

// starten der Session vor jeglicher Ausgabe des PHP-Programms:
session_start();

// Ändern von Session Inhalten:
$_SESSION['cart'] = [
    "product-1" => 5,
    "product-2" => 3
]

// Ausgabe der Session Inhalte:
foreach ($_SESSION['cart'] as $key => $value) {
    echo $key . " " . $value . "\n";
}
```


