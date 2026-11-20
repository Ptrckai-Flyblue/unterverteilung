# Fachliche und didaktische Prüfung „Unterverteilung – Baukasten“

Prüfer: Sicht Berufsschullehrer Elektroniker/in EGT (mit Meistertitel)
Gegenstand: `/home/claude/unterverteilung/index.html` (nicht verändert), Netzform TN-C-S
Stand: 20.11.2026

## Vorgehen

- Alle Erklärtexte im Quelltext gelesen: `INFO` und `FIT`, `GSTEPS` (14 Schritte), `openHelp()`, alle `toast()`-Meldungen, `check()` und `roomRules()` mit `RTYPES`, die Abzweigdose (`FN`, `checks()`, `buildSteps()`), das Messgerät `volt()`, `simulate()`, `computeRT()`, `onTick()`, `touch()` und `fiTest()`.
- Die Simulation lief im Browser (Playwright und Chrome, Skripte unter `/home/claude/lehrer_scripts/t1.py` und `t2.py`). Bilder liegen in `/home/claude/review2/shots_lehrer/`. Getestet habe ich diese Fälle: Lichtkreis fertig, SLS aus, Schalter aus, N an der Lampe offen, Körperschluss mit und ohne PE, mit 30-mA-FI, mit 300-mA-FI und ohne FI, Kurzschluss, Überlast 5 kW an B16, Herd ohne N, Prüftaste und die Ausgangslage des geführten Aufbaus.
- Wie sicher die Quellen sind: **[N]** heißt Normen- und Fachbuchwissen, sicher. **[W]** heißt im Web nachgeprüft. **[U]** heißt, ich bin unsicher. Das steht dann beim Punkt.

Reihenfolge der Einstufung: **GEFÄHRLICH**, dann **fachlich falsch**, dann **missverständlich**, dann **Hinweis**.

---

## A. GEFÄHRLICH: falsch, oder ein zwingender Sicherheitshinweis fehlt

### A1 · „Du bist geschützt“ nach dem Berühren eines Gehäuses unter Spannung, auch beim 300-mA-FI
- **Fundstelle:** `touch(d)`, Toast nach der FI-Auslösung
- **Ist:** „Du hast das Gehäuse … berührt. Strom floss durch deinen Körper zur Erde … Der FI hat das in ca. 20–40 ms erkannt. **Du bist geschützt.**“ Der Test zeigt: Der Text kommt genauso, wenn der FI ein **300-mA-FI** ist. `touch()` nimmt einfach den FI mit dem kleinsten IΔn, egal wie groß der ist.
- **Soll (30 mA):** „Der FI hat nach etwa 20–40 ms abgeschaltet. Den Schlag hast du trotzdem bekommen, nur kurz. Meist überlebt man das, aber man kann sich erschrecken und zum Beispiel von der Leiter fallen. Der FI ist ein **zusätzlicher** Schutz. Er ersetzt den Schutzleiter nicht. Ein Gehäuse ohne PE ist immer ein Fehler.“
- **Soll (300 mA):** keine Entwarnung. Stattdessen: „Ein 300-mA-FI ist ein Brandschutz und schützt keine Menschen. Der Strom durch den Körper (etwa 100–250 mA) kann unter seiner Auslöseschwelle bleiben. Lebensgefahr!“ In der Simulation soll ein FI mit IΔn über 30 mA bei Berührung **nicht** sicher auslösen.
- **Begründung:** Nach DIN VDE 0100-410 (415.1) ist der 30-mA-FI ein *zusätzlicher Schutz*. Er begrenzt die Einwirkdauer, aber der Strom fließt trotzdem. 300 mA liegen weit über der Schwelle, ab der Herzkammerflimmern möglich ist (IEC 60479-1, ab etwa 50 mA bei längerer Einwirkdauer). **[N]**

### A2 · Offener Neutralleiter: Die App zeigt „keine Spannung“, in echt liegen 230 V an
- **Fundstelle:** `buildNets()`. Der Verbraucher selbst ist dort keine leitende Verbindung zwischen L und N. Dazu `volt()` und die `check()`-Meldung „Neutralleiter N fehlt“.
- **Ist (getestet):** Ich habe die N-Leitung zur Lampe entfernt, Schalter ein. Messung Lampe N gegen PE: „**– – –** Kein Messwert: Mindestens ein Punkt hat keine Verbindung zum Netz“. `check()` meldet nur: „Neutralleiter N fehlt, **das Gerät bekommt keinen Strom**.“
- **Soll:** Der N-Anschluss am Gerät liegt über den Verbraucher (Glühfaden, Netzteil, Heizwiderstand) auf L-Potential. Das Messgerät soll **230 V** zeigen. Meldung: „Neutralleiter N unterbrochen: Das Gerät geht nicht. **Achtung: Am N-Anschluss des Geräts und an der ganzen Ader bis zur Unterbrechung liegen 230 V gegen PE an.** Ein unterbrochener N ist genauso gefährlich wie L.“
- **Begründung:** Das ist ein klassischer Unfall in der Praxis. Der Monteur hält den blauen Leiter für spannungsfrei, weil „das Licht ja nicht geht“. Grundwissen aus dem Fachbuch (z. B. Fachkunde Elektrotechnik, Abschnitt Schutzmaßnahmen und Fehlerarten). **[N]**

