# Beispiele

Reguläre Ausdrücke können mit unterschiedlichen Werkzeugen getestet und erstellt werden. Das Werkzeug [Regular Expressions 101](https://regex101.com/) kann dazu nur empfohlen werden.

### Alle Vorkommen des Namens Maier

#### Über Zeichenklassen

```
M[ae][iy]er
```

#### Über Alternativen und Gruppierung

```
M(?:ai|ei|ay|ey)er
```

### Email-Adresse (einfach)

```
[a-zA-Z_.\-]+@[a-z\-]*\.[a-z]{2,}
```

### IP-Adresse v4 (einfach)

```
\d{1,3}\.\d{1,3}\.\d{1,3}\.\d{1,3}
```

### Farbangabe als Hex-Wert

```
#(?:[a-fA-F0-9]{3}){1,2}
```

### Eine Zahl größer 15

```
(?:1[6-9]|[2-9]\d|\d{3,})
```

```
(?:1(?:[6-9]|\d{2,})|[2-9]\d+)
```