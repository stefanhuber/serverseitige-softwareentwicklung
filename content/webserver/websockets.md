# Websockets

`WebSockets` sind `Client-initiierte` `persistente` `full-duplex` Kommunikationskanäle zwischen Web-Browser und Web-Server.

Der Web-Browser initiert den `WebSocket` über einen sog. WebSocket Handshake Request. Falls der Web-Server das `WebSocket Protokoll` unterstützt antwortet dieser mit einem `101 Status` und die bestehende TCP-Verbindung wird für den `WebSocket` verwendet.

Der Web-Browser würde beispielsweise folgenden HTTP-Request als `WebSocket Handshake Request` an den Web-Server senden:

```
GET ws://localhost:9080/ HTTP/1.1
Host: localhost:9080
Connection: Upgrade
Upgrade: websocket
```

Dies entspricht einem klassischen GET-Request. Wichtig ist, dass die URI mit dem Protokoll `ws` beginnt. Zusätzlich werden die HTTP-Header `Connection: Upgrade` und `Upgrade: websocket` angegeben. Dies soll den Web-Server veranlassen, dass eben eine WebSocket Verbindung aufgebaut wird.

Der Web-Server würde, falls er das Protokoll unterstützt, folgenden HTTP-Response senden:

```
HTTP/1.1 101 Switching Protocols
Upgrade: websocket
Connection: Upgrade
```

Der Status `101` würde hier bedeuten, dass das HTTP-Protkoll gewechselt wird und entsprechende das `WebSocket-Protokoll` verwendet wird.

## Web-Browser API

In allen modernen Web-Browsern wird die WebSocket API unterstützt. Die WebSocket API bietet die Möglichkeit asynchrone Nachrichten vom Web-Server über die Angabe einer JavaScript Funktion abzuarbeiten.

Es gibt unterschiedliche WebSocket Events, welche im Web-Browser eintreten können und für welche entsprechende Handler definiert werden müssen:

 - **onmessage:** Event für den Eingang einer Nachricht vom Web-Server
 - **onconnection:** Event für den erfolgreichen Verbindungsaufbau mit dem Web-Server
 - **onerror:** Event für das Auftreten eines Fehlers zum Web-Server
 - **onclose:** Event für das Schließen einer 

Die Methode `send` der `WebSocket-Connection` kann im Web-Browser genutzt werden um Nachrichten an den Web-Server zu senden.

Folgend wird ein einfaches Beispiel gegeben wie `WebSockets` im Web-Browser genutzt werden können:

```JavaScript
let connection = new WebSocket('ws://localhost:8080'); 
// wss://... für verschlüsselte Kommunikation

connection.onmessage = function(e) {
    // über einen Eventhandler (JavaScript Funktion), werden
    // Nachrichteneingänge im Client abgearbeitet (asynchron)
    console.log("message from server: " + e.data);
};

// Nachrichten können an den Server gesendet werden
connection.send("Hello I'm here!");
```

## WebSockets in PHP

PHP hat keine native `WebSocket` API. Es gibt jedoch unterschiedliche Bibliotheken, welche einfache Interfaces bereitstellen um `WebSockets` zu implementieren. Die Bibliothek [`Ratchet`](http://socketo.me/) ist ein Beispiel für eine PHP-Bibliothek zur Implementierung von `WebSockets`.

Im Prinzip muss ein Interface implementiert werden, das ähnliche Methode wie die JavaScript API aufweist:

```php
<?php

namespace App;

use Ratchet\MessageComponentInterface;
use Ratchet\ConnectionInterface;

class Chat implements MessageComponentInterface
{
    public function onOpen(ConnectionInterface $conn)
    {
    }

    public function onMessage(ConnectionInterface $from, $message)
    {
    }

    public function onClose(ConnectionInterface $conn)
    {
    }

    public function onError(ConnectionInterface $conn, \Exception $e)
    {
    }
}
```

## Einfaches Beispiel

Durch die Web-Server Architektur von Nginx werden `WebSockets` unterstützt. Innerhalb eines bereitgestellten [`Git-Repository`](https://github.com/stefanhuber/simple-websocket-demo) wurde ein Chat mittels `WebSockets` implementiert.

Das Projet besteht aus einer `index.html`, welche unter anderem den notwendigen JavaScript Code für die `WebSocket` Anbindung am Client enthält. Ebenfalls findet sich eine Implementierung `MessageComponentInterface` in PHP für die serverseitige Implementierung des Chats. Dises Programm wird als PHP Dienst am Web-Server gestartet.

Für das Projekt werden 2 Domänen benötigt. Einerseits die Domain `chat.local`, welche die `index.html` Datei über das HTTP-Protokoll ausliefert. Andererseits die Domain `socket.chat.local`, welche für `WebSocket` Datenverkehr verwantwortlich ist. In der `Hosts-Datei` müssen dazu folgende Domänen ergänzt werden:

```
# Kommentare mit #
# IP-Adresse / Hostname über Leerraum getrennt
127.0.0.1   chat.local
127.0.0.1   socket.chat.local
```

Nginx wird für die `WebSocket` Anbindung als Reverse Proxy konfiguriert und leitet die `WebSocket` Komminkation an das entsprechende PHP-Programm weiter:

```
http {
    server {
		listen 80;
		server_name chat.local;
		root C:\examples\web-socket-demo\public;
	}
	
	server {
		listen 80;
		server_name socket.chat.local;
	
		location / {
			proxy_pass http://localhost:9080;
			proxy_http_version 1.1;
			proxy_set_header Upgrade $http_upgrade;
			proxy_set_header Connection "upgrade";
		}	
	}
}
```

Zum Start und zur Ausführung des Projektes müssen die Schritte in der README befolgt werden. Eine Chat-Konversation zwischen `Alice` und `Bob` würde folgendermaßen aussehen. Die ältesten Nachrichten stehen jeweils unten und `Alice` war bereits im Chatfenster als `Bob` sich einloggte:

<div style="display:flex">
    <img style="flex: 50% 0 0; margin:0 10px; max-width:50%;" src="../images/websocket-chat-01.png">
    <img style="flex: 50% 0 0; margin:0 10px max-width:50%;" src="../images/websocket-chat-02.png">
</div>