### A3 · Die Fehlvorstellung „N und PE sind ungefährlich“ wird an mehreren Stellen bestärkt
- **Fundstellen und Ist:**
  1. `check()`, Leiterfarbe: „Außenleiter darf nicht blau sein … man würde eine Spannung führende Ader **für ungefährlich halten**“. Das sagt indirekt: Blau ist ungefährlich.
  2. `volt()`: „N bzw. PE gegeneinander – beide liegen auf Erdpotential.“
  3. `volt()` bei der Messung Lampe N gegen PE im normalen Lichtkreis (getestet): „0 V · Beide Punkte sind **direkt miteinander verbunden**.“ Das stimmt nicht. Sie sind nur an der PEN-Aufteilung im Zählerschrank verbunden.
  4. Hilfe: „Gelb leuchtende Punkte und Leitungen führen Spannung.“ N und PE leuchten nie, auch nicht am PEN des HAK.
- **Soll:**
  1. „… sonst hält man sie für den Neutralleiter oder den Schutzleiter. Beide dürfen nie eigenmächtig als Außenleiter genutzt werden.“ Das Wort „ungefährlich“ muss weg.
  2. und 3. „N gegen PE: ungefähr 0 V. N und PE sind nur an **einer** Stelle verbunden, an der PEN-Aufteilung im Zählerschrank. Unter Last sind ein paar Volt normal. Zeigt das Messgerät deutlich mehr, ist das ein Fehler (z. B. N unterbrochen). Dann sofort freischalten und die Elektrofachkraft holen.“
  4. In der Hilfe ergänzen: „Nicht leuchtend heißt nur: in **diesem** Modell spannungsfrei. In echt kann N bei einem Fehler Spannung führen, ebenso PE und PEN (z. B. bei einem PEN-Bruch). Spannungsfreiheit nur mit einem zweipoligen Spannungsprüfer feststellen.“
- **Begründung:** DIN VDE 0105-100, 3. Sicherheitsregel: Spannungsfreiheit **allpolig** feststellen. Beim PEN-Bruch im TN-C-Teil können alle Körper mit PE Spannung annehmen. Die Dose-Anleitung der App (`FN.N`) sagt es schon richtig: „Kann bei einem Fehler … trotzdem Spannung führen – vor dem Anfassen immer messen.“ Diesen Satz auch im Hauptteil verwenden. **[N]**

### A4 · Der geführte Aufbau verdrahtet unter Spannung. Freischalten und Prüfung vor dem Einschalten fehlen
- **Fundstelle:** `lichtkreis(false)` und `startGuide()`, `GSTEPS[0..13]`, Text „Geschafft“
- **Ist (getestet, Bild `guide0.png`):** Beim Start sind alle HAK-Sicherungen eingesetzt, SLS, FI und LS stehen auf EIN. Schon vor Schritt 1 leuchten die HAK-Klemmen gelb. Nach Schritt 1 führt die Sammelschiene L1 Spannung. Bis Schritt 13 wird an Teilen unter Spannung „angeklemmt“. In Schritt 14 heißt es dann „Schalte den Lichtschalter ein“ und danach „Geschafft – Die Lampe leuchtet!“. Ein Hinweis auf die 5 Sicherheitsregeln oder die Erstprüfung kommt im ganzen Ablauf nicht vor. Die 5 Regeln stehen nur in der Abzweigdosen-Anleitung.
- **Soll:**
  - Neuer Schritt 0, als Hinweiskasten vor Schritt 1: „In echt arbeitet man **nie** unter Spannung. Vor jeder Arbeit gelten die 5 Sicherheitsregeln: 1. Freischalten. 2. Gegen Wiedereinschalten sichern. 3. Spannungsfreiheit feststellen (zweipolig, allpolig). 4. Erden und kurzschließen (in der Hausinstallation meist nicht nötig). 5. Benachbarte Teile unter Spannung abdecken. In diesem Modell ist der Strom zum Üben schon da, damit du nach jedem Schritt siehst, was passiert.“
  - Besser noch technisch lösen: Beim geführten Aufbau SLS, FI und LS auf AUS starten. Vor dem Einschalten kommt ein eigener Schritt, und erst danach wird eingeschaltet.
  - Neuer Schritt 14 vor dem Einschalten: „**Prüfen vor dem ersten Einschalten** (macht die Elektrofachkraft, DIN VDE 0100-600): Sichtprüfung, Durchgang des Schutzleiters, Isolationswiderstand. Erst dann einschalten. Danach Schleifenimpedanz bzw. Abschaltbedingung, FI-Auslösung und Drehfeld messen und alles im Prüfprotokoll festhalten.“
  - Der jetzige Schritt 14 wird Schritt 15: „Einschalten: erst LS, dann Schalter.“ Danach „Prüftaste am FI drücken“.
- **Begründung:** DIN VDE 0105-100 (5 Sicherheitsregeln). Nach DIN VDE 0100-600 ist eine Erstprüfung vor der Inbetriebnahme Pflicht. Didaktisch gilt: Was der Laie in der Übung sieht, macht er später nach. **[N]**

