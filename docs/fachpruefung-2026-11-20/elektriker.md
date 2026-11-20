# Review „Unterverteilung – Baukasten“ aus Sicht Elektriker/Monteur

Geprüft: `/home/claude/unterverteilung/index.html` (Stand Commit `efe3eb9`), nicht verändert.
Vorgehen: Code gelesen (DOSE-Modul, TYPES/INFO, check(), roomRules(), RTYPES, ZDEF/zonesFor, MAT/materials()), App mit Playwright/Chrome gestartet, `DOSE.boxModel()` für Aus-, Serien-, Wechselschaltung, 3 Lampen, volle Dose, Zuleitung 5×1,5 (Hebel- und Lüsterklemme) durchgerechnet, `exampleHouse()` mit `check()` und `materials()` ausgewertet, Pferdestall-Raum mit Steckdose/Schalter/Lampe angelegt, Dosen-Editor und Wandansicht als Screenshot angesehen.

Evidenzangaben: **stark** = Norm-/Herstellertext geprüft oder eindeutig in der Praxis; **mittel** = Normwissen aus Erfahrung, Wortlaut hier nicht nachgelesen; **schwach** = Praxisregel/Einschätzung, ehrlich unsicher.

Gesamteindruck vorweg: Die Abzweigdose ist fachlich überraschend sauber. Aderfarben, Klemmenzuordnung, Wechsel-/Serienschaltung, 5 Sicherheitsregeln, Zugentlastung, Reserveadern und Prüfung nach VDE 0100-600 sind so, wie ich es einem Lehrling beibringen würde. Die großen Lücken liegen woanders: **Sicherung und Leitungsquerschnitt werden nicht gegeneinander geprüft**, und **der Raumtyp Stall deckt die wichtigsten Punkte der VDE 0100-705 nicht ab**. Beides ist für das Pferdestall-Projekt relevant.

---

## A. GEFÄHRLICH falsch (Brand oder Stromschlag möglich)

### A1. Keine Prüfung „Sicherung passt zur Leitung“ (Überlastschutz)
- **Fundstelle:** `check()` Zeile ~676; `TYPES.ls1.ratings [6,10,13,16,20]`, `ls3.ratings [16,20,25,32]`, `rcbo.ratings` bis 20, `sls.ratings [35,50,63]`, jeweils mit eigenem Feld `cable`.
- **Ist:** Getestet im Beispielhaus: „Licht“ auf **B20 mit NYM-J 3×1,5** → **keine Meldung**. „Steckdose“ B20 mit 3×1,5 → nur `warn` mit der Begründung „Bei 1,5 mm² und Schuko-Steckdose ist B16 die Obergrenze“. Herd `ls3` lässt sich auf **3×B32 mit NYM-J 5×2,5** stellen, ohne Meldung. SLS E63 mit NYM-J 5×10 ebenfalls ohne Meldung.
- **Soll:** Die Bedingung I_B ≤ I_n ≤ I_z aus DIN VDE 0100-430 muss für jeden Stromkreis gelten. Belastbarkeit nach DIN VDE 0298-4, grob für Kupfer, PVC, 30 °C:
  - 1,5 mm²: höchstens B16 (Verlegeart B2/C). Bei Verlegung in wärmegedämmter Wand (A1) oder bei Häufung höchstens B13 bzw. B10.
  - 2,5 mm²: B16 bis B20 (Verlegeart C bis 25 A).
  - 5×2,5 als Drehstrom: höchstens B16 bis B20.
  - 10 mm² (Hauptleitung): je nach Verlegeart etwa 35 bis 50 A, also nicht E63.
- **Begründung:** Eine Leitung, die stärker abgesichert ist, als sie tragen kann, wird bei Dauerlast warm, ohne dass der LS auslöst. Das ist die klassische Brandursache. Evidenz: **stark** (Grundregel VDE 0100-430; Tabellenwerte VDE 0298-4 **mittel**, hier aus Erfahrung, nicht nachgeschlagen).
- **Korrektur:** In `check()` eine Tabelle `IZ={ '1,5':16, '2,5':20, '4':25, '6':32, '10':40, '16':63 }` (bewusst konservativ, Verlegeart B2, 2 bzw. 3 belastete Adern) einbauen, den Querschnitt aus `d.cable` lesen und bei `rating > IZ` den Fehler **`err`** ausgeben: „Sicherung B20 ist zu groß für 1,5 mm². Die Leitung kann überhitzen, bevor die Sicherung auslöst. Höchstens B16, oder 2,5 mm² verlegen.“ Bei der Steckdose die Begründung trennen: Schuko ist 16 A, deshalb höchstens B16, *und zusätzlich* die Querschnittsregel. Aus `warn` ein `err` machen.

### A2. Dose wählt automatisch 1,5 mm² für Lampe und Schalter, auch wenn die Zuleitung 2,5 mm² hat
- **Fundstelle:** `DOSE.expand()` → `const qs=(b.art==='steckdose'||b.art==='weiter'||b.art==='zuleitung')?fq:1.5;`
- **Ist:** Getestet mit der Konfiguration „voll“: Zuleitung NYM-J 3×2,5, Lampen und Schalter werden *automatisch* mit NYM-J 3×1,5 geplant. Es kommt nur ein `achtung` „Querschnitt gemischt“.
- **Soll:** Wer eine 2,5er-Zuleitung legt, hat meist B20 oder eine lange Strecke. Dann sind 1,5-mm²-Abgänge nicht mehr geschützt (siehe A1). Die Automatik darf keinen Zustand erzeugen, den die eigene Prüfung bemängelt.
- **Begründung:** Für einen Laien gilt die Voreinstellung als „richtig“. Er liest die Warnung, versteht aber nicht, dass er die Sicherung kennen muss. Evidenz: **stark** (Logik von A1).
- **Korrektur:** Auto-Querschnitt = Querschnitt der Zuleitung für *alle* Abgänge. Alternativ das Feld „Sicherung im Verteiler: B10/B13/B16/B20“ in den Dosen-Editor aufnehmen und daraus den Mindestquerschnitt ableiten. Die Warnung „Querschnitt gemischt“ auf `fehler` hochstufen, sobald die Sicherung > B16 ist oder unbekannt bleibt.

