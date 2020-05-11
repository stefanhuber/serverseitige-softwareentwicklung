# Nginx Konfiguration

## Allgemeines

Die einzelnen Konfigurationsdateien sind über sog. `Direktiven` organisiert. Es gibt dabei `einfache Direktiven` und `Block-Direktiven`.

Eine `einfache Direktive` besteht dabei aus einem Bezeichner und zugehörigen Konfigurationsparametern. Die Konfigurationsparameter sind über Leerräume getrennt. Die `einfache Direktive` wird über einen Strichpunkt abgeschlossen:

```
name param_1 param_2 ... param_n;
```

Eine `Block-Direktive` hat generell einen ähnlichen Aufbau zur `einfachen Direktive`. Innerhalb von geschweiften Klammern können jedoch noch weitere `einfache Direktiven` enthalten sein:

```
name param_1 param_2 {
    inner_name param_i1 param_i2;
    # ...
}
```

Des Weiteren können innerhalb von `Block-Direktiven` verschachtelt weitere `Block-Direktiven` eingefügt werden. Dadurch kann eine Hierarchie geformt werden:

```
http {
    server {
        xyz p1 p2 …;
    }
}
```

Bei einer `Block-Direktive` spricht man auch von einem Kontext der definiert wird. Dabei werden `einfache Direktiven` bei der Verschachtelung von `Block-Direktiven` nach innen vererbt. Der Äußerste Kontext außerhalb einer `Block-Direktive` wird `main` oder `globaler` Kontext genannt.

Kommentare in Konfigurationsdateien können über die Raute (`#`)  durchgeführt werden. 

Die Hauptkonfiguration von Nginx findet sind in der Datei `nginx.conf`. Diese findet sich innerhalb der Windows Installation im Ordner `conf`.

## Aufbau Konfigurationsdatei

Die Datei `nginx.conf` stellt die Hauptkonfigurationsdatei von Nginx dar. 

```
# Main/Global Kontext

worker_processes  1;

access_log  logs/access.log;
error_log   logs/error.log;
pid         logs/nginx.pid;

http {

    server {
        listen 80;
        server_name example.com;
        root C:\example_root;

        location / {            
        }

        location ~ \.(png|jpeg|jpg) {
        }
    }

    server {
        listen 80;
        server_name mysite.at
        root C:\other_root;
    }
}
```

Die wichtigsten Konfigurationsblöcke für Nginx finden sich innerhalb von `http`, `server` und `location`.

#### http

Die `http` Direktive dient dabei als Abgrenzung auf oberster Ebene zu anderen Funktionen von Nginx. Alles was sich innerhalb der `http` Direktive befindet betrifft ausschließlich die Web-Server Konfiguration. Nginx kann zum Beispiel auch als Mailserver oder Streamingserver verwendet werden (Konfiguration innerhalb `mail` oder `stream` Direktiven).

#### server

Die `server` Blöcke definieren einen `Virtual Host`, welcher über eine IP-Adresse, eine Domain oder einem Port bestimmt wird.

Port und/oder IP-Adresse wird über die `listen` Direktive bestimmt. Die Direktive `server_name` bestimmt die Domain, welche mit dem `Virtual Host` verknüpft sein soll. Die Domain kann dabei auch als Regular Expression angegeben werden.
 
#### location
 
`location` Blöcke finden sicher innerhalb von `server` Blöcken und bestimmen wie einzelne HTTP-Requests verarbeitet werden. Es kann zum Beispiel bestimmt werden, ob:

 - URIs als statische Dateien zurück gegeben werden sollen
 - URIs PHP Skripte ansteuern sollen
 - URIs weitergeleitet werden sollen (Reverse Proxy Funktion)
 - URIs authentifiziert werden sollen
 - uvm.

Eine `location` Direktive hat folgenden Aufbau:

```
location optional_modifier location_match { }
```

Der `location_match` wird als String oder als Regular Expression interpretiert, abhängig vom `optional_modifier`. Einige Beispiele sollen die Varianten aufzeigen:

##### Exact Match

Im Falle von `=` als `optional_modifier` wird ein exakter Match des `location_match` benötigt, dass die URI über die `location` verarbeitet wird. Im unten angegeben Beispiel würde die URI `/special/path` exakt im HTTP-Request angefragt werden müssen, dass die `location` zur Anwendung kommt.

```
location = /special/path {
}
```

##### Prefix

Falls kein `optional_modifier` angegeben wurden, wird der `location_match` als Prefix interpretiert. Im unten angeführten Beispiel würden alle URIs, welche `/example/` als Prefix haben von diesem `location` Block verarbeitet. 

```
location /example/ {
}
```

##### Regex Matching

Der `optional_modifier` `~` oder `~*` bedeuten, dass der `location_match` als case-sensitive oder case-insensitive Regular Expression interpretiert werden soll. In unten angegebenen Beispiel würden alle URIs, welche mit einem Bild-Suffix enden (auch case-insensitive) von der `location` verarbeiten. 

```
location ~* \.(png|gif|ico|jpg|jpeg)$ {
} 
```

## Konfigurationsbeispiele

### Worker Prozesse

Im `globalen Kontext` finden sich Konfigurationen, welche den gesamten Server betreffen zum Beispiel die Anzahl der Worker Prozesse. Mit der Direktive `worker_processes` kann dies bestimmt werden. Aufgrund der Architektur von Nginx sollte innerhalb einer Produktivumgebung die Anzahl der Worker der Anzahl der CPU Kerne entsprechen (Dafür kann der Parameter `auto` verwendet werden).

Wenn die Anzahl der Worker Prozesse zum Beispiel auf `2` gesetzt wird, sieht die Prozessauflistung im Windows Task-Manager in etwa so aus:

```
worker_processes 2;
```

![Nginx Worker Prozesse](images/nginx-worker-processes.png "Nginx Worker Prozesse")

Dort sind 3 Nginx Prozesse ersichtlich. Dabei ist ein Prozess der `Masterprozess`, welcher die Konfiguration und die Verwaltung der `Worker` durchführt. 2 Prozesse sind die `Worker`, welche für die Verarbeitung der HTTP-Requests gestartet wurden.

### Virtual Hosting

Am `localhost` kann durch Einträge in die `Hosts-Datei` die DNS-Auflösung für beliebige Domains statisch durchgeführt werden. Die `Hosts-Datei` ist in folgenden Ordnern zu finden:

 - Windows: `C:\Windows\System32\drivers\etc\hosts`
 - Unix: `/etc/hosts`

Innerhalb der `Hosts-Datei` können folgende Einträge durchgeführt werden:

```
# Kommentare mit #
# IP-Adresse / Hostname über Leerraum getrennt
127.0.0.1	webshop.local
127.0.0.1	websnap.local
```

Dies würde dazu führen, dass die Domains `webshop.local` und `websnap.local` beide mit der IP-Adresse `127.0.0.1` aufgelöst werden.

Innerhalb der `nginx.conf` Datei können nun 2 `Virtual Hosts` eingerichtet werden. Ein `Virtual Host` wird innerhalb der `http` Direktive mit der `server` Direktive eingerichtet. Innerhalb der `server` Direktive werden die Direktiven `listen`, `server_name` und `root` verwendet. Die 3 Direktiven haben folgende Bedeutung:

 - `listen`: Mit `listen` wird spezifiziert auf welchem Port der `Virtual Host` auf eingehende HTTP-Requests reagieren soll.
 - `server_name`: Durch `server_name` werden die Regeln für den Host-Header angegeben für den der `Virtual Host` definiert wurde (siehe `Name-based Virtual Hosting`).
 - `root`: Mit `root` wird der Web-Root angegeben. Von diesem Verzeichnis aus werden Dateien vom Web-Server über den URI-Pfad identifiziert.

```
http {
    # andere Konfigurationen …

    server {
        listen 80;
        server_name webshop.local;
        root C:\examples\webshop;
    }
    server {
        listen 80;
        server_name websnap.local;
        root C:\examples\websnap;
    }
}
```

Falls die beiden Ordner erstellt werden, können jeweils 2 `index.hml` Dateien angelegt werden. In den Dateien sollen jeweils `h1-Überschriften` angelegt werden:

Zum Beispiel würde die Datei `C:\examples\webshop\index.html` in etwa so aussehen:

```html
<h1>Webshop</h1>
```

Falls Nginx gestartet und die Konfiguration neu geladen wurde, können die Websites im Web-Browser aufgerufen werden. Der Port wurde mit `9008` gewählt, da unter Windows auf Port `80` möglicherweise bereits ein Dienst läuft.

<div style="display:flex">
    <img style="flex: 50% 0 0; margin:0 10px; max-width:50%;" src="../images/virtual-hosting-01.png">
    <img style="flex: 50% 0 0; margin:0 10px max-width:50%;" src="../images/virtual-hosting-02.png">
</div>

### Nginx und PHP (FastCGI)

Falls PHP wie [hier](../tutorials/01.md) beschrieben installiert wurde, kann PHP als CGI Modul gestartet werden. Dabei wird PHP als laufender Prozess gestartet. Der Web-Server kann diesen Prozess zur Ausführung von PHP, innerhalb der Verarbeitung eines HTTP-Requests, nutzen.

Mit dem Command `php-cgi` kann PHP als CGI Modul gestartet werden. Mit dem Flag `-b` kann die IP-Adresse bzw. der Port angegeben werden auf den der Socket reagiert:

![FastCGI starten](images/fastcgi.png "FastCGI starten")

!!! info
    Für eine produktive Umgebung ist dieses Vorgehen nicht gedacht. Dazu soll der FastCGI Process Manager (FPM) genutzt werden.

Der `Virtual Host` für die Domain `webshop.local` muss etwas angepasst werden. Dazu werden unter anderem Variablen verwendet, welche von Nginx bereitgestellt werden (Variablen beginnen mit `$`).

Die Variable `$uri` gibt den Pfad, welcher im HTTP-Request angefragt wurde wieder. Die Direktive `try_files` innerhalb der `location` versucht ob es die Datei `$uri` im Web-Root gibt. Falls diese Datei vorhanden ist, wird sie als statische Datei zurückgegeben. Anderenfalls wird die Uri `/index.php?$query_string` ausgewählt. 

Dadurch wird die nächste passende `location` gesucht. Durch die Angabe von `~` wird eine Regex spezifiziert (Uri mit einer `.php` Datei). In dieser `location` wird der HTTP-Request an das CGI-Interface weitergegeben. Dort wird das PHP-Skript ausgeführt und der HTTP-Response erzeugt.

```
http {
    # andere Konfigurationen ...

    server {
        listen 80;
        server_name webshop.local;
        root C:\Pfad\zum\Uebungsblatt-2\public;
        
        location / {
            try_files $uri /index.php?$query_string;
        }		
                        
        location ~ \.php$ {
            fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
            include fastcgi_params;
            fastcgi_pass 127.0.0.1:9123;
        }
    }
```

Das Projekt aus Übungsblatt 2 kann bei entsprechender Konfiguration über `Nginx` ausgeführt werden. Nach Aufruf der Domain `webshop.local` sollte der entsprechende Webshop erscheinen:

![PHP CGI Module](images/virtual-hosting-03.png "PHP CGI Module")