### A5 · „Der Hauptschalter schaltet das ganze Haus ab“
- **Fundstellen:** `TYPES.sls.sub` („Schaltet das ganze Haus ab“), `INFO.sls.e` („Damit schaltest du die ganze Anlage ab.“), `GSTEPS[2].e` („Der Hauptschalter schaltet das ganze Haus ab.“)
- **Ist (getestet):** Ich habe den SLS ausgeschaltet. Die Messung SLS-Eingang L1 gegen N zeigt **230 V**. Die Simulation ist hier richtig, die Texte nicht.
- **Soll:** „Der Hauptschalter schaltet alles **hinter** ihm ab: Zähler, Unterverteilung, Stromkreise. **Vor** ihm bleibt alles unter Spannung: das Kabel vom Hausanschluss, die Sammelschienen unten im Zählerschrank, der Überspannungsschutz darauf und die Eingangsklemmen des Hauptschalters selbst. Diese Spannung schaltet nur der Netzbetreiber ab. Hast du eine PV-Anlage, einen Batteriespeicher oder ein Notstromaggregat, kann auch von dort Spannung kommen.“ Kurztext: „Schaltet alles hinter dem Zähler ab“.
- **Begründung:** Aufbau nach VDE-AR-N 4100. Der netzseitige Anschlussraum hängt vor dem SLS direkt am HAK, abgesichert nur über NH 63 A. Dort sind die Kurzschlussströme hoch und es besteht Lichtbogengefahr. **[N]** Im NAR wird eine „Trennvorrichtung (Empf. SH-Schalter E 35 A)“ und ein SPD Typ 1 genannt **[W: EAM Netz, Aufbauhinweise direkt messende Zähleranlagen]**.

### A6 · Rechtlicher Hinweis zum vorgezählten Bereich fehlt
- **Fundstellen:** `INFO.ss5.t` („Die Hauptleitung vom HAK an die Klemmen unten anschließen.“), `GSTEPS[0..3]`, Hilfe („… gehören in die Hände einer Elektrofachkraft“)
- **Ist:** Die App vermittelt: Eine Elektrofachkraft darf das. Der Laie verdrahtet in der Übung selbst HAK, Sammelschiene und PEN.
- **Soll** (Hilfe und Info zu HAK, Sammelschiene und SLS): „Am Hausanschluss und im Zählerschrank vor dem Zähler darf nur der Netzbetreiber oder ein **im Installateurverzeichnis des Netzbetreibers eingetragener** Elektrobetrieb arbeiten. Dafür reicht es nicht, Elektrofachkraft zu sein. Plomben nie selbst öffnen. Neue Anlagen und Änderungen müssen beim Netzbetreiber angemeldet werden.“
- **Begründung:** Nach § 13 Abs. 2 NAV darf die Anlage hinter der Hausanschlusssicherung außer vom Netzbetreiber nur von einem eingetragenen Installateur errichtet, erweitert und geändert werden. Für den Bereich zwischen Hausanschlusssicherung und Messeinrichtung gilt das auch bei der Instandhaltung **[W: buzer.de, § 13 NAV]**. Die Seite gesetze-im-internet.de war für mich nicht erreichbar (Freigabe abgelaufen). **[N/W]**

---

## B. Fachlich falsch

### B1 · Die Aufgaben von LS, PE und FI im TN-System sind falsch dargestellt
- **Fundstellen:**
  - `INFO.ls1.t[3]`: „Schützt die Leitung, nicht den Menschen. Dafür ist der FI da.“
  - `TYPES.fi4.sub` und `fi2.sub`: „Schützt Menschen“. Das gilt auch für die 300-mA-Variante.
  - `simulate()`: Bei Körperschluss mit PE löst **nur der FI** aus (getestet).
  - Der Zusatz ohne FI lautet: „Ohne FI wird der Körperschluss **nur** über den hohen Kurzschlussstrom L–PE abgeschaltet.“
- **Ist:** Der Lernende versteht: Der Mensch ist durch den FI geschützt, der LS ist nur für die Leitung da.
- **Soll:**
  - `ls1`: „Schützt die Leitung vor Überlast und Kurzschluss. Zusammen mit dem Schutzleiter schützt er auch den Menschen: Bekommt ein Metallgehäuse Spannung, fließt über PE ein sehr hoher Strom, und der LS schaltet in Millisekunden ab. Dafür muss der Schutzleiter durchgehend angeschlossen sein. Das ist die eigentliche Schutzmaßnahme im TN-Netz (Abschaltung in höchstens 0,4 s). Der 30-mA-FI ist ein zusätzlicher Schutz, falls doch einmal Strom durch einen Menschen fließt.“
  - FI-Untertitel: „30 mA: zusätzlicher Personenschutz · 300 mA: Brandschutz“
  - Toast bei Körperschluss mit PE: „Körperschluss an … Über PE fließt ein sehr hoher Fehlerstrom (wie bei einem Kurzschluss). Abschalten können **beide**: der FI, weil Strom fehlt, und der LS mit seinem Schnellauslöser. Wer schneller ist, hängt vom Strom ab.“ Ohne FI das Wort „nur“ streichen: „Ohne FI schaltet der LS ab. Das ist die normale Schutzmaßnahme im TN-Netz.“
