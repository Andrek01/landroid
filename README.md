# landroid using pyworxcloud from MTrab

Landroid-Plugin (proof of concept)

## Table of Content

1. [Changelog](#changelog)
2. [Generell](#generell)


## Changelog <a name="changelog"/></a>


### 2022-05-23

- bugfix für parsen des Exclusion Kalenders
- Erweiterung der structs um rain_sensor_triggered / rain_delay_time_remaining

### 2022-05-20
- Verwendung von Pyworxcloud 1.4.20
- requirements auf pyworxcloud == 1.4.20 gesetzt
- Verwendung von TryToPoll()
- worx.update() entfernt
- Wetter auskommentiert
- Kalenderfunktionen für mähen nach UZSU hinzugefügt
- Parsen und Anzeigen der Ausschluss und Mähkalender  (editieren nicht möglich)
- MQTT-Befehl "_ots" durch worx.startEdgecut() ersetzt
- Button für Edgecut in Visu eingebaut
- Kalender in Visu eingebaut

## Widgets für die Kalender des Mowers

### Kalender für die Ausschlusszeiten (nur Anzeige - Änderungen werden durch die Serverdaten bei Aktualisierung überschrieben)
<pre>
<code>
{{device.uzsutable('','worx.visu.exclusion_uzsu.uzsu','Mäher-Ausschlusszeiten',['1','3'],['2','4'],['orangered','blue'],['red','red'],'','','true','5m','solid','true','true','true','true','1','list',['Generic-ON:1','Generic-OFF:2','Beregnung-ON:3','Beregnung-OFF:4'])}}
</code>
</pre>


### Kalender für die App-Mähzeiten (nur Anzeige - Änderungen werden durch die Serverdaten bei Aktualisierung überschrieben)

<pre>
<code>
{{device.uzsutable('','worx.visu.app_mow_uzsu.uzsu','Mähzeiten App-Kalender',['0','1'],'2',['greenyellow','green'],'red','','','true','5m','solid','true','true','true','true','1','list',['Mähen:0','Mähen+Kante:1','Mähen-Ende:2'])}}
</code>
</pre>

### Kalender für Mähen nach UZSU

>Falls zunächst die Kante geschnitten werden soll und im Anschluss komplett gemäht werden soll kann wie folgt
>parametriert werden.
>Es kann eine Schaltzeit für Kante mähen um 9:00 angegeben werden, die Startzeit für Mähen wird dann auf 9:15 gesetzt
>mähen beenden auf 15:00

<pre>
<code>
{{device.uzsutable('','worx.visu.mow_control_uzsu.uzsu','Mähen nach UZSU',['1','2'],'0',['greenyellow','green'],'red','','','true','5m','solid','true','true','true','true','1','list',['Mähen:1','Kante:2','Ende:0'])}}
</code>
</pre>

# Inbetriebnahme
## --------------------------------------
- Im Order "/items" ist eine Beispiel-Item-Datei
- im Ordner "/plugins" ist das komplette Plugin. Diesen in /smarthome/plugins/landroid kopieren
- Die Rechte auf den  Plugin + Item Order prüfen und eventuell korrigieren
- Die Einträge in der /etc/plugin.yaml ergänzen
- shNG neu starten
## --------------------------------------


## Generell <a name="generell"/></a>

Die Daten des Plugin müssen in den Ordner /usr/local/smarthome/plugins/landroid/ (wie gewohnt)
Die Rechte entsprechend setzen.

Das Plugin muss in der /etc/plugin.yaml eingefügt werden :

<pre>
<code>
landroid:
    plugin_name: landroid
    class_path: plugins.landroid
    parent_item: worx
    cycle: 120   # Intervall für Update vom Mäher = 120 Sekunden
    landroid_user: XXXXXXXXXXXXXXXXXX
    landroid_pwd:  YYYYYYYYYYYYYYYYYY
</code></pre>

Der Parameter "cycle" ist die Poll-Rate, wie oft die Daten von der IOT-Cloud abgerufen werden sollen.
User und PWD sind im Klartext zu hinterlegen.
Das "parent"-item ist für die vordefinierten Structs.

Im Ordner /items muss eine Item-Datei mit folgendem Inhalt erstellt werden (landroid.yaml).
Die items werden über structs aus der plugin.yaml des plugins erstellt.
<pre>
<code>
%YAML 1.1
---
worx:
    struct: landroid.child
</code></pre>

**Falls ein anderer Name für das parent-Item genutzt werden soll, muss dieser
in der /etc/plugin.yaml gleichlautend zum Eintrag in der /items/landroid.yaml eingetragen werden**
