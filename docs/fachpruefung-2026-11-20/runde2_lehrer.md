# Runde 2 – Stellungnahme Elektrotechniklehrer

Grundlage: meine Prüfung (`lehrer.md`), der Bericht des Elektromeisters (`meister.md`), der Bericht des Elektrikers (`elektriker.md`) und Patricks Foto eines echten Zählerschranks (`/mnt/user-data/uploads/1fb1f1e0-image.png`).
Wie sicher die Quellen sind: **[N]** heißt Norm- und Fachbuchwissen, **[W]** heißt im Web belegt (in Runde 1), **[U]** heißt unsicher.

---

## 0. Was das Foto zeigt und was es für die App bedeutet

Zu sehen ist ein zweifeldriger Zählerschrank nach VDE-AR-N 4100. Ich beschreibe nur, was man erkennt. Was ich deute, ist mit [U] markiert.

- **Linkes Feld, unten (netzseitiger Anschlussraum):** fünf waagrechte Kupfer-Sammelschienen. Darauf sitzen mit Adapter ein 3-poliger SLS, daneben ein Überspannungsschutz und rechts ein kleines 1-poliges Gerät. Das ist vermutlich der Spannungsabgriff für den Zusatzraum bzw. das Gateway [U]. Unten links führt eine grün-gelbe Ader vom Schienensystem weg. Das passt zu PEN-Aufteilung und Erdung, ist aber nicht sicher zu erkennen [U].
- **Linkes Feld, Mitte:** Zählerplatz mit BKE-Adapter. Die Adern für den Zähler (braun, schwarz, grau, blau) sind noch nicht angeschlossen, der Zähler fehlt.
- **Linkes Feld, oben (anlagenseitiger Anschlussraum):** zwei 5-polige Abzweigklemmen, eine davon mit **„NOT“** beschriftet, dazu ein grün-gelber PE-Leiter. Daraus folgt, dass von hier die Zuleitungen abgehen, nicht direkt vom Zähler.
- **Rechtes Feld:** ein Verteilerfeld mit N-/PE-Klemmen oben, Schützen bzw. Relais mit Anzeige, 4-poligen Geräten und N-Klemmen. Unten rechts liegt das Feld für Zusatzanwendungen (APZ/RfZ) mit Datenleitung.
  - **[U]:** Die Beschriftung „NOT“ und die Schütze sprechen für eine **Netz-/Notstrom-Umschaltung**. Ist das so, gibt es eine zweite Einspeisung. Die muss bei den 5 Sicherheitsregeln mitgedacht werden (Freischalten aller Einspeisungen, Rückspeisung).

**Didaktische Folgen:**
1. Den Aufbau von unten nach oben (netzseitiger Anschlussraum, Zählerplatz, anlagenseitiger Anschlussraum) zeigt die App **richtig**. Das Foto bestätigt es.
2. Der geführte Lichtkreis verbindet aber **Zähler direkt mit FI**. Auf dem Foto geht es über die **Abzweigklemme oben**. Die `hlak` sollte deshalb in den geführten Aufbau, sonst lernt man einen Weg, den es in echt so nicht gibt.
3. Das Feld für Zusatzanwendungen (Gateway, Steuerbox) und das Verteilerfeld im Zählerschrank kommen in der App nicht vor. Für das Verständnis reicht ein Satz in `INFO.zs`.
4. Wegen der möglichen Notstrom-Einspeisung kommt in die 5 Regeln der Zusatz „alle Einspeisungen (Netz, Notstrom, PV, Speicher)“.

---

## 1. Wo ich zustimme