- **Begründung:** DIN VDE 0100-410, 411.3 (Fehlerschutz durch automatische Abschaltung) und 411.3.2.2 (TN, Endstromkreise bis 32 A: höchstens 0,4 s). 415.1 regelt den RCD als zusätzlichen Schutz. Die App sagt es in `FN.PE` der Dose-Anleitung selbst richtig („damit Sicherung oder FI sofort abschalten“). **[N]**

### B2 · Herd ohne N: Die Simulation sagt „Kein Strom“
- **Fundstelle:** Statusanzeige `statusOf()` und `check()` beim Herd. `powered()` verlangt N auf Erdpotential.
- **Ist (getestet):** N am Herd entfernt, Status: „Kein Strom“. Meldung: „Neutralleiter N fehlt, das Gerät bekommt keinen Strom.“ Das widerspricht `INFO.herd.t[2]` („Fehlt N, liegen die Platten in Reihe an 400 V“).
- **Soll:** Bei Drehstromverbrauchern ohne N wird die Meldung zum Fehler: „N fehlt: Die Platten laufen in Sternschaltung ohne Mittelpunktleiter weiter. Sind sie ungleich belastet, verschiebt sich der Sternpunkt. Einzelne Platten bekommen dann mehr als 230 V, bis zu 400 V. Dabei können Geräte und Leitungen überhitzen. Das gilt genauso für ein ganzes Haus, wenn der N bzw. PEN zur Unterverteilung unterbrochen ist.“ Die Simulation soll das wenigstens anzeigen, etwa mit „Platten heizen unterschiedlich stark, Überspannung“.
- **Begründung:** Sternschaltung mit unsymmetrischer Last ohne N führt zur Sternpunktverschiebung (Fachkunde und Tabellenbuch Elektrotechnik, Drehstrom). Ein N- bzw. PEN-Bruch ist eine häufige Brandursache. **[N]**

### B3 · Überlast: Zeitangabe falsch, Auslösung schon unter 1,45 × In
- **Fundstelle:** `computeRT()` (Auslösung ab r > 1,13 nach 8 s, ab r ≥ 1,45 nach 3 s) und der Toast in `onTick()`
- **Ist (getestet):** 5 kW an B16 sind 21,7 A, also 136 %. Nach 8 s kommt: „In echt dauert das **Sekunden bis Minuten**“.
- **Soll:** „Überlast: 21,7 A bei 16 A (136 %). Zwischen dem 1,13- und dem 1,45-fachen Nennstrom löst ein LS nach Norm irgendwann innerhalb von mehr als einer Stunde aus, vielleicht auch gar nicht. Erst ab dem 1,45-fachen muss er innerhalb einer Stunde abschalten, beim 2- bis 3-fachen nach Sekunden bis etwa einer Minute. Hier ist die Zeit stark gerafft.“ Didaktisch besser: Im Bereich 1,13–1,45 eine Warnung zeigen („Leitung wird dauerhaft zu warm“) statt einer schnellen Auslösung, oder die Raffung im Panel deutlich sichtbar machen.
- **Begründung:** DIN EN 60898-1: kleiner Prüfstrom 1,13 × In, keine Auslösung in 1 h. Großer Prüfstrom 1,45 × In, Auslösung in unter 1 h. Bei 2,55 × In: 1–60 s (In bis 32 A). Der Text in `INFO.ls1.t[0]` ist richtig, der Toast nicht. **[N]**

### B4 · Bad (DIN VDE 0100-701): Bereichsregeln falsch zusammengefasst
- **Fundstelle:** `RTYPES['Bad / Dusche'].hint/.text`
- **Ist:** „In den Bereichen 0–2 (bis 60 cm um Wanne/Dusche) keine Steckdosen und Schalter; Leuchten dort mindestens IPX4.“
- **Soll:** „Bereich 0 (in der Wanne oder Dusche): nur fest angeschlossene Geräte mit Kleinspannung (SELV bis 12 V AC), mindestens IPX7. Bereich 1 (über der Wanne bzw. Dusche bis 2,25 m): keine Steckdosen und Schalter für 230 V, nur Geräte, die dafür zugelassen sind, mindestens IPX4 (IPX5 bei Strahlwasser). Bereich 2 (60 cm daneben): keine 230-V-Steckdosen (außer Rasiersteckdose mit Trenntrafo). Schalter und andere fest installierte Geräte sind erlaubt, mindestens IPX4. Alle Stromkreise im Bad über einen FI mit höchstens 30 mA.“
- **Begründung:** Laut baunetzwissen sind in Bereich 2 „alle fest installierten Installationsgeräte“ außer 230-V-Steckdosen zulässig, und Bereich 0 verlangt IPX7 **[W: baunetzwissen.de, Elektrische Schutzbereiche im Bad]**. **[U]** Für Bereich 1 nennt dieselbe Quelle IPX5. Ich habe IPX4 in Erinnerung (IPX5 nur bei Strahlwasser). Bitte in der Norm nachsehen. Die jetzige App-Regel ist strenger als die Norm und damit nicht gefährlich, aber fachlich falsch.