### A3. Stall (VDE 0100-705): zusätzlicher Schutzpotentialausgleich fehlt völlig
- **Fundstelle:** `RTYPES['Stall / Landwirtschaft']` (Text, `short`, `hint`), `roomRules()`. In der ganzen Datei gibt es nichts zu Potentialausgleich, Erder oder Haupterdungsschiene, außer einem Nebensatz in `INFO.pek`.
- **Ist:** Die Regeln sind IP44, FI ≤ 300 mA, Steckdosen 30 mA, Leuchten mit begrenzter Wärme, „Leitungen vor Tieren geschützt verlegen“.
- **Soll:** In Räumen für Nutztiere ist ein **zusätzlicher Schutzpotentialausgleich** Pflicht. Er verbindet alle fremden leitfähigen Teile (Tränken, Fressgitter, Boxenstangen, Metallrohre) und die Bewehrung im Boden. Dazu kommt ein Metallgitter im Boden.
- **Begründung:** Großtiere reagieren schon auf kleine Berührungsspannungen, weil sie über vier Hufe einen großen Schrittspannungsweg haben. Pferde verweigern dann die Tränke oder verletzen sich. Sekundärquelle WEKA zu VDE 0100-705: „In Räumen zur Nutztierhaltung ist ein zusätzlicher Schutzpotenzialausgleich zu erstellen … Bewehrungen von Böden, Wänden und Güllekellern sind einzubeziehen“ ([WEKA](https://www.weka.de/elektrosicherheit/vde-0100-705-landwirtschaftliche-und-gartenbauliche-betriebsstaetten/)). Evidenz: **stark**.
- **Korrektur:** Im Stall-Text und als `hint` ergänzen: „Zusätzlicher Schutzpotentialausgleich Pflicht: Tränken, Fressgitter, Boxenstangen, Metallrohre und Bodenbewehrung mit grün-gelbem Leiter (mind. 4 mm² Cu, geschützt verlegt) verbinden, zur PE-Schiene. Planung durch die Elektrofachkraft.“ In der Materialliste bei Stallräumen einen Posten „Potentialausgleichsschiene + PA-Leiter 4 mm² / 6 mm²“ vorschlagen.

### A4. Stall: Geräte in Reichweite der Tiere, Steckdose standardmäßig auf 30 cm
- **Fundstelle:** `ZDEF` (`steckdose:.3`, `schalter:1.05`), `zonesFor()`, `midDefault()`. Der Raumtyp beeinflusst die Höhen nicht.
- **Ist:** Getestet im Pferdestall: Steckdose landet auf 0,30 m, Schalter auf 1,05 m, ohne Hinweis. Die Wandansicht zeigt die Wohnungs-Installationszonen nach DIN 18015-3.
- **Soll:** Nach VDE 0100-705 dürfen Betriebsmittel für Nutztiere nicht erreichbar sein. Steckdosen dürfen nicht mit brennbarem Material (Stroh, Heu, Einstreu) in Berührung kommen ([WEKA](https://www.weka.de/elektrosicherheit/vde-0100-705-landwirtschaftliche-und-gartenbauliche-betriebsstaetten/), **stark**). Praxis: Schalter und Steckdosen nicht in die Box, sondern in die Stallgasse, Sattel- oder Futterkammer. Dort hoch genug, dass weder Pferdemaul noch Mistgabel, Einstreu oder der Wasserschlauch hinkommen, also eher **≥ 1,5 m**. In der Box nur Deckenleuchten mit Schutzkorb oder schlagfest. Die 1,5 m sind Praxis, keine Normzahl. Evidenz für die Zahl: **schwach**. Pferde erreichen mit Kopf und Hals deutlich über 2 m.
- **Begründung:** Pferde knabbern an allem, was sie erreichen. Eine Steckdose auf 30 cm steckt im Stall in der Einstreu, wird beim Ausmisten getroffen und beim Abspritzen nass.
- **Korrektur:** Bei den Raumarten Stall und Lager eigene Standardhöhen setzen (z. B. Steckdose/Schalter 1,5 m), einen `warn` ausgeben, wenn ein Gerät im Stall unter ~1,4 m sitzt, und die Zonen nach DIN 18015-3 in der Wandansicht für Stall/Außen ausblenden oder mit „gilt nur für Wohngebäude“ kennzeichnen. DIN 18015 gilt für Wohngebäude, nicht für Stallungen (**mittel**).

### A5. Pferdestall mit Heu oder Stroh = feuergefährdet, aber Raumtyp „Stall“ verlangt nur IP44
- **Fundstelle:** `RTYPES['Stall / Landwirtschaft'].ip='IP44'`; nur `'Lager (Heu/Stroh)'` nennt IP5X, und auch das nur als „empfohlen“.
- **Ist:** Im Stall-Raum besteht eine Leuchte mit IP44 die Prüfung.
- **Soll:** Ein Pferdestall mit Strohstreu und Heuraufe ist praktisch immer auch feuergefährdete Betriebsstätte. Dort gelten Leuchten mit begrenzter Oberflächentemperatur, **mindestens IP54** und Maßnahmen gegen Staubablagerung ([WEKA zu 705](https://www.weka.de/elektrosicherheit/vde-0100-705-landwirtschaftliche-und-gartenbauliche-betriebsstaetten/), **stark**). Hinzu kommen ein FI ≤ 300 mA für alle Kreise (in der App vorhanden) und keine Halogenstrahler.
- **Korrektur:** Für den Stall ein Kontrollkästchen „Heu/Stroh im Raum (feuergefährdet)“ anbieten oder den Stall gleich mit `ip:'IP54'` vorbelegen. Den Text im `hint` von „begrenzte Wärmeabgabe“ ändern in „Leuchten mit begrenzter Oberflächentemperatur, mind. IP54, staubdicht montiert, Abstand zu Heu/Stroh“. Zusätzlich auf Schlagfestigkeit hinweisen (IK-Wert, Schutzkorb). Für die IK-Klasse nenne ich bewusst keine feste Zahl, Evidenz **schwach**.

### A6. Keine Prüfung von Spannungsfall und Abschaltbedingung bei langen Leitungen
- **Fundstelle:** Im ganzen Code fehlt ein Spannungsfall- oder Schleifenimpedanz-Check (`grep Spannungsfall` = 0). Die Länge `d.len` wird nur für die Materialliste genutzt.
- **Ist:** Eine Stall-Steckdose mit 40 m NYM-J 3×1,5 an B16 ist für die App „korrekt“.
- **Soll:** Nach DIN 18015-1 und VDE 0100-520 gilt für Endstromkreise ab Zähler ein Richtwert von ≤ 3 % Spannungsfall. Bei 1,5 mm² und 16 A ist das nach rund **18 m** erreicht, bei 2,5 mm² nach rund 30 m (ΔU = 2·L·I/(56·A)). Bei sehr langen Leitungen löst der LS im Kurzschluss außerdem nicht mehr schnell genug aus (Abschaltbedingung VDE 0100-410). Evidenz: **mittel** (Formel stark, 3 %-Wert aus DIN 18015-1 aus Erfahrung).
- **Begründung:** Der Stall liegt oft 30 bis 80 m vom Haus. Genau dort ist das wichtig.
- **Korrektur:** Bei `d.len` > 0 den Spannungsfall rechnen. Über 3 % ein `warn` mit dem nächstgrößeren Querschnitt als Vorschlag. Ab ca. 60 m bei 1,5 mm² ein `err` „Abschaltung im Fehlerfall fraglich – Schleifenimpedanz von der Elektrofachkraft messen lassen“. Für entfernte Gebäude den Hinweis „eigene Unterverteilung im Stall, Zuleitung NYY-J 5×… im Erdreich“.

---

## B. Fachlich falsch oder unvollständig

### B1. Leitungsauswahl ohne NYY für kleine Querschnitte, kein Erdkabel-Hinweis
- **Fundstelle:** `CABLES=['NYM-J 3×1,5', …, 'NYY-J 5×10','NYY-J 5×16','H07V-K Einzelader','Andere']`; `DOSE.KABELTYPEN` nur NYM.
- **Ist:** Kein NYY-J 3×1,5 / 3×2,5 / 5×1,5 / 5×2,5 / 5×4 / 5×6, kein H07RN-F.
- **Soll:** NYM ist laut Herstellerdatenblatt für trockene, feuchte und nasse Räume und geschützt im Freien geeignet, **„jedoch nicht im Erdreich“** ([Waskönig NYM-J Datenblatt](https://www.waskoenig.de/download/0007?locale=de), **stark**). Für eine Zuleitung zum Stall im Erdreich braucht es NYY-J. Die Erdverlegung erfolgt bei Landwirtschaft mindestens 0,6 m tief mit mechanischem Schutz, auf Acker 1 m ([WEKA](https://www.weka.de/elektrosicherheit/vde-0100-705-landwirtschaftliche-und-gartenbauliche-betriebsstaetten/), **stark**).
- **Wichtig, damit niemand Falsches lernt:** NYY ist **kein Nagerschutz**. Der PVC-Mantel wird genauso angefressen wie bei NYM. Gegen Ratten und Mäuse helfen Stahlrohr, Metallkanal oder Leitungen mit ausdrücklichem Nagetierschutz. Evidenz: **mittel** (Praxis; Kabelhersteller bieten eigene „Nagerschutz“-Typen an). Die 705 verlangt ausdrücklich besondere Aufmerksamkeit für den Nagetierschutz (WEKA, **stark**).
- **Korrektur:** `CABLES` um NYY-J 3×1,5, 3×2,5, 5×1,5, 5×2,5, 5×4, 5×6 ergänzen. In `DOSE.KABELTYPEN` ebenfalls NYY-J 3×1,5/3×2,5/5×1,5/5×2,5 aufnehmen (Aderfarben identisch, Außendurchmesser etwas größer, Datenblatt prüfen). Im Stall-Text ergänzen: „Leitungen außerhalb der Reichweite der Tiere; gegen Nager in Stahl- oder starres Kunststoffrohr bzw. Metallkanal; NYY nur für Erdreich nötig, ist aber kein Nagerschutz.“

### B2. Kabelmeter in der Materialliste sind zu niedrig
- **Fundstelle:** `materials()` (nur `d.len` je LS); `routeInfo()`; Panel-Text „Wege zur Dose nach unten bitte dazurechnen“; Zeile 1239: `if(/^NYM|^Schrauben/.test(r[1]))continue;` übernimmt die Leitungen aus der Dose nicht.
- **Ist:**
  1. „Übernehmen“ setzt die Länge = Weg im Grundriss + Steigleitung. Die senkrechten Stiche (Decke → Schalter 1,05 m ≈ 1,2–1,5 m, Decke → Steckdose 0,3 m ≈ 2,0–2,2 m), die Reserve in Verteiler und Dosen und der Verschnitt fehlen.
  2. Je Stromkreis gibt es nur *einen* Leitungstyp. Die NYM-J 5×1,5 zum Wechsel- oder Serienschalter und die Schalterleitungen aus dem Dosen-Editor landen nicht in der Gesamtliste (werden per Regex verworfen).
- **Soll:** Praxisrechnung je Abgang: Waagerechte im Grundriss + (Raumhöhe − Montagehöhe − 0,3 m Zone) je Gerät + 0,5–1 m im Verteiler + 0,15–0,2 m je Dosenende, dann +10 % Verschnitt. Gekauft wird in 50-m- oder 100-m-Ringen. Evidenz: **schwach**, Praxiswerte.
- **Korrektur:** In `routeInfo()` den Stich zu jedem Gerät aus `zOf()` und Raumhöhe automatisch addieren, je Abgang Reservezuschläge rechnen und einen Verschnittfaktor als Option anbieten. Die Leitungen aus `doseModel().material` als eigene Positionen (Typ + Stückzahl, Länge „vor Ort“) in die Gesamtliste übernehmen statt sie zu verwerfen. Zusätzlich eine Spalte „Ringgröße“ (50/100 m).

### B3. Materialliste ignoriert Raumtyp und Montageart
- **Fundstelle:** `MAT.schalter` „Ausschalter mit Wippe und Rahmen“, `MAT.steckdose` „Schutzkontaktsteckdose mit Rahmen“, `MAT.lampe` „Leuchte (Schutzklasse I)“; `materials()`: `Geräte-/Schalterdose Unterputz` = Anzahl schalter+steckdose.
- **Ist:** Getestet im Pferdestall. Die Liste nennt „Ausschalter mit Wippe und Rahmen“, „Geräte-/Schalterdose Unterputz ×2“ und keine Schutzart.
- **Soll:** Im Stall, im feuchten Keller und außen verwendet man **Aufputz-Feuchtraumgeräte IP44/IP54/IP55** ohne UP-Dose. Doppelsteckdose und Kombination brauchen im UP-Fall eine Doppel-/Zweifachdose bzw. zwei Dosen. Für die Herdanschlussdose braucht es eine UP-Geräte- bzw. Herdanschlussdose. Diese werden nicht gezählt (`dsteck`, `kombi`, `herd` fehlen in der Zählung). Evidenz: **stark** (Praxis).
- **Korrektur:** In `MAT` die IP des Geräts und „Aufputz“/„Unterputz“ je nach Raumtyp in den Namen schreiben: „Feuchtraum-Aufputz-Steckdose IP55“. UP-Dosen nur in Raumtypen ohne `ip`-Regel zählen; `dsteck`/`kombi` als 2 Dosen bzw. Doppeldose zählen, `herd` als Herdanschlussdose ohne extra Gerätedose.

### B4. Kleinmaterial wird nicht automatisch vorgeschlagen
- **Fundstelle:** `renderDocEdit()` Zeile 1271 (Schnellknöpfe „Lüsterklemmen, Verbindungsklemmen 3-fach, Aderendhülsen 1,5/2,5, Kabelbinder, Kabelschellen, Leerrohr M20, Abdeckstreifen, Beschriftungsstreifen“), `materials()`.
- **Ist:** Kleinmaterial gibt es nur von Hand per Knopf, ohne Menge. Nur der Dosen-Editor nennt Schellen (1 je Leitung) und Dübel.
- **Soll (übliche Kleinteileliste einer UV plus Installation):**
  - Leitungsschellen/Nagelschellen: bei Aufputz-NYM etwa alle 25–30 cm plus je eine kurz vor jeder Dose. Menge ≈ Kabelmeter / 0,3 (**schwach**, Praxis und Herstellerangaben schwanken).
  - Dübel + Schrauben für Schellen, Dosen und Verteiler.
  - Installationsrohr bzw. Kabelkanal im Stall (Nager, Tiere).
  - Aderendhülsen für H07V-K in der Verteilung (feindrähtig in Schraubklemmen, **stark**) bzw. für feindrähtige Geräteleitungen.
  - Kabeleinführungen/Kabelverschraubungen M20/M25 für Aufputz-Verteiler und -Geräte.
  - Berührungsschutz für freie Phasenschienen-Pins, Blindabdeckstreifen für freie TE (die Texte nennen das, die Liste nicht).
  - Beschriftung: Stromkreisverzeichnis, Etiketten für LS und Dosen.
  - Potentialausgleichsschiene + PA-Leiter (siehe A3).
  - Bei UP: Gips/Schnellzement, Schlitzfräsen, Hohlwanddosen.
- **Korrektur:** Aus Kabelmetern, Dosenzahl und Raumtyp Mengen ableiten (Schellen = m/0,3; Dübel = Schellen + 2 je Dose/Gerät; Aderendhülsen = Anzahl Verdrahtungen × 2 usw.) und mit „Schätzung“ kennzeichnen.

### B5. „Verdrahtung in der Verteilung“ ohne Querschnitt und Länge
- **Fundstelle:** `materials()`: `add('Verdrahtung in der Verteilung', 'Verdrahtung '+Farbe, n, 'Verb.')`.
- **Ist:** Beispielhaus: „Verdrahtung Braun 5 Verb.“, „Schwarz 5 Verb.“ usw.
- **Soll:** Einkaufen kann man nur Meter H07V-K (bzw. H07V-U/-R) je Querschnitt und Farbe: Einspeisung FI 10 mm², Abgänge 1,5/2,5 mm². Dazu die passenden Aderendhülsen. Evidenz: **stark** (Praxis).
- **Korrektur:** Je Verbindung Querschnitt aus dem Gerät ableiten (FI-Einspeisung 10 mm², LS-Ausgang = Leitungsquerschnitt) und mit z. B. 0,4 m je Verbindung als Meterposition „H07V-K 10 mm² braun – x m“ ausgeben.

### B6. Überspannungsschutz: Widersprüchliche Angaben zu Ort und Schaltung
- **Fundstelle:** `INFO.spdss` („Im TN-C-Teil vor der PEN-Aufteilung: 3+0 … gegen PEN“), `TYPES.spdss` (busTap L1, L2, L3, **PE** auf der 5-poligen Sammelschiene, also nach der Aufteilung), `INFO.spd` („laut VDE-AR-N 4100 im gezählten Bereich“), `INFO.hlak` („Hier wird auch der Überspannungsschutz angeschlossen“), `MAT.spdss` „TN-C 3+0“.
- **Ist:** Die App sagt an drei Stellen drei Verschiedenes: vor dem Zähler auf der Sammelschiene, nach dem Zähler im anlagenseitigen Anschlussraum, an der Abzweigklemme. Die Schaltung heißt „3+0 gegen PEN“, angeschlossen wird aber an PE.
- **Soll:** Das ist nicht mein Kerngebiet, deshalb nur als Hinweis. Ich bin hier **unsicher (schwach)**, weil die Details beim Netzbetreiber (TAB) und in VDE-AR-N 4100/VDE 0100-534 liegen. Nach meinem Kenntnisstand ist 3+0 an PE vertretbar, wenn der Ableiter unmittelbar an der PEN-Aufteilung sitzt. Nach der Aufteilung (TN-S) ist sonst 4+0 bzw. 3+1 üblich.
- **Korrektur:** Einen Ort festlegen und alle Texte daran angleichen. Die Schaltung so beschreiben, wie sie gezeichnet ist („L1–L3 gegen PE, direkt an der PEN-Aufteilung“). Von Vater/Elektrofachkraft gegen die TAB des Netzbetreibers prüfen lassen.

### B7. Bad-Regeln ungenau
- **Fundstelle:** `RTYPES['Bad / Dusche'].hint/text`.
- **Ist:** „In den Bereichen 0–2 (bis 60 cm um Wanne/Dusche) keine Steckdosen und Schalter; Leuchten dort mindestens IPX4.“
- **Soll:** Bereich 0 verlangt mindestens IPX7 und nur Kleinspannung, nicht IPX4. Bei bodengleicher Dusche reicht Bereich 1 bis 1,2 m um den Wasserauslass. 230-V-Steckdosen sind in Bereich 0–2 unzulässig, Rasiersteckdosen und SELV/PELV sind in Bereich 2 erlaubt ([Baunetzwissen](https://www.baunetzwissen.de/bad-und-sanitaer/fachwissen/elektroinstallationen/elektrische-schutzbereiche-im-bad-172830)). Evidenz: **mittel**. Die Sekundärquelle nennt für Bereich 1 IPX5. Nach meiner Kenntnis verlangt VDE 0100-701:2008 IPX4 und IPX5 nur bei Strahlwasser. Ob Schalter in Bereich 2 zulässig sind, kann ich nicht sicher sagen. Die strengere App-Regel ist sicherheitlich unkritisch.
- **Korrektur:** Den Text ändern in „Bereich 0: nur SELV ≤ 12 V, IPX7. Bereich 1 und 2: Leuchten mind. IPX4, keine 230-V-Steckdosen (Rasiersteckdose erlaubt). Bodengleiche Dusche: Bereich 1 = 1,2 m um den Brausekopf.“

### B8. Herd: sinnvoller Fehlerhinweis, aber Querschnitt nicht an die Absicherung gekoppelt
- **Fundstelle:** `TYPES.ls3.def {rating:16, cable:'NYM-J 5×2,5'}`, `INFO.ls3`, `check()`.
- **Ist:** Die Verdrahtung im Beispiel ist korrekt: L1/L2/L3 braun/schwarz/grau, N blau an der N-Schiene desselben FI, PE grün-gelb an der PE-Schiene, 3-poliger LS. Das passt. Bei 3×B20/B25/B32 gibt es aber keine Meldung, siehe A1.
- **Korrektur:** Wird mit A1 erledigt. Optional der Hinweis „Viele Herde laufen nur auf 2 Phasen – Typenschild/Anschlussplan des Herds beachten (Brücken in der Herdklemme)“ (**mittel**, Praxis).

### B9. FI-Bemessungsstrom gegen Vorsicherung und Belastung nicht geprüft
- **Fundstelle:** `MAT.fi4` „40 A“ fest, `sls.ratings [35,50,63]`.
- **Ist:** Bei SLS E50/E63 vor einem 40-A-FI gibt es keine Meldung.
- **Soll:** Ein FI hat keinen eigenen Überlastschutz. Seine Belastung muss durch eine Vorsicherung ≤ I_n(FI) oder durch die Summe der nachgeschalteten LS mit Gleichzeitigkeit begrenzt sein (Herstellerangabe). Evidenz: **mittel**.
- **Korrektur:** Einen `hint` ausgeben, wenn die SLS-Nennstromstärke größer ist als die des FI und die Summe der LS am FI größer ist als die FI-Nennstromstärke: „FI-Belastung prüfen (63-A-FI wählen oder Gleichzeitigkeit begründen)“.

---

## C. Unpraktisch (Baustellen-Sicht, Dosen-Anleitung)

### C1. In der Anleitung fehlt „Leitungen kennzeichnen, bevor sie in die Dose gehen“
- **Fundstelle:** `buildSteps()` Schritte 2–3.
- **Ist:** Die Anleitung geht davon aus, dass man weiß, welche Leitung die Zuleitung, welche die Schalterleitung und welche zu Lampe 1 führt. Bei „Aus + 3 Lampen“ kommen aber **fünf identische NYM-J 3×1,5** an.
- **Soll:** Jede Leitung beim Verlegen am Ende beschriften („Zul. F3“, „Sch.“, „L1“, „L2“ …) mit Edding auf dem Mantel oder einem Klebeetikett. Wenn das versäumt wurde: bei ausgeschalteter Sicherung mit Durchgangsprüfer ausklingeln. Werden Zuleitung und Schalterleitung vertauscht, liegt die Dauerphase auf der markierten blauen Ader oder an der Lampe. Evidenz: **stark** (Praxis; das ist der häufigste Fehler beim Laien).
- **Korrektur:** Einen neuen Schritt 2 „Leitungen zuordnen und beschriften“ einfügen. Der Text kann aus `c.kurz` erzeugt werden, dazu der Tipp: „Nicht unter Spannung suchen – ausklingeln mit Durchgangsprüfer“.

### C2. Abschluss fehlt: Deckel verschrauben, Dose beschriften
- **Fundstelle:** `buildSteps()` Schritt „Prüfen“: „Deckel aufsetzen – nichts darf eingeklemmt werden“.
- **Soll:** Den Deckel mit allen Schrauben fest verschrauben, sonst gibt es keine IP-Schutzart (die Abox ist laut Händler „geschraubt“ und plombierbar, [hardy-schmitz](https://hardy-schmitz.de/Spelsberg%20Abox%20040-L%20Abzweigkasten-2401533.html)). Auf den Deckel Stromkreis-Nr. und Funktion schreiben („F3 Licht Stall“), damit der Nächste weiß, welchen LS er ausschalten muss. Evidenz: **stark**.
- **Korrektur:** Zwei Sätze im Prüfschritt ergänzen.

### C3. Abisolieren: „Lehre auf der Klemme“ und eindrähtig/feindrähtig
- **Fundstelle:** `buildSteps()`, `HEBEL.abisolier 11`, `LUESTER.abisolier 8`.
- **Ist:** Die 11 mm für WAGO 221 stimmen (**stark**).
- **Soll:** Praxis-Tipp ergänzen: „Länge an der Abisolierlehre auf der Klemme messen; nach dem Stecken darf kein Kupfer sichtbar sein.“ Bei der Lüsterklemme sind 8 mm eine App-Annahme. Je nach Größe ist das zu lang oder zu kurz, die Regel lautet: „so lang wie die Klemmhülse, kein Kupfer vorne raus“. Evidenz: **schwach** für die 8 mm.
- **Korrektur:** Bei der Lüsterklemme statt einer festen Zahl den Text „so lang wie das Messingröhrchen (ca. 7–9 mm)“ angeben.

### C4. Lüsterklemmen im Stall
- **Fundstelle:** `checks()` → Hinweis „Lüsterklemmen: Schrauben nachziehen“.
- **Ist:** Der Hinweis ist allgemein und gut.
- **Soll:** Im Stall und in feuchter Umgebung zusätzlich: Ammoniak und Feuchte lassen Messing und Schrauben korrodieren. Schraubverbindungen setzen sich und werden locker, das gibt Übergangswiderstand und Wärme. Dort Hebelklemmen nehmen und die Dose dicht halten. Evidenz: **mittel**. Korrosion von Messing durch Ammoniak ist bekannt. Wie viel davon in einer IP66-Dose ankommt, kann ich nicht beziffern.
- **Korrektur:** Den Text ergänzen. Optional im Stall die Lüsterklemme mit Warnung „nicht empfohlen“ versehen.

### C5. Lampen: Durchgangsverdrahtung als Alternative fehlt
- **Fundstelle:** `expand()` (jede Lampe bekommt eine eigene Leitung aus der Dose).
- **Ist:** Bei 3 Lampen kommen 3 Lampenleitungen in die Dose, PE ist mit 5 Adern voll.
- **Soll:** Im Stall verdrahtet man Feuchtraum-Lichtleisten fast immer **von Leuchte zu Leuchte durch**, weil sie Durchgangsklemmen haben. Dann geht nur eine Leitung in die Dose. Das ergibt weniger Adern, kleinere Dosen und weniger Membranen, die aufgemacht werden. Evidenz: **mittel** (Praxis; Durchgangsverdrahtung muss der Leuchtenhersteller zulassen, Strom der Durchgangsklemme beachten).
- **Korrektur:** Bei „Lampe“ die Option „Lampen hintereinander (durchverdrahtet)“ anbieten. Dann entsteht 1 Leitung aus der Dose, und die Anleitung sagt: „Durchgangsklemme der Leuchte nutzen, Herstellerangabe max. Strom beachten“.

### C6. Werkzeugliste unvollständig
- **Fundstelle:** `buildMaterial()` → `werkzeug`.
- **Ist:** Zweipoliger Spannungsprüfer, Abmantelwerkzeug, Abisolierzange, Seitenschneider (+ Schlitz-Schraubendreher bei Lüster).
- **Soll:** Bohrmaschine/Akkuschrauber + Bohrer passend zum Dübel, Schraubendreher (Dosen- und Deckelschrauben, auch bei Hebelklemmen nötig), Durchgangsprüfer zum Ausklingeln (C1), Stift zum Beschriften, Isolierband (bei markierten Adern schon in der Materialliste). Spannungsprüfer nach DIN EN 61243-3, **kein Phasenprüfer-Schraubendreher**. Evidenz: **stark**.
- **Korrektur:** Die Liste ergänzen und den Satz „Phasenprüfer (‚Lügenstift‘) ist kein Spannungsprüfer“ aufnehmen.

### C7. Zwei Stromkreise in einer Dose = „Fehler“
- **Fundstelle:** `checks()` → „Zwei Zuleitungen in einer Dose“ (`fehler`).
- **Ist:** Wird hart verboten.
- **Soll:** Normativ ist es zulässig, mehrere Stromkreise in einer Dose zu führen, wenn alle Leitungen für die höchste Spannung isoliert sind und die Klemmen getrennt und gekennzeichnet werden. Gefährlich ist dabei, dass beim Freischalten der zweite Kreis vergessen wird und die N-Leiter vermischt werden (FI löst aus). Evidenz: **mittel**. Für Laien halte ich das strenge Verbot für richtig.
- **Korrektur:** Die Stufe beibehalten, aber den Text ehrlicher fassen: „Für dein Projekt: eine Dose je Stromkreis. Elektrofachkräfte machen das mit getrennten, beschrifteten Klemmen. N und PE der Kreise dürfen nie zusammen in eine Klemme.“

### C8. Dose befestigen
- **Fundstelle:** `buildSteps()` Schritt 2: „mit zwei Schrauben befestigen“.
- **Soll:** Die Abox hat innenliegende Befestigungsstellen ([hardy-schmitz](https://hardy-schmitz.de/Spelsberg%20Abox%20040-L%20Abzweigkasten-2401533.html)). Zwei diagonal gesetzte Schrauben reichen. Im Stall bzw. an Holz Edelstahl- oder verzinkte Schrauben verwenden. Die Membranen zeigen nach unten oder zur Seite, das ist bereits als Hinweis drin. Evidenz: **mittel**.
- **Korrektur:** Den Text um „diagonal“ und „im Stall rostfreie Schrauben“ ergänzen.

---

## D. Hinweise und Kleinigkeiten (fachlich korrekt, aber verbesserbar)

| # | Fundstelle | Beobachtung | Vorschlag | Evidenz |
|---|---|---|---|---|
| D1 | `DOSE.KABELTYPEN['NYM-J 4×1,5']` `quelle:'annahme'` | Die Aderfarben gn-ge/braun/schwarz/grau entsprechen HD 308 S2 für 4-adrig mit Schutzleiter. Das ist also belegt. Die Automatik nimmt für Serien- und Wechselschalter trotzdem 5×1,5 und lässt Blau als Reserve. | Auf `beleg` setzen. 5×1,5 als Standard ist praxisgerecht (Lagerware). | stark |
| D2 | `TYPES.adose.def.ip:'IP54'`, `INFO.adose` „IP54 oder IP55“, `MAT.adose` vs. `DOSE.DOSEN.ip:'IP66'` | Uneinheitlich: Der Schaltplan nennt IP54, der Dosen-Editor eine Abox mit IP66. | Angleichen (IP66 bzw. „mind. IP54, z. B. Abox IP66“). | stark |
| D3 | `checks()` blaue Ader als Schaltdraht vs. `check()` Leiterfarbe | Die Dose erlaubt Blau als markierten Schaltdraht (richtig, VDE 0100-510), der Schaltplan meldet Blau an L immer als Fehler. Für Lernende widersprüchlich. | Im Schaltplan eine Farbe „blau, schwarz markiert“ zulassen oder den Fehlertext um „außer gekennzeichnete Schalterleitung ohne N“ ergänzen. | mittel |
| D4 | `buildSteps()` PE-Schritt: „Lass ihn etwas länger … reißt als Letzter ab“ | Diese Regel gilt für Geräteanschlüsse mit Zugentlastung (Herd, Gerät). In der Dose schadet sie nicht, bringt aber wenig. | Als „gilt vor allem an Geräten“ kennzeichnen. | mittel |
| D5 | `buildSteps()` Regel 4 „Erden und Kurzschließen: in der Regel nicht nötig“ | Stimmt für die Hausinstallation. Ausnahme ist Rückspeisung durch PV, Notstromaggregat oder Batteriespeicher, im Stall z. B. ein Aggregat. | Ergänzen: „… es sei denn, PV/Aggregat/Speicher kann zurückspeisen.“ | mittel |
| D6 | `zonesFor()` Türzonen nur an der Schlossseite (`o.lock`) | Ob DIN 18015-3 ZS-t beidseitig der Tür vorsieht, konnte ich im Wortlaut nicht klären ([Baunetzwissen](https://www.baunetzwissen.de/elektro/fachwissen/leitungen-verteiler/leitungsverlegung-und-installationszonen-153012) nennt nur 10–30 cm). In der Praxis sitzt der Schalter an der Schlossseite. | Unsicher; von der Fachkraft klären lassen. Die Deckenzonen ZD-r/ZD-t (≥ 20 cm bzw. 15 cm Wandabstand, max. 30 cm breit) fehlen, sie sind laut Baunetzwissen Teil der Norm. | schwach / mittel |
| D7 | `ZDEF`, Zonen | Die Höhen 105/30 cm, ZW-o 15–45 cm unter der Decke, ZW-u 15–45 cm, ZW-m 100–130 cm und ZS 10–30 cm stimmen mit DIN 18015-3 überein ([Baunetzwissen](https://www.baunetzwissen.de/elektro/fachwissen/leitungen-verteiler/leitungsverlegung-und-installationszonen-153012)). Abzweigdose 30 cm unter der Decke ist richtig. | Bleibt so; nur für Stall/Außen anpassen (A4). | stark |
| D8 | `DOSE.DOSEN` (Abox 025/040/060/100-L) | Plausibel: Die Abox 040-L hat 11 Einführungen (10 seitlich + 1 hinten), Dichtbereich 4–21 mm, bis 4 mm², IP66, außen 102×102×57 mm ([hardy-schmitz](https://hardy-schmitz.de/Spelsberg%20Abox%20040-L%20Abzweigkasten-2401533.html)). Die Abox 025-L gibt es ([Galaxus](https://www.galaxus.ch/de/s4/product/spelsberg-abzweigkasten-abox-025-l-schutzart-ip66-max-80240001-abzweigdose-43185356)). `membranMaxD:17` ist konservativer als 21 mm. Das ist in Ordnung. | Optional `membranMaxD` für 040-L auf 21 setzen. | stark |
| D9 | `HEBEL` (WAGO 221-412/413/415: 0,2–4 mm², 11 mm, 32 A, 1 Leiter je Stelle) | Stimmt nach meiner Kenntnis. | – | mittel (nicht nachgelesen, aber Standardwissen) |
| D10 | `INFO.ls1`, `INFO.steckdose` „1,5 mm² meist B16 (abhängig von Verlegeart)“ | Richtig, aber die wichtigste Ausnahme fehlt: in wärmegedämmter Wand (Verlegeart A1) oder gebündelt nur B13/B10. | Einen Halbsatz ergänzen; dazu die Prüfung aus A1. | mittel |
| D11 | `TYPES.sls.def.cable 'NYM-J 5×10'` | Die Leitung ZS → UV hängt als Eigenschaft am SLS. Sie liegt aber hinter dem Zähler (Abzweigklemme → UV). | Feld an `hlak` oder `uv` hängen, damit Länge und Querschnitt am richtigen Bauteil stehen. | mittel |
| D12 | Haupterdungsschiene / Fundamenterder | Kommt im Modell nicht vor. Im TN-C-S gehört die PEN-Aufteilung an die Haupterdungsschiene mit Fundament- oder Ringerder (DIN 18014). Für einen Stall in einem eigenen Gebäude stellt sich die Frage nach einem eigenen Erder und Potentialausgleich. | Ein Bauteil „Haupterdungsschiene“ mit Hinweis ergänzen. | mittel |
| D13 | `RTYPES.Stall` | Weitere 705-Punkte fehlen: Not-Aus nicht in Reichweite der Tiere; Heizgeräte (z. B. Tränkenheizung) mit Betriebsanzeige; Lüftung/Licht bei Intensivhaltung in eigenen Kreisen ([WEKA](https://www.weka.de/elektrosicherheit/vde-0100-705-landwirtschaftliche-und-gartenbauliche-betriebsstaetten/)). Die genauen FI-Werte (30 mA Steckdosen ≤ 32 A, 100 mA über 32 A, 300 mA alle übrigen) kenne ich aus der Norm, konnte sie hier aber nicht im Wortlaut belegen. | Die Punkte im Stall-Text ergänzen. | stark (Liste) / mittel (FI-Werte) |
| D14 | Kreuzschaltung | Wird nicht angeboten. Für einen Stall mit 3 Türen ist sie aber üblich. | Eventuell später; dann Hinweis „besser Taster + Stromstoßschalter“. | – |
| D15 | `INFO.herd` „Fehlt N, liegen die Platten in Reihe an 400 V“ | Stimmt nur, wenn zwei Platten an verschiedenen Phasen laufen. Allgemein ist es eine Sternpunktverschiebung bis zu 400 V. | Ein Wort ergänzen: „… können zwei Platten in Reihe an 400 V liegen“. | mittel |

---

## Was ausdrücklich gut ist (bitte nicht „verbessern“)

- **Aderzuordnung:** Bei Aus-, Serien- und Wechselschaltung ist sie korrekt, getestet mit `boxModel`. Zuleitung braun = L, blau = N. Schalterleitung 3×1,5: braun = L hin, blau = geschaltet zurück, **markiert**, mit Warnung und Isolierband in der Materialliste. Lampenleitung: braun = geschaltet, blau = N. Wechsel: A bekommt L + 2 Korrespondierende, B gibt die geschaltete Phase zurück, Blau als Reserve.
- **Reserveadern der Zuleitung:** Je eine eigene Klemme und der Hinweis „können Spannung führen“. So ist es richtig.
- **Brücken** haben die Farbe und den Querschnitt der dicksten Ader. Hebelklemmen werden für Laien empfohlen.
- **5 Sicherheitsregeln:** zweipoliger Prüfer, vorher und nachher testen, alle Adern messen. Prüfung nach VDE 0100-600 durch die Elektrofachkraft inkl. Isolationsmessung, Polarität, Schleifenimpedanz und FI.
- **Zugentlastung** über eine Schelle kurz vor der Dose („Membran ist keine Zugentlastung“), Mantel 5–10 mm in der Dose, Einführung im Feuchtraum von unten oder seitlich.
- **Schaltplan:** Farben L1/L2/L3 braun/schwarz/grau, N blau, PE grün-gelb, PEN grün-gelb mit blauer Markierung. PEN ≥ 10 mm². N-Schiene je FI, „L und N über denselben FI“, „N und PE nach der Aufteilung nie wieder verbinden“, Herd mit 3-poligem LS, Licht B10.

---

## Priorisierte Umsetzungsreihenfolge

1. A1 + A2 (LS ↔ Querschnitt, Auto-Querschnitt der Dose): ein Check und eine Zeile. Beides ist brandrelevant.
2. A3–A5 (Stall: Potentialausgleich, Reichweite und Höhen, feuergefährdet IP54): betrifft direkt das Pferdestall-Projekt.
3. A6 (Spannungsfall/Länge): beim Stall wahrscheinlich.
4. B1–B5 (NYY und Nagerschutz, Kabelmeter, Raumtyp in der Materialliste, Kleinteile, Verdrahtung in Metern).
5. C1–C2 (Leitungen beschriften, Deckel und Beschriftung): kleiner Aufwand, großer Nutzen auf der Baustelle.
6. Rest.

## Quellen

- WEKA, „VDE 0100-705 – landwirtschaftliche und gartenbauliche Betriebsstätten“: https://www.weka.de/elektrosicherheit/vde-0100-705-landwirtschaftliche-und-gartenbauliche-betriebsstaetten/
- Waskönig+Walter, Datenblatt (N)YM-J (Einsatzbereich, „nicht im Erdreich“): https://www.waskoenig.de/download/0007?locale=de
- Spelsberg Abox 040-L, Händlerdaten (11 Einführungen, 4–21 mm, 4 mm², IP66, innenliegende Befestigung, geschraubter Deckel): https://hardy-schmitz.de/Spelsberg%20Abox%20040-L%20Abzweigkasten-2401533.html
- Spelsberg Abox 025-L (Existenz/Typ): https://www.galaxus.ch/de/s4/product/spelsberg-abzweigkasten-abox-025-l-schutzart-ip66-max-80240001-abzweigdose-43185356
- Baunetzwissen, Installationszonen DIN 18015-3: https://www.baunetzwissen.de/elektro/fachwissen/leitungen-verteiler/leitungsverlegung-und-installationszonen-153012
- Baunetzwissen, Schutzbereiche im Bad: https://www.baunetzwissen.de/bad-und-sanitaer/fachwissen/elektroinstallationen/elektrische-schutzbereiche-im-bad-172830
- Ohne Online-Beleg, aus Normkenntnis (Evidenz entsprechend „mittel“): VDE 0100-430 (I_B ≤ I_n ≤ I_z), VDE 0298-4 (Belastbarkeit), DIN 18015-1 (Spannungsfall 3 %), VDE 0100-510 (Blau als Außenleiter ohne N), HD 308 S2 (Aderfarben).

Hinweis: Diese Prüfung ersetzt nicht die Planung und Erstprüfung durch eine Elektrofachkraft.