| Punkt | Meister | Elektriker | Meine Haltung |
|---|---|---|---|
| Keine Prüfung LS-Nennstrom gegen Querschnitt (I_B ≤ I_n ≤ I_z) | A1 | A1 | **Volle Zustimmung, Einstufung GEFÄHRLICH.** Das habe ich in Runde 1 nur am Rand (C7) erwähnt. Es gehört nach oben: Das ist **die** klassische Brandursache, und die App lässt B20 an 1,5 mm² ohne Meldung zu. [N] |
| Dose wählt 1,5 mm² hinter einer 2,5-mm²-Zuleitung | – | A2 | Zustimmung. Was die App voreinstellt, darf ihre eigene Prüfung nicht bemängeln. Für Laien gilt die Voreinstellung als richtig. |
| 300-mA-FI: „Du bist geschützt“ | A2 | – | Deckt sich mit meinem A1. |
| Offener N zeigt „Kein Messwert“ | A3 | – | Deckt sich mit meinem A2. Die Logik-Korrektur des Meisters (Last als hochohmige Kante für die Potentialbestimmung) ist der richtige Weg. |
| Haupterdungsschiene, Schutzpotentialausgleich, Erder fehlen | A4 | D12 | Zustimmung. In Runde 1 stand das bei mir unter „missverständlich“ (C10). Der Meister stuft es zu Recht höher: Eine Anlage ohne Schutzpotentialausgleich ist nicht in Ordnung, und die App sagt „keine Fehler“. [N] |
| Hauptleitung 4-adrig bei PEN-Aufteilung im netzseitigen Anschlussraum; 5×10 bei E50/E63 zu klein | A5, C1 | – | Zustimmung (EAM Netz [W]). |
| Kundendoku: SPD als „nicht vorhanden“ ausgewiesen | B1 | – | Zustimmung. Das ist ein Programmfehler mit falscher Aussage gegenüber dem Kunden. |
| Fehlender SPD als `warn` statt `hint` | B2 | – | Zustimmung. |
| PEN auf PE-Schiene mit Brücke wird als Fehler gemeldet | B5 | – | Zustimmung. So wird in der Praxis aufgeteilt, die App darf das nicht als Fehler werten. Didaktisch wichtig: Genau **dieser eine** Punkt ist die erlaubte Verbindung, jede weitere ist verboten. |
| § 13 NAV im Text zu Zählerschrank und Sammelschienen | B6 | – | Deckt sich mit meinem A6. |
| Stall: Schutzpotentialausgleich, TN-S, FI-Werte, IP54 bei Heu/Stroh, Geräte außer Reichweite | B7 | A3–A5 | Zustimmung. Für den Pferdestall ist das der wichtigste Praxisblock. |
| Spannungsfall und Abschaltbedingung bei langen Leitungen | C2 | A6 | Zustimmung, mit Einschränkung zur Einstufung (siehe 2.3). |
| Selektivität SLS–LS nur laut Herstellertabelle | C6 | – | Zustimmung. Dann wird auch der Simulationstext „Der Hauptschalter bleibt drin“ ehrlicher. |
| Erfundener Wert „NH 63 A“ in der Kundendoku | C3 | – | Zustimmung (Projektregel „keine erfundenen Werte“). |
| Prüfprotokoll nach DIN VDE 0100-600 in der Doku | D1 | – | Zustimmung. |
| Blau als Schaltdraht ist erlaubt, der Schaltplan widerspricht | B10 | D3 | Zustimmung, Vorschlag zur einheitlichen Lehre in 3.2. |
| Zwei Stromkreise in einer Dose bleiben für Laien ein Fehler | – | C7 | Zustimmung zum Beibehalten, mit dem ehrlicheren Text des Elektrikers. |
| Bad-Regeln, Herd ohne N, FI-Typ, PV/Notstrom bei Regel 4 | C5, B9 | B7, D5, D15 | Zustimmung, deckt sich mit Runde 1. |

---

## 2. Wo ich widerspreche oder einschränke

### 2.1 Zu viele Fachdetails für das Lernpublikum (Meister A1, A5, C2, D1)
Fachlich stimmt alles. Didaktisch würde ich es nicht übernehmen, wenn dafür jeder Stromkreis eine Verlegeart A1, B1, B2 oder C bekommt und die App Iz-Tabellen anzeigt. Für Patrick heißt das sonst: viele Fehlermeldungen, die er nicht einordnen kann.
**Vorschlag:**
- Eine **konservative Grundregel** ohne Auswahl (Tabelle des Elektrikers: 1,5 mm² → höchstens 16 A, 2,5 → 20, 4 → 25, 6 → 32, 10 → 40, 16 → 63).
- Bei Überschreitung `err` mit einem Satz Begründung.
- Wer genauer rechnen will, findet die Verlegeart unter „Technisch“. Die Feinauslegung bleibt bei der Elektrofachkraft.
- Ein Widerspruch im Meistervorschlag selbst: „rating > Iz bei Verlegeart C → `warn`, also immer unzulässig“. Was immer unzulässig ist, muss `err` sein.