### B5 · Leiterfarbe: Die App widerspricht sich selbst und der Norm
- **Fundstelle:** `check()`, Fehler „Außenleiter (L…) darf nicht blau sein“, gegenüber `DOSE.checks()`: „dann darf die blaue Ader als Phase genutzt werden (DIN VDE 0100-510)“ mit Kennzeichnung
- **Ist:** Im Schaltplan ist eine blaue Ader als L immer ein *Fehler*. In der Abzweigdose ist sie erlaubt, wenn sie gekennzeichnet ist.
- **Soll** (im Schaltplan als Warnung, nicht als Fehler): „Blau ist für N reserviert. Hat eine Leitung keinen N (z. B. Schalterleitung), darf die blaue Ader als Außenleiter dienen, muss aber an beiden Enden schwarz oder braun gekennzeichnet werden. Besser: eine Leitung ohne blaue Ader nehmen.“ Grün-gelb bleibt ein Fehler, ohne Ausnahme.
- **Begründung:** DIN VDE 0100-510, Abschnitt 514.3.1, und DIN EN 60445. **[N]**

### B6 · Stall, Lager (DIN VDE 0100-705 bzw. -420): zwingende Punkte fehlen
- **Fundstelle:** `RTYPES['Stall / Landwirtschaft']`. Wichtig für den geplanten Pferdestall.
- **Ist:** „IP44, FI ≤ 300 mA (Brandschutz), Steckdosen 30 mA, Leuchten geschützt …“
- **Soll (ergänzen):** „Ab dem Einspeisepunkt als TN-S verlegen, also mit getrenntem N und PE und **ohne PEN** im Stall. In **allen** Stromkreisen ein FI: Steckdosen bis 32 A mit 30 mA, alle anderen höchstens 300 mA. Wo Tiere stehen, ist ein **zusätzlicher Schutzpotentialausgleich** Pflicht, z. B. eine Baustahlmatte im Boden, angeschlossen an den Potentialausgleich. Pferde reagieren schon auf kleine Berührungsspannungen.“ Wenn möglich auch eine Prüfregel „Stall ohne FI ≤ 30 mA an Steckdosen“ als Fehler.
- **Begründung:** Für landwirtschaftliche Betriebsstätten verlangt die Norm RCDs in allen Stromkreisen, TN-Anlagen ab der Einspeisung als TN-S, einen zusätzlichen Schutzpotentialausgleich in Bereichen mit Tieren und mindestens IP44 **[W: WEKA, VDE 0100-705]**. Die mA-Werte 30, 100 und 300 sind aus der Norm bekannt **[N]**, aber nicht im Web nachgeprüft **[U, gering]**.

### B7 · Kurzschluss und Körperschluss werden in den Meldungen gleich benannt
- **Fundstelle:** `shortDesc()`. Sie zeigt bei L–N und bei L–PE immer „L1 ↔ N/PE“ und nennt beides „Kurzschluss“.
- **Soll:** L–N: „Kurzschluss (L1 ↔ N)“. L–PE bzw. L–Gehäuse: „Körperschluss mit hohem Fehlerstrom (L1 ↔ PE)“. L–L: „Kurzschluss zwischen Außenleitern“. So lernt man die Begriffe Kurzschluss, Körperschluss, Erdschluss und Fehlerstrom sauber auseinanderzuhalten.
- **Begründung:** Begriffe nach DIN VDE 0100-200 bzw. IEV 826. **[N]**

---

## C. Missverständlich

### C1 · „Geschalteter Außenleiter: führt nur Spannung, wenn der Schalter an ist“
- **Fundstelle:** `GSTEPS[10].e`, ähnlich `INFO.schalter`
- **Soll ergänzen:** „Achtung: Schalter AUS heißt **nicht** spannungsfrei. Im Schalter und in der Dose liegt weiter 230 V an. Ist der Schalter falsch angeschlossen oder ist es ein Wechselschalter, kann auch die Lampe Spannung haben. Zum Lampenwechsel oder Anklemmen immer den LS ausschalten und messen.“ **[N]**

### C2 · Messgerät: „Kein Messwert“ bei abgeschalteten Leitern
- **Fundstelle:** `volt()`, Ergebnis `v:null` (getestet bei SLS aus und Schalter aus)
- **Ist:** „– – – Kein Messwert … Leiter offen oder abgeschaltet“
- **Soll:** Ein echter Spannungsprüfer zeigt dort „keine Spannung“ bzw. etwa 0 V. Vorschlag: „0 V – hier ist der Leiter abgeschaltet. In echt gilt eine Stelle erst als spannungsfrei, wenn du **alle** Leiter gegeneinander und gegen PE gemessen hast (L–N, L–PE, N–PE). Den Prüfer vorher und nachher an einer Steckdose testen.“ Ausnahme ist der offene N (siehe A2): Dort 230 V zeigen. **[N]**

### C3 · Prüftaste: „er funktioniert“
- **Fundstelle:** `fiTest()`
- **Soll:** „… hat ausgelöst – seine Mechanik funktioniert. Die Prüftaste prüft **nicht**, ob die Schutzleiter angeschlossen sind oder ob der FI schnell genug abschaltet. Das misst die Elektrofachkraft mit einem Prüfgerät.“ **[N]**

