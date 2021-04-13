# Web-Server

Ein Web-Server ist eine Software zur Verarbeitung von HTTP-Requests. Zu den typischen Aufgaben eines Web-Server gehören:

 - Auslieferung von statischen Dateien
 - Einsatz als Proxy-Server (Forward-Proxy oder Reverse-Proxy)
 - Load Balancing
 - URL Rewriting
 - Virtual Hosting
 - Bereitstellung von TLS
 - HTTP-Request Verarbeitung (zB Ausführung von PHP-Skripten)
 - uvm.

Es gibt sehr viele unterschiedliche Web-Server Software Pakete. Nach Statistiken von [W3Tech](https://w3techs.com/technologies/overview/web_server) dominieren der `Apache HTTP Server` und `Nginx` das WWW.

## Web-Server Architektur

Programme werden innerhalb von Prozessen oder Threads vom Betriebssystem ausgeführt. Programme bestehen aus Instruktionen, welche vom Betriebssystem über den Prozess/Thread auf entsprechenden CPU Kernen ausgeführt werden. Komplexe Programme können aus mehreren Prozessen oder Threads bestehen. Der primäre Grund dafür ist die Möglichkeit Arbeit auf mehre CPU Kerne parallel zu verteilen.

### Blocking Architektur

Web-Server sollen dazu ausgelegt sein tausende von eingehenden HTTP-Requests zu verarbeiten. Eine gängige Architektur in Web-Servern (zum Beispiel Apache HTTP-Server) ist es für jede Verbindung einen Thread oder Prozess zur Bearbeitung bereitzustellen. Am Beispiel des Apache HTTP-Server gibt es zwei gängige Konfigurationen:

 - **[Apache MPM prefork](https://httpd.apache.org/docs/current/en/mod/prefork.html): ** Es gibt einen Kontrollprozess, welcher Kindprozesse (als `Worker` bezeichnet) startet und in einem Pool vorhält. Dabei werden eingehende Verbindungen jeweils von einem Worker-Prozess aus dem Pool bearbeitet. Allgemein spricht man bei der `Apache MPM prefork` Architektur von einer `process-per-connection` Architektur.
 - **[Apache MPM worker](https://httpd.apache.org/docs/current/en/mod/worker.html): ** Ähnlich zum `MPM prefork` gibt es einen Kontrollprozess, welcher Kindprozesse als `Worker` erzeugt. Ein `Worker` erzeugt jedoch jeweils einen `Thread Pool`. Die Verbindungen werden dann von den einzelnen `Threads`, welche von einem `Worker` verwaltet werden, abgearbeitet. Allgemein spricht man bei der `Apache MPM worker` Architektur von einer `thread-per-connection` Architektur.

Diese Architekturen sind grundsätzlich logisch aufgebaut, da jeder Netzwerkverbindung ein Thread oder Prozess gegenübersteht. Die `thread-per-connection` Architektur ist dabei grundsätzlich effizienter als die `process-per-connection` Architektur, da Threads sehr viel leichtgewichtiger sind als Prozesse. Dennoch konsumieren Prozesse und Threads einen beträchtlichen Teil an Betriebssytem Ressourcen. Immer wenn Prozesse vom Scheduler zur Bearbeitung an den CPU Kern zugeteilt werden, verursacht dies einen erheblichen Aufwand (sog. `Context Switch`). Der Skalierbarkeit dieser Architektur sind also gewisse Grenzen gesetzt.

Zusätzlich zu dem Ressourcen-Aufwand des Thread- und Prozess-Handling (zB `Context Switching`) verharren die Prozesse oder Threads oft in einem blockierenden Zustand. Eine Verbindung, welche eben von einem Prozess oder Thread bearbeitet wird, besteht meist aus vielen HTTP-Requests und HTTP-Responses (**HTTP 1.1 Connection Keep-Alive**). Die blockierende Zeit (Wartezeit) zwischen den HTTP-Requests, welche von Clients erzeugt werden, ist erheblich.

### Non-Blocking Architektur

Den Performance Problemen der `process-per-connection` oder `thread-per-connection` basierten Architekturen steht die `non-blocking` `ereignisgesteuerte` Architektur vom Nginx Web-Server entgegen.

Ähnlich zur den Apache Architekturen gibt es einen Kontrollprozess (als `Master` genannt bei Nginx). Dieser `Master` erzeugt `Worker` Prozesse. Dabei gilt die vorgeschlagene Konfiguration, dass die Anzahl der `Worker` der Anzahl an CPU Kernen entsprechen soll (für produktive Konfigurationen).

Nginx verwirft den Ansatz von einem Prozess oder Thread je Verbindung. Ein `Worker` orientiert sich an den HTTP-Requests unabhängig von den Verbindungen. Dies führt dazu, dass kein Blockieren entsteht und `Worker` kontinuierlich "arbeiten" und HTTP-Requests verarbeiten (von unterschiedlichen Verbindungen).

Ebenfalls ist die Anzahl der `Worker` auf die Anzahl der verfügbaren CPU Kerne zu verteilen. Dies führt dazu, dass kein Overhead für den Prozess- oder Thread-Wechsel seitens des Schedulers entsteht. `Worker` sind kontinuierlich mit der Verarbeitung von HTTP-Requests beschäftigt und würden nur in den passiven Zustand wechseln, falls keine Arbeit mehr vorhanden ist.