### 2.2 Hauptleitung „mindestens 63 A“ (Meister A5)
Das hängt vom Netzbetreiber bzw. der TAB ab. Der Meister kennzeichnet es selbst als unsicher. Deshalb `warn` mit „mit Netzbetreiber/TAB klären“, kein `err`. Diese Leitung verlegt ohnehin der eingetragene Betrieb, nicht Patrick.

### 2.3 Spannungsfall 3 % (Meister C2, Elektriker A6)
Die 3 % aus DIN 18015-1 sind ein Richtwert für **Wohngebäude**. Abgesehen von Sonderfällen ist das keine Sicherheitsgrenze. Deshalb nur `hint` oder `warn`, **nie** `err`. Die **Abschaltbedingung** ist dagegen sicherheitsrelevant. Weil die Netzimpedanz unbekannt ist, kann die App sie aber nicht berechnen. Bei langen Leitungen (z. B. Stall 40 m und mehr) passt dieser Text: „Ob der LS im Fehlerfall schnell genug abschaltet, muss gemessen werden (Schleifenimpedanz).“ Die Ein-Zahl-Grenze des Elektrikers (60 m bei 1,5 mm² → `err`) ist zu genau für eine unbekannte Größe. Ich würde sie als `warn` formulieren. [N]

### 2.4 Stall: Höhe der Geräte (Elektriker A4)
Inhaltlich richtig. Die 1,5 m sind aber Praxis, keine Norm (der Elektriker sagt das selbst). Deshalb `warn`, nicht GEFÄHRLICH, und der Text soll „Praxisempfehlung“ sagen. Gefährlich ist im Stall vor allem der fehlende Schutzpotentialausgleich und die Brandlast, nicht die Höhe von 30 cm an sich.

### 2.5 30-mA-Meldung für Leuchten im Stall (Meister C4)
Ich stimme zu, dass die Begründung „in Wohnungen“ dort nicht passt. Den Ratschlag würde ich aber **nicht** abschwächen. Für einen privaten Pferdestall, in dem Laien arbeiten und Wasser im Spiel ist, empfehle ich didaktisch weiter **30 mA für alle Endstromkreise**, und 300 mA nur als zusätzlichen Brandschutz. Formulierung: „Pflicht nach 705: ≤ 300 mA. Empfehlung: 30 mA auch fürs Licht.“

### 2.6 SPD im netzseitigen Anschlussraum „nur Funkenstrecke“ (Meister B3)
Das deckt sich mit meinem Kenntnisstand: Vor dem Zähler darf kein Ableitstrom fließen. Den Wortlaut der VDE-AR-N 4100 habe ich aber nicht gesehen [U]. Übernehmen, Quelle DEHN nennen.

### 2.7 Brandschutzschalter (Meister B8)
Fachlich richtig. Für den Baukasten ist das ein `hint` mit niedriger Priorität und gehört **nicht** in die erste Umsetzungsrunde.

---

## 3. Didaktische Sicht auf die Streitpunkte

### 3.1 SPD 3+0 L–PEN auf der Sammelschiene, PEN-Aufteilung auf derselben Schiene
**Stand der Fachrunde:** Alle drei Prüfer halten 3+0 für vertretbar, **wenn** der Ableiter unmittelbar an der PEN-Aufteilung sitzt. Keiner konnte die genaue Grenze im Normtext belegen. Die 0,5 m in DEHN DS273 betreffen die Länge der Anschlussleitungen, nicht den Abstand zur Aufteilung. Ich bewerte das als mittlere Evidenz für „zulässig in diesem Aufbau“ und als unsicher für die genaue Abstandsgrenze.