### C4 · Knopf „Alles korrekt“
- **Fundstelle:** `live()`, Beschriftung des Prüfknopfs
- **Soll:** „Keine Planfehler gefunden“. Der Hinweis auf DIN VDE 0100-600 steht nur in der aufgeklappten Liste. Ein Laie liest „Alles korrekt“ als Freigabe. **[didaktisch]**

### C5 · Reihenfolge an der Lampe: PE „vor N“, aber nach L
- **Fundstelle:** `GSTEPS[9..12]`. L kommt in Schritt 11, PE erst in Schritt 12 mit der Begründung „Darum kommt er vor N dran.“
- **Soll:** Am Verbraucher die Reihenfolge PE, dann N, dann L (zuletzt die geschaltete Ader vom Schalter). Text: „PE immer zuerst anklemmen und zuletzt abklemmen.“ Das passt dann auch zur Dose-Anleitung („Schutzleiter zuerst“). **[N, gängige Praxisregel]**

### C6 · Schritt 1: „Der Hausanschluss sitzt ganz unten im Keller“ und „Für einen Lichtkreis reicht L1“
- **Soll:** „Der Hausanschluss sitzt meist im Keller oder im Hausanschlussraum, manchmal auch in einer Nische außen. In echt werden immer alle drei Außenleiter und der PEN angeschlossen. Hier verbinden wir nur L1, damit es übersichtlich bleibt.“ **[didaktisch]**

### C7 · Zuleitung zur Unterverteilung ohne Klemme, Querschnitt und passende Absicherung
- **Fundstelle:** `GSTEPS[4..8]` (Zähler wird direkt mit dem FI verbunden, PE kommt von der unteren Sammelschiene) und `check()`. Dort wird nichts davon geprüft.
- **Ist:** Die Hauptleitungsabzweigklemme (`hlak`) kommt im Lichtkreis nicht vor. Den Leitungsquerschnitt der Zuleitung zur UV prüft die App nicht. Ein SLS mit E50 oder E63 ist mit „NYM-J 5×10“ ohne Warnung möglich. Ob der FI 40 A vor Überlast geschützt ist, prüft die App auch nicht, z. B. bei einem SLS über 40 A.
- **Soll:** Im Text von Schritt 5 ergänzen: „In echt geht vom oberen Anschlussraum (Abzweigklemme) **eine** 5-adrige Leitung (L1, L2, L3, N, PE) zur Unterverteilung. Ihr Querschnitt muss zur vorgeschalteten Absicherung passen, z. B. 10 mm² Cu bei E35, je nach Verlegeart und Länge. Das legt die Elektrofachkraft fest.“ Neue `check()`-Hinweise: „SLS-Nennstrom zu groß für die Zuleitung“ und „FI ist nicht gegen Überlast geschützt (Nennstrom der Vorsicherung größer als der des FI)“. **[N; Werte abhängig von der Verlegeart, bewusst nur als Beispiel genannt]**

### C8 · Dose-Anleitung: „führt Strom“ statt „führt Spannung“
- **Fundstelle:** `FN.L` („Führt immer Strom (230 V)“), `FN.S` und `FN.K` („führt … Strom“), `infoFor` („führt nur Strom, wenn eingeschaltet ist“)
- **Soll:** „Führt immer **Spannung** (230 V gegen N und PE), solange der LS eingeschaltet ist. Strom fließt erst, wenn ein Gerät eingeschaltet ist.“ Spannung und Strom zu verwechseln ist der häufigste Begriffsfehler bei Anfängern. **[N]**

### C9 · Herd-Info: „Fehlt N, liegen die Platten in Reihe an 400 V“
- **Soll:** „Fehlt N, hängen die Platten ohne Mittelpunkt im Stern. Bei ungleicher Last bekommt eine Platte mehr als 230 V (Sternpunktverschiebung). Sind nur zwei Platten an, liegen sie in Reihe an 400 V.“ **[N]**

### C10 · PEN-Info unvollständig
- **Fundstelle:** `INFO.pen`, `GSTEPS[1].e`
- **Soll ergänzen:** „Der PEN wird zuerst an die PE-Schiene bzw. PE-Klemme angeschlossen, von dort geht es zur N-Schiene. Der PEN darf nie geschaltet oder unterbrochen werden. Bricht er, können alle Metallteile mit Schutzleiter Spannung bekommen. An der Aufteilung wird die Haupterdungsschiene angeschlossen (Fundamenterder, Schutzpotentialausgleich für Wasser, Heizung und Gas).“
- **Begründung:** DIN VDE 0100-540, Abschnitt 543.4: PEN an die Klemme bzw. Schiene des Schutzleiters, kein Schalten. DIN VDE 0100-410, Abschnitt 411.3.1.2: Schutzpotentialausgleich über die Haupterdungsschiene. Im ganzen Modell kommen Haupterdungsschiene und Schutzpotentialausgleich nicht vor. Das Wort „Hauptpotentialausgleich“ steht nur im Text der PE-Schiene, und dieser Begriff ist veraltet. **[N]**

---

## D. Hinweise (Feinschliff, Begriffe, Unsicheres)

