# Arduino-Pro-Mini-RF

### Hallo Freunde der drahtlosen Hausautomation,

nachdem ich täglich mit Leiterplatten Schaltungs- und Layout-Entwicklung zu tun habe und dabei auch immer den Bestückungs- und Lötprozess begleite, hat mich interessiert, welches Ergebnis JLCPCB bei der Bestückung liefert.
Nun habe ich also überlegt, mit welcher Schaltung/Bestückung man den SMT Service von JLCPCB einmal testen könnte.

Als ich mir die Projektseite von [asksinpp.de](https://asksinpp.de/Projekte/) noch einmal angeschaut habe, fiel mir auf, dass ungefähr die Hälfte aller Projekte folgende Komponenten enthält:

1.	Arduino Pro Mini 3,3V/8MHz
2.	CC1101 Modul
3.	Config-Taster
4.	Status LED

#### Da stellte sich mir die Frage – warum das alles nicht vereinen, wenn es sowieso fast immer benötigt wird?

Damit könnte man schneller Prototypen auf Lochraster aufbauen und es wäre Anfängerfreundlich.

###### Warum ist auf die Idee noch keiner gekommen?
Ich vermute der geringe Preis von Arduino Pro Mini und CC1101 Modul bei Import aus China stellt die Wirtschaftlichkeit meiner Kobination in Frage.
Sobald man aber in Deutschland kauft, ist eine All-in-One Leiterplatte durchaus konkurrenzfähig.

#### Was ist daraus geworden? Der Arduino-Pro-Mini-RF

![Full_Assy_Iso.jpg](https://github.com/Asselhead/Arduino-Pro-Mini-RF/blob/master/Images/Full_Assy_Iso.jpg)

Alle bisherigen Leiterplatten die den Arduino Pro Mini verwenden, könnten auch mit der RF Version weiter betrieben werden, benötigen aber kein zusätzliches CC1101 Modul.
Zukünftige "Baseboards" könnten kleiner ausfallen, da der Platz für das CC1101 Modul entfällt.

## Was habe ich gemacht?

Ich habe mir als Basis die Schaltung des Arduino Pro Mini (von Sparkfun) genommen.
Hier habe ich zunächst den Mikrocontroller ATMEGA328P-AU (TQFP-32) geändert in den ATMEGA328P-MU (HVQFN-32), da dieser Funktionsgleich, aber auf Grund seines Gehäuses deutlich kleiner ist.

### Aus der Arduino Pro Mini Schaltung habe ich folgendes entfernt:

1.	Reset Taster
2.	User LED
3.	Power LED
4.	Anschlüsse für A/D-Wandler Eingänge A6 und A7 (aus Platzgründen)

### Folgendes wurde ausgetauscht:

-	3,3V Spannungsregler MIC5205 wurde durch Type MCP1703T-3302E/CB
ersetzt. Dieser hat ein sehr geringes IQ (2µA), eine Eingangsspannung bis 16V, kann (max.) 250mA und hat ein kleines SOT-23A Gehäuse. (Wird nur bei Bedarf bestückt)

### Folgendes wurde hinzugefügt:

1.	Texas Instruments CC1101RGPR mit RF-Frontend
2.	Status LED (an D4)
3.	Config Button (an D8)
4.	Reset Baustein (Open Drain) als Babbling Idiot Protection (derzeit 2,32V – als Bestückungsoption)
5.	SHT31 Temp./Humi Sensor – Achtung! Auf der BOT Seite - nur als Option für versierte Selbstlöter.


Die Belegung der beiden Stiftleisten links und rechts ist identisch zum Arduino Pro Mini Standard, ebenso die Position von A4(SDA) und A5(SCL) für den I2C Bus Anschluss.

Die A/D-Wandler Eingänge A6 und A7 mussten leider entfallen, da der Platz nicht ausgereicht hat.

Die Leiterplatte hat genau wie das Original die Abmessungen 17,78 x 33,02mm (bzw. 0,7“x1,3“).

#### Leider muss man bei der Bestückung durch JLCPCB einige Einschränkungen hinnehmen:

1.	Es wird kein passender Keramik Resonator mit 8 MHz angeboten – dieser muss derzeit von Hand nachbestückt werden, sofern man nicht mit der Genauigkeit des internen Oszillators auskommt. Dazu wurden die Lötpads geringfügig vergrößert, damit es von Hand etwas leichter fällt.
2.	Es wird kein kleiner Taster zur Bestückung angeboten – dieser muss derzeit ebenfalls von Hand nachbestückt werden.
3.	Für die beste Performance des CC1101 Chip werden Kondensatoren und Induktivitäten benötigt, die entweder nicht verfügbar sind, oder als „Extended Part“ eine zusätzlich Einrichtungsgebühr zur Folge haben. (Dies ist für unsere Zwecke aber nicht unbedingt ein großer Nachteil, denn die CC1101 Module verwenden ähnliche Bauteile)


Da in vielen Fällen ein Batteriebetrieb gewünscht ist, habe ich den Spannungsregler und dessen Eingangs-Kondensator nicht bestücken lassen.
Wer die Schaltung z.B. an 12V betreiben möchte, muss einen Kondensator (Bauform 0603 – mindestens 16V Type) und den Spannungsregler (SOT-23) nachträglich bestücken.
Wenn sich hier herausstellt, dass die Mehrheit die Version mit bestücktem Spannungsregler benötigt, kann dieser (& Kondensator) automatisch mit bestückt werden.