**Wie die App das lehren soll:**
1. **Modell:** Den Aufbau so lassen, er entspricht dem Foto (SPD auf den Schienen, Aufteilung auf demselben System).
2. **Beschriftung** ändern von „3+0 · L–PEN“ zu **„3+0 · L1, L2, L3 gegen PE(N)“**. Sie greift an der PE-Schiene ab, die direkt am Aufteilungspunkt mit dem PEN verbunden ist.
3. **Regel in `check()`:** `spdss` ist nur in Ordnung, wenn die PEN-Aufteilung im **selben** Zählerschrank auf **derselben** Schiene sitzt. Sonst `warn`: „Hier ist N schon getrennt (TN-S). Dann ist ein Ableiter auch zwischen N und PE nötig: 4+0 oder 3+1 (TNS-Ausführung).“
4. **Erklärtext** einheitlich an allen drei Stellen (`spdss`, `spd`, `hlak`), siehe Soll-Text 7. Damit verschwindet der Widerspruch, den der Elektriker in B6 beschreibt.
5. **Ehrlichkeit:** Unter „Technisch“ ein Satz: „Wo genau die Grenze liegt, regeln DIN VDE 0100-534 und der Hersteller. Im Zweifel die TNS-Variante nehmen.“

### 3.2 Blaue Ader als Schaltdraht: Fehler oder erlaubt?
**Fachlich** (DIN VDE 0100-510, 514.3.1): Die blaue Ader darf als Außenleiter benutzt werden, wenn **in dieser Leitung** kein Neutralleiter gebraucht wird. Grün-gelb darf nie etwas anderes sein. Kennzeichnen ist Praxis und empfohlen. [N]

**Eine einheitliche Lehre in drei Stufen, überall gleich formuliert:**

| Situation | Stufe | Text (identisch in Schaltplan und Dose) |
|---|---|---|
| Grün-gelb an L oder N | **Fehler** | „Grün-gelb ist nur für den Schutzleiter. Nie anders verwenden.“ |
| Blau an L, nicht gekennzeichnet | **Warnung** | „Blau ist für N vorgesehen. Als Schaltdraht nur erlaubt, wenn die Leitung keinen N braucht, und dann an beiden Enden schwarz oder braun kennzeichnen.“ |
| Blau an L, gekennzeichnet („blau, schwarz markiert“) | **Hinweis** | „Erlaubt (DIN VDE 0100-510). Einfacher und eindeutiger: eine Leitung ohne blaue Ader, z. B. NYM-J 5×1,5.“ |
| Blau an L, und an derselben Stelle wird ein N gebraucht | **Fehler** | „Hier braucht es einen N. Blau muss N bleiben.“ |

**Umsetzung im Schaltplan:** In der Farbwahl der Leitung eine Farbe „blau, schwarz markiert“ anbieten. Im Schaltplan gibt es keine Mehraderleitungen, deshalb lässt sich die Bedingung „Leitung ohne N“ nicht prüfen. Dort reicht als Standard die Warnung. Die Dose kennt die Leitungen und kann genauer unterscheiden.

**Didaktischer Grundsatz:** „Erlaubt, aber nur mit Kennzeichnung, besser vermeiden.“ So prägt man sich nichts Falsches ein: Blau bleibt N, Ausnahmen werden sichtbar gemacht.

### 3.3 Geführter Aufbau: unter Spannung arbeiten, 5 Sicherheitsregeln, Erstprüfung – ohne zu überfrachten
**Problem heute:** Alles ist von Anfang an eingeschaltet, man klemmt an Teilen unter Spannung an, und ohne Prüfung folgt „Die Lampe leuchtet“. Gelernt wird: anschließen, einschalten, fertig.

**Ziel:** Sicherheit als **Rahmen** der Übung, nicht als Textwand. Drei kurze Stellen genügen.

1. **Start (Schritt 0, eine Karte, ein Knopf):**
   Die Simulation beginnt **freigeschaltet**: HAK-Sicherungen gezogen (Hinweis „Netzbetreiber“), SLS, FI und LS aus. Auf der Karte stehen die 5 Regeln als kurze nummerierte Liste mit je ca. 5 Wörtern. Knopf „Verstanden – Anlage ist freigeschaltet“. Nichts leuchtet gelb. Damit sieht man: Gearbeitet wird spannungsfrei.
2. **Während des Verdrahtens (kein Zusatzschritt):**
   - Ein schmales Band oben im Anleitungskasten: **„Übung · Anlage freigeschaltet“**.
   - Bei den Schritten am HAK und im netzseitigen Anschlussraum (heute 1–4) ein kleines Etikett: **„Macht der eingetragene Elektrobetrieb – hier nur zum Verstehen.“**
   - Die Abzweigklemme oben (`hlak`) als Zwischenstation einbauen (siehe Foto).
   - PE an der Lampe vor N und L verbinden.