- **D1 · „Strangspannung“** (`volt()`): Besser „230 V: Spannung zwischen Außenleiter und N (bzw. PE)“, also U₀. Die verkettete Spannung von 400 V heißt „Außenleiterspannung“. „Strangspannung“ ist in Lehrbüchern nicht falsch, gehört aber eigentlich zum Strang eines Verbrauchers oder Erzeugers. **[N]**
- **D2 · Umgangssprache:** „Sicherung“ für LS und „Phase“ für Außenleiter (Dose) einmal erklären: „Sicherung = Leitungsschutzschalter (LS)“, „Phase = umgangssprachlich für Außenleiter L“. **[didaktisch]**
- **D3 · SPD 3+0 auf der PE-Schiene nach der Aufteilung** (`spdss`, Beschriftung „3+0 · L–PEN“, greift aber an der PE-Sammelschiene ab): Liegt die PEN-Aufteilung direkt daneben, ist das in der Praxis üblich. Wo genau die Grenze liegt, ab der zusätzlich ein N–PE-Ableiter (3+1 bzw. 4+0) nötig ist, konnte ich nicht sicher belegen **[U]**. Ich habe einen Abstand von 0,5 m in Erinnerung, gefunden habe ich nur die 0,5-m-Regel für die Länge der Anschlussleitungen **[W: DEHN DS273]**. Vorschlag für die Info: „3+0 gilt für den TN-C-Teil bzw. direkt an der PEN-Aufteilung. Weiter hinten (TN-S) ist 3+1 oder 4+0 nötig.“
- **D4 · Überspannungsschutz in der UV „immer vor dem FI“:** steht richtig in `INFO.spd`, aber `check()` prüft es nicht. Ergänzen. **[N]**
- **D5 · `INFO.spd`:** „laut VDE-AR-N 4100 im gezählten Bereich … Typ 1 vor dem Zähler nur … in Absprache“. Laut den Aufbauhinweisen von EAM Netz ist im netzseitigen Anschlussraum ein SPD Typ 1 vorgesehen, im anlagenseitigen Raum Typ 1 oder 2 **[W]**. Die Regel ist je nach TAB des Netzbetreibers verschieden. Vorschlag: „Wo genau, legen die VDE-AR-N 4100 und die TAB des Netzbetreibers fest.“
- **D6 · „Kein Hauptschalter (SLS) … gehört vor den Zähler“:** Die VDE-AR-N 4100 verlangt dort eine Trennvorrichtung. Ein SH-Schalter ist der übliche Weg bzw. eine Empfehlung **[W: EAM „Empf. SH-Schalter E 35 A“]**. Formulierung: „üblich und meist von der TAB gefordert“.
- **D7 · FI-Typ:** Fehlt ganz. Ergänzen: „Typ A ist Mindeststandard. Für Wallbox, Wärmepumpe oder Frequenzumrichter ist je nach Hersteller Typ F oder Typ B (bzw. eine DC-Fehlerstromerkennung) nötig.“ **[N]**
- **D8 · „Weg des Stroms: Hin über L, zurück über N“:** Für Laien in Ordnung. Ein Satz dazu: „Bei Wechselstrom wechselt die Richtung 50-mal pro Sekunde. ‚Hin‘ und ‚zurück‘ ist ein Bild.“ **[didaktisch]**
- **D9 · PV, Speicher, Notstrom:** Nirgends erwähnt, dass solche Anlagen auch nach dem Abschalten noch Spannung einspeisen können. In die Hilfe und zu den 5 Regeln aufnehmen („alle Einspeisungen freischalten“). **[N]**
- **D10 · Installationszone ZS-t nur auf der Schlossseite** (`zonesFor`, Text Wandansicht): Laut DIN 18015-3 gibt es senkrechte Zonen 10–30 cm neben Türen. Ob die Norm nur die Schlossseite meint oder beide Seiten, weiß ich nicht sicher **[U]**. Bitte in der Norm nachsehen. Die anderen Maße (ZW-o und ZW-u 15–45 cm, ZW-m 100–130 cm, ZS 10–30 cm, Vorzugshöhen 30 und 105 cm) sind richtig **[N]**.
- **D11 · „Lebensgefahr … fließt ungebremst durch den Körper“:** Besser „Der Strom wird nur durch deinen Körper begrenzt, etwa 0,1–0,3 A. Das ist lebensgefährlich.“ **[N]**
- **D12 · Feuchter Keller IP44:** strenger als die Mindestanforderung, als Empfehlung in Ordnung. Als „Empfehlung“ kennzeichnen. **[U, gering]**

---

## E. Was fachlich stimmt (bitte beibehalten)

