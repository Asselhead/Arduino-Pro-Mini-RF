# Arduino-Pro-Mini-RF

### Hallo Freunde der drahtlosen Hausautomation,

da ich fast täglich mit Leiterplatten Schaltungs- und Layout-Entwicklung zu tun habe und dabei auch immer ein wenig den Bestückungs- und Lötprozess begleite, hat mich interessiert, welches Ergebnis JLCPCB bei der Bestückung liefert.
Nun habe ich überlegt, mit welcher Schaltung/Bestückung man den SMT Service von JLCPCB einmal testen könnte.

Als ich mir die Projektseite von [asksinpp.de](https://asksinpp.de/Projekte/) noch einmal angeschaut habe, fiel mir auf, dass ungefähr die Hälfte aller Projekte folgende Komponenten enthält:

1.	Arduino Pro Mini 3,3V/8MHz
2.	CC1101 Modul
3.	Config-Taster
4.	Status LED

#### Da stellte sich mir die Frage – warum das alles nicht vereinen, wenn es sowieso fast immer benötigt wird?

Damit könnte man schneller Prototypen auf Lochraster aufbauen und es wäre eine anfängerfreundliche Lösung.

###### Warum ist auf die Idee noch keiner gekommen?
Ich vermute, der geringe Preis von Arduino Pro Mini und CC1101 Modul stellt bei Import aus China, die Wirtschaftlichkeit meiner Kombination in Frage.
Sobald man aber in Deutschland kauft, ist eine "All-in-One" Leiterplatte durchaus konkurrenzfähig.

#### Was ist daraus geworden? Der Arduino-Pro-Mini-RF!

![Full_Assy_Iso.jpg](https://github.com/Asselhead/Arduino-Pro-Mini-RF/blob/master/Images/Full_Assy_Iso.jpg)

Alle bisherigen ASKSINN++ Leiterplatten die den Arduino Pro Mini verwenden, könnten auch mit der RF Version weiter betrieben werden, benötigen aber kein zusätzliches CC1101 Modul.
Zukünftige "Baseboards" könnten kleiner ausfallen, da der Platz für das CC1101 Modul entfällt.

## Was habe ich gemacht?

Ich habe als Basis die Schaltung des Arduino Pro Mini (von Sparkfun) genommen.
Hier habe ich zunächst den Mikrocontroller ATMEGA328P-AU (TQFP-32) in den ATMEGA328P-MU (HVQFN-32) geändert, da dieser funktionsgleich, aber aufgrund seines Gehäuses deutlich kleiner ist.

### Aus der Arduino Pro Mini Schaltung habe ich entfernt:

1.	Reset Taster
2.	User LED
3.	Power LED
4.	Anschlüsse für A/D-Wandler Eingänge A6 und A7 (aus Platzgründen)

### Es wurde ausgetauscht:

-	3,3V Spannungsregler MIC5205 wurde durch Type MCP1703T-3302E/CB
ersetzt. Dieser hat ein sehr geringes IQ (2µA), eine Eingangsspannung bis 16V, kann (max.) 250mA und hat ein kleines SOT-23A Gehäuse. (Wird nur bei Bedarf bestückt)

### Es wurde hinzugefügt:

1.	Texas Instruments CC1101RGPR mit RF-Frontend
2.	Status LED (an D4)
3.	Config Button (an D8)
4.	Reset Baustein (Open Drain) als Babbling Idiot Protection (derzeit 2,32V – als Bestückungsoption)
5.	SHT31 Temp./Humi Sensor – Achtung! Auf der BOT Seite - nur als Option für versierte Selbstlöter.

### Ist er kompatibel?

Die Belegung der beiden Stiftleisten links und rechts ist identisch zum Arduino Pro Mini Standard, ebenso die Position von A4(SDA) und A5(SCL) für den I2C Bus Anschluss.

Die A/D-Wandler Eingänge A6 und A7 mussten leider entfallen, da der Platz nicht ausgereicht hat.

Die Leiterplatte hat genau wie das Original die Abmessungen 17,78 x 33,02mm (bzw. 0,7“x1,3“).

#### Leider muss man bei der Bestückung durch JLCPCB einige Einschränkungen hinnehmen:

1.	Es wird kein passender Keramik Resonator mit 8 MHz angeboten – dieser muss derzeit von Hand nachbestückt werden, sofern man nicht mit der Genauigkeit des internen Oszillators auskommt. Dazu wurden die Lötpads geringfügig vergrößert, damit es von Hand etwas leichter fällt.
2.	Es wird kein kleiner Taster zur Bestückung angeboten – dieser muss derzeit ebenfalls von Hand nachbestückt werden.
3.	Für die beste Performance des CC1101 Chip werden Kondensatoren und Induktivitäten benötigt, die entweder nicht verfügbar sind, oder als „Extended Part“ eine zusätzlich Einrichtungsgebühr zur Folge haben. (Dies ist für unsere Zwecke aber kein großer Nachteil, denn die CC1101 Module verwenden ähnliche Bauteile und erste Tests mit der Leiterplatte haben gezeigt, dass die Reichweite des Arduino-Pro-Mini-RF hervorragend ist.)


Da in vielen Fällen ein Batteriebetrieb gewünscht ist, habe ich den Spannungsregler und dessen Eingangs-Kondensator nicht bestücken lassen.
Wer die Schaltung z.B. an 12V betreiben möchte, muss einen Kondensator (Bauform 0603 – mindestens 16V Type) und den Spannungsregler (SOT-23) nachträglich bestücken.
Wenn sich hier herausstellt, dass die Mehrheit die Version mit bestücktem Spannungsregler benötigt, kann dieser (& Kondensator) automatisch mit bestückt werden.

## What you get!

Dieser Screenshot zeigt, was alles automatisch bestückt werden kann:

![Part_Assy_Iso.jpg](https://github.com/Asselhead/Arduino-Pro-Mini-RF/blob/master/Images/Part_Assy.jpg)

## What you don´t get!

Es fehlen, wie bereits erwähnt, der LDO-Spannungsregler mit Eingangskondensator, der Reset Baustein, der Keramik Resonator und der Taster.

Den Config Taster zu haben ist schon sehr komfortabel, er lässt sich auch recht leicht von Hand Bestücken. Auch der Keramik Resonator könnte für Präzisions-Zwecke von dem ein oder anderen benötigt werden. Dessen Lötpads sind etwas größer ausgeführt, damit er leichter verlötet werden kann.

Der Reset Baustein (IC4) ist eine Option mit der ggf. ein Babbling Idiot verhindert werden kann.

Wird die Schaltung nicht mit 3V an Batterien betrieben, sondern an einer höheren Spannung (bis ca. 14,5V), dann muss der LDO Spannungsregler (IC2) und dessen Eingangskondensator (10µF/16V/0603) bestückt werden.

## Funktioniert das überhaupt?

Nachdem ich die ersten Muster bekommen habe und die Antenne angelötet habe, habe ich den [MCUDude MiniCore Bootloader](https://github.com/MCUdude/MiniCore) per ISP Programmer aufgespielt.
Die Einstellungen habe ich folgendermaßen gewählt:

![Bootloader.png](https://github.com/Asselhead/Arduino-Pro-Mini-RF/blob/master/Images/Bootloader.png)

Danach habe ich per FTDI USB-Seriell Adapter den genialen [FreqTest](https://github.com/pa-pa/AskSinPP/blob/master/examples/FreqTest/FreqTest.ino) Sketch aufgespielt.
Das Ergebnis sieht recht vielversprechend aus:
```
AskSin++ V4.1.6 (Aug 28 2020 13:03:21)
CC init1
CC Version: 14
 - ready
Start searching ...
Freq 0x21656A 868.300 MHz: 345682.  1 / -57dBm
Search for upper bound
Freq 0x21657A 868.306 MHz: 00AC01.  1 / -63dBm
Freq 0x21658A 868.313 MHz: B28FEB.  1 / -54dBm
Freq 0x21659A 868.319 MHz: 69677B.  1 / -50dBm
Freq 0x2165AA 868.325 MHz: B28FEB.  1 / -54dBm
Freq 0x2165BA 868.332 MHz: 69677B.  1 / -51dBm
Freq 0x2165CA 868.338 MHz: 69677B.  1 / -52dBm
Freq 0x2165DA 868.344 MHz: 003F00.  1 / -79dBm
Freq 0x2165EA 868.351 MHz:   0
Search for lower bound
Freq 0x21655A 868.294 MHz: 6185C2.  1 / -77dBm
Freq 0x21654A 868.287 MHz: B28FEB.  1 / -52dBm
Freq 0x21653A 868.281 MHz: 69677B.  1 / -54dBm
Freq 0x21652A 868.274 MHz: B28FEB.  1 / -52dBm
Freq 0x21651A 868.268 MHz: 69677B.  1 / -50dBm
Freq 0x21650A 868.262 MHz: 00AC01.  1 / -65dBm
Freq 0x2164FA 868.255 MHz: 35CD83.  1 / -52dBm
Freq 0x2164EA 868.249 MHz: B28FEB.  1 / -52dBm
Freq 0x2164DA 868.243 MHz:   0

Done: 0x2164EA - 0x2165DA
Calculated Freq: 0x216562 868.297 MHz
Store into config area: 6562...stored!
```

Als "echten" Sketch habe ich dann den [HM-RC-P1](https://github.com/pa-pa/AskSinPP/blob/master/examples/HM-RC-P1/HM-RC-P1.ino) vom 1-Tasten Paniksender programmiert.
Dem Arduino-Pro-Mini-RF habe ich dann auf einem Mini-Steckbrett noch schnell eine Taste spendiert, anschließend in meiner (immer noch) CCU2 ein Programm Taste->Terrassenbeleuchtung angelegt und mit dem "Aufbau" in den Garten gegangen. Funktioniert Prima. Dann durchs Törchen ins Feld - so ca. 40m Luftlinie von der CCU2 entfernt - Empfang -> kein Problem.

Damit wäre schon einmal klar - wenn man sich einigermaßen an die App-Notes von TI hält, funktioniert der CC1101 immer.