3. **Vor dem Einschalten (ein Prüfschritt, dann das Einschalten in Reihenfolge):**
   - **Schritt „Prüfen“:** eine Karte mit drei Zeilen (Sichtprüfung · Schutzleiter durchgängig · Isolation gemessen). Knopf „Prüfung erledigt (Elektrofachkraft)“. Erst danach geht es weiter.
   - **Schritt „Einschalten“:** HAK-Sicherungen (Netzbetreiber), dann SLS, FI, LS, Lichtschalter. Die Simulation zeigt jetzt den Strom.
   - **Schritt „Nach dem Einschalten“:** Prüftaste am FI drücken. Hinweis: „Die Fachkraft misst noch Schleifenimpedanz und FI-Auslösezeit und schreibt das Prüfprotokoll.“
   - Abschluss „Geschafft“: „Die Lampe leuchtet, und die Anlage ist geprüft.“

Ergebnis: 14 → etwa 18 Schritte, aber nur **zwei** davon sind Lesekarten (Start und Prüfen). Der Rest wird geklickt. Die ausführlichen Texte stehen hinter „Mehr dazu“. Die 5 Regeln werden nicht bei jedem Schritt wiederholt. Dafür ist das Band „freigeschaltet“ dauerhaft zu sehen.

**Warum so:** Sicherheitsregeln merkt man sich, wenn man sie **tut** (Freischalten, Prüfen, Einschalten in Reihenfolge), nicht, wenn man sie nur liest. Die Simulation kann außerdem zeigen, dass erst nach dem Einschalten Spannung da ist. Das ist die wichtigste Erfahrung.

---

## 4. Soll-Texte für die 10 wichtigsten Erklärtexte

Jeweils fertig zum Einsetzen, in einfachem Deutsch.

**1. Hauptschalter SLS (`INFO.sls.e`, Kurztext `sls.sub`)**
> Der Hauptschalter schaltet alles **hinter** ihm ab: Zähler, Unterverteilung und alle Stromkreise. Davor bleibt alles unter Spannung: Hausanschluss, Sammelschienen und Überspannungsschutz. Gibt es Notstrom, PV oder einen Speicher, kann auch von dort Spannung kommen.

Kurztext: „Schaltet alles hinter dem Zähler ab“

**2. PEN-Aufteilung (`INFO.pen.e`)**
> Aus dem Netz kommt ein gemeinsamer Leiter für Neutral- und Schutzleiter, der PEN. Hier wird er einmal aufgeteilt: zuerst auf die Schutzleiterschiene (PE), von dort weiter zum Neutralleiter (N), und an diesen Punkt kommt auch die Haupterdungsschiene. Danach dürfen N und PE nie wieder verbunden werden, und der PEN darf nie unterbrochen werden.

**3. FI-Schalter (`INFO.fi4.e` / `fi2.e`)**
> Der FI vergleicht den Strom, der hinfließt, mit dem, der zurückkommt. Fehlt etwas, weil Strom über PE oder über einen Menschen abfließt, schaltet er ab. Der 30-mA-FI ist ein **zusätzlicher** Schutz für Menschen, der 300-mA-FI nur Brandschutz. Berührst du L und N gleichzeitig, merkt der FI nichts.

**4. Leitungsschutzschalter (`INFO.ls1.e`)**
> Der LS schaltet ab, wenn zu viel Strom fließt (Überlast) oder bei einem Kurzschluss, damit die Leitung nicht heiß wird. Zusammen mit dem Schutzleiter schützt er auch dich: Bekommt ein Metallgehäuse Spannung, fließt über PE ein sehr hoher Strom und der LS schaltet sofort ab. Seine Größe richtet sich nach der Leitung, bei 1,5 mm² höchstens 16 A.

**5. Neutralleiter / N-Schiene (`INFO.nk.e`)**
> Über den blauen Neutralleiter fließt der Strom zum Netz zurück. Er hat normalerweise fast keine Spannung gegen Erde, ist aber **nicht ungefährlich**: Ist er unterbrochen, liegen am N des Geräts 230 V an. Deshalb vor dem Anfassen jeden Leiter messen, auch den blauen.