- Aufbau des Zählerschranks von unten nach oben: netzseitiger Anschlussraum mit Sammelschienen, SLS und PEN-Aufteilung, darüber der Zählerplatz mit BKE, oben der anlagenseitige Anschlussraum. Die PEN-Aufteilung im netzseitigen Anschlussraum ist nach VDE-AR-N 4100 zulässig **[W: EAM Netz, Kap. 4.2]**.
- PEN: grün-gelb mit blauer Markierung, mindestens 10 mm² Cu. Nach der Aufteilung N und PE nie wieder verbinden. Die Prüfung meldet es, wenn sie doch verbunden sind.
- FI: Summenstromwandler, L und N durch den FI, eigene N-Schiene je FI. L und N über verschiedene FIs wird als Fehler erkannt. Kurzschluss L–N löst den FI richtigerweise nicht aus (getestet). Bei Körperschluss ohne PE löst bis zur Berührung nichts aus (getestet). 30-mA-Pflicht nach VDE 0100-410 für Steckdosen bis 32 A und Beleuchtung in Wohnungen.
- LS: Prüfströme 1,13 und 1,45 × In, Kennlinie B mit 3–5 × In. Selektivität LS vor SLS vor NH 63 ist in der Simulation plausibel (getestet: „Der Hauptschalter davor bleibt drin“).
- Messgerät: 230 V und 400 V an den richtigen Punkten. Nach dem SLS liegt am SLS-Eingang weiter Spannung an (getestet).
- Schalter schaltet L, nicht N. Das wird geprüft.
- Abzweigdose: 5 Sicherheitsregeln, Prüfung nach DIN VDE 0100-600 vor und nach dem Einschalten, PE zuerst und etwas länger lassen, Reserve-Adern der Zuleitung mitmessen, Warnung bei zwei Einspeisungen. Fachlich gut. Diese Texte sind das Vorbild für den Hauptteil.
- Hinweise zu Überspannungsschutz (Pflicht seit DIN VDE 0100-443:2016-10, Typ 2 in der UV ab 10 m) **[W: DEHN DS273: „Der Schutzbereich eines SPDs beträgt 10 Meter.“]**.

---

## F. Testprotokoll (Auszug)

| Fall | Ergebnis App | Bewertung |
|---|---|---|
| Lichtkreis, Lampe L–N | 230 V | richtig |
| Lampe N–PE | 0 V, „direkt miteinander verbunden“ | Wert richtig, Text falsch (A3) |
| SLS aus, SLS-Eingang L1–N | 230 V | richtig, aber Texte sagen „ganzes Haus aus“ (A5) |
| Schalter aus, Lampe L–PE | „Kein Messwert“ | besser 0 V (C2) |
| N zur Lampe offen, Lampe N–PE | „Kein Messwert“ | **gefährlich falsch**, richtig wären 230 V (A2) |
| Körperschluss, PE da, FI 30 mA | nur FI löst aus | vertretbar, Text unvollständig (B1) |
| Körperschluss, PE da, kein FI | LS löst aus | richtig, Text „nur“ abwerten (B1) |
| Körperschluss ohne PE, FI 30 mA, Berühren | FI aus, „Du bist geschützt“ | Text überzogen (A1) |
| dasselbe mit FI 300 mA | FI aus, „Du bist geschützt“ | **gefährlich falsch** (A1) |
| Kurzschluss L–N | LS aus, FI bleibt ein, SLS bleibt ein | richtig, Begriff „N/PE“ ungenau (B7) |
| 5 kW an B16 (136 %) | Auslösung nach 8 s, „Sekunden bis Minuten“ | falsch (B3) |
| Herd ohne N | „Kein Strom“ | falsch (B2) |
| Geführter Aufbau beim Start | HAK, SLS, FI und LS ein, Klemmen unter Spannung | gefährliches Vorbild (A4) |

---

## Quellen

- DIN VDE 0100-410, -510, -540, -600, -701, -705, -443/-534. DIN VDE 0105-100. DIN EN 60898-1. VDE-AR-N 4100. DIN 18015-3. Wiedergabe aus Fachwissen und Tabellenbuch, Normtexte nicht eingesehen.
- EAM Netz: Aufbauhinweise für direkt messende Zähleranlagen (PEN-Aufteilung im NAR, SPD Typ 1 im NAR, „Empf. SH-Schalter E 35 A“): https://www.eam-netz.de/fileadmin/user_upload/Fuer_Zuhause/Hausanschluss/Anschlussbedingungen/Aufbauhinweise_fuer_direkt_messende_Zaehleranlagen_im_Netzgebiet_der_EAM_Netz_GmbH_final_1.pdf
- baunetzwissen, Elektrische Schutzbereiche im Bad: https://www.baunetzwissen.de/bad-und-sanitaer/fachwissen/elektroinstallationen/elektrische-schutzbereiche-im-bad-172830
- DEHN DS273, Neuerungen DIN VDE 0100-443/-534 (10 m Schutzbereich, 0,5 m Anschlusslänge): https://www.dehn.de/sites/default/files/media/files/Neuerungen-DIN-VDE-0100-443-534-DS273.pdf
- WEKA, VDE 0100-705 (RCD in allen Stromkreisen, TN-S, zusätzlicher Schutzpotentialausgleich, IP44): https://www.weka.de/elektrosicherheit/vde-0100-705-landwirtschaftliche-und-gartenbauliche-betriebsstaetten/
- § 13 NAV (Arbeiten hinter der Hausanschlusssicherung nur durch den Netzbetreiber oder einen eingetragenen Installateur): https://www.buzer.de/gesetz/7430/a146694.htm. Die Seite gesetze-im-internet.de war nicht erreichbar.