**6. Schutzleiter / PE-Schiene (`INFO.pek.e`)**
> Der grün-gelbe Schutzleiter verbindet alle Metallgehäuse mit der Erde. Bekommt ein Gehäuse Spannung, fließt der Fehlerstrom über PE ab, und LS oder FI schalten ab, bevor du es anfasst. PE wird nie geschaltet, nie unterbrochen und nie durch einen FI geführt.

**7. Überspannungsschutz auf den Sammelschienen (`INFO.spdss.e`, einheitlich auch für `spd` und `hlak`)**
> Der Überspannungsschutz leitet kurze Spannungsspitzen, etwa von einem Blitz in der Nähe, zur Erde ab. Dieser hier sitzt vor dem Zähler direkt an der PEN-Aufteilung und schützt L1, L2 und L3 gegen PE (3+0). Sitzt ein Ableiter weiter hinten, wo N und PE schon getrennt sind, braucht er auch einen Pfad zwischen N und PE (4+0 oder 3+1).

**8. Gehäuse berühren (`touch()`, zwei Fälle)**
> **30-mA-FI:** Der FI hat nach etwa 30 ms abgeschaltet. Du hast trotzdem einen Schlag bekommen, nur kurz. Der FI ersetzt keinen Schutzleiter: Ein Gehäuse ohne PE ist immer ein Fehler.
>
> **300-mA-FI oder kein FI:** Lebensgefahr! Durch deinen Körper fließen etwa 100–250 mA. Das ist zu wenig, um einen 300-mA-FI sicher auszulösen, aber genug für Herzkammerflimmern. Ein 300-mA-FI ist Brandschutz, kein Personenschutz.

**9. Messgerät (`volt()`): N gegen PE und offener Leiter**
> **N–PE:** etwa 0 V. N und PE sind nur an **einer** Stelle verbunden, an der PEN-Aufteilung im Zählerschrank. Mehr als ein paar Volt bedeuten einen Fehler.
>
> **Offener Leiter:** Ist dieser Leiter über ein eingeschaltetes Gerät mit L verbunden, zeigt der Prüfer 230 V, auch wenn er blau ist. Spannungsfrei ist eine Stelle erst, wenn du **alle** Leiter gegeneinander und gegen PE gemessen hast (L–N, L–PE, N–PE).

**10. Geführter Aufbau: Start- und Prüfkarte (`GSTEPS`, neu)**
> **Start:** In echt arbeitet man nie unter Spannung. Die 5 Regeln: 1. Freischalten. 2. Gegen Wiedereinschalten sichern. 3. Spannungsfreiheit zweipolig messen. 4. Erden und kurzschließen, wenn nötig. 5. Teile unter Spannung abdecken. Die Übungsanlage ist jetzt freigeschaltet.
>
> **Prüfen:** Vor dem ersten Einschalten prüft die Elektrofachkraft die Anlage (DIN VDE 0100-600): Sichtprüfung, Schutzleiter durchgängig, Isolation gemessen. Erst danach wird eingeschaltet, zuerst der Hauptschalter, dann FI, dann LS.

---

## 5. Priorität für die Umsetzung (gemeinsame Sicht)

1. **Sofort, Lebens- oder Brandgefahr:**
   - Querschnitt gegen LS (Meister A1, Elektriker A1)
   - `touch()` 30/300 mA
   - Offener N im Messgerät
   - Geführter Aufbau freigeschaltet, mit Prüfkarte
   - Texte zu SLS, N und PE (Soll-Texte 1, 5, 6, 8, 9, 10)
2. **Danach:**
   - Haupterdungsschiene mit Schutzpotentialausgleich
   - Stall-Regeln nach 705 (Pferdestall)
   - SPD in der Doku und SPD-Texte einheitlich
   - LS-, PE- und FI-Rollen (Soll-Texte 3, 4)
   - § 13 NAV
   - Blau-Regel vereinheitlichen
3. **Dann:**
   - Hauptleitung 4×16
   - Spannungsfall als `hint`
   - Prüfprotokoll in der Doku
   - Überlast-Zeittext
   - Herd ohne N
   - Bad
   - `hlak` im geführten Aufbau
4. **Später:** Brandschutzschalter (AFDD), FI-Typ, Selektivitätstabelle, Kleinkram.
