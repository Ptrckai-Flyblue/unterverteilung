# Runde 2: Stellungnahme Elektriker (Geselle, Baustelle)

Grundlage: die Berichte `meister.md` (Elektromeister) und `lehrer.md` (Berufsschullehrer), mein eigener Bericht `elektriker.md` und Patricks Foto `/mnt/user-data/uploads/1fb1f1e0-image.png`. Das Foto habe ich in Ausschnitten vergrößert angesehen. Die Auflösung ist gering, deshalb kann ich keine Typenschilder lesen. Wo ich etwas nur aus Form und Lage schließe, steht das dabei.

Evidenz wie in Runde 1: **stark** / **mittel** / **schwach**.

---

## 1. Wo ich zustimme

Die drei Berichte decken sich in den wichtigsten Punkten. Damit ist die Reihenfolge für Patrick klar:

| Thema | Meister | Lehrer | ich | Kommentar aus der Praxis |
|---|---|---|---|---|
| LS ↔ Querschnitt wird nicht geprüft | A1 | (C7) | A1 | Einstimmig der wichtigste Fehler. Tabelle siehe Abschnitt 3.2. |
| „Du bist geschützt“ beim 300-mA-FI | A2 | A1 | – | Volle Zustimmung, das habe ich nicht getestet. Auf der Baustelle sagt man: Der 300er ist für den Stall gegen Brand, nicht für den Menschen. |
| Offener N zeigt „kein Messwert“ | A3 | A2 | – | Volle Zustimmung. Das ist *der* Unfall bei Lampenmontage: „Der blaue war doch abgeklemmt.“ |
| Haupterdungsschiene, Potentialausgleich und Erder fehlen | A4 | C10 | D12 / A3 (Stall) | Zustimmung. Im Foto sieht man die gn-ge-Anbindung im Zählerschrank (Abschnitt 3.1). |
| Hauptleitung, SLS E50/E63 an 5×10 | A5, C1 | C7 | B9 / D11 | Zustimmung, mit einer Einschränkung (Abschnitt 2, W1). |
| Stall VDE 0100-705 (Potentialausgleich, TN-S, IP54 feuergefährdet, FI) | B7 | B6 | A3–A5 | Einstimmig. Für Patricks Pferdestall Priorität 1. |
| AFDD-Hinweis für Heu/Stroh und Holzstall | B8 | – | – | Zustimmung als Hinweis. Bei einem Holzstall mit Heuboden würde ich ihn dem Kunden aktiv empfehlen. |
| Bad-Bereiche (Bereich 0 IPX7/SELV, Rasiersteckdose) | C5 | B4 | B7 | Einstimmig. Die Frage IPX4/IPX5 in Bereich 1 bleibt offen, alle drei Berichte sind dort unsicher. |
| Blau als Außenleiter: Schaltplan sagt Fehler, Dose sagt erlaubt | B10 | B5 | D3 | Einstimmig: Im Schaltplan `warn` statt `err`, Grün-gelb bleibt `err`. |
| SPD in der Doku „nicht vorhanden“, SPD nur als `hint` | B1, B2 | – | – | Zustimmung (Programmfehler bzw. falsche Stufe). |
| SPD im NAR: Funkenstrecke, 25 kA, plombierbar | B3 | – | B6 (unsicher) | Zustimmung. Passt zum Foto, der Ableiter sitzt im NAR auf der Schiene. |
| PEN auf PE-Schiene mit Brücke zur N gilt als Fehler | B5 | – | – | **Wichtig**, so baut man das real (siehe Foto). Muss als Aufteilung erkannt werden. |
| Arbeiten im vorgezählten Bereich nur eingetragener Installateur (§ 13 NAV) | B6 | A6 | – | Zustimmung. Patrick und sein Vater dürfen den NAR nicht anfassen. Das gehört deutlich in die App. |
| Hauptschalter „schaltet das ganze Haus ab“ | – | A5 | – | Zustimmung. Sammelschienen, SPD und SLS-Eingang bleiben unter Spannung. Darauf fallen auch Profis rein. |
| Geführter Aufbau verdrahtet unter Spannung | – | A4 | – | Zustimmung: mit allem auf AUS starten, Prüfschritt vor dem Einschalten. |
| LS schützt mit PE auch den Menschen (TN-Abschaltung) | – | B1 | – | Volle Zustimmung, fachlich zentral. |
| Spannungsfall und Abschaltbedingung | C2 | – | A6 | Einstimmig. Beim Stall in der Regel ausschlaggebend für den Querschnitt. |
| Prüfprotokoll VDE 0100-600 in der Doku | D1 | A4 | – | Zustimmung. Ohne Protokoll keine Fertigmeldung. |
| FI-Typ (A/F/B), FI-Überlast hinter großem SLS | B9 | D7 | B9 | Zustimmung. |
| Erfundener Wert „NH 63 A“ in der Doku | C3 | – | – | Zustimmung (Projektregel). |
| Herd ohne N, Überlastzeiten | – | B2, B3 | D15 | Zustimmung. |
| Spannung statt Strom bei L/S/K | – | C8 | – | Zustimmung. |

---

## 2. Wo ich widerspreche oder einschränke

**W1. Meister A5: Standard-Leitung des SLS auf „NYM-J 5×16“ ändern**
Ich stimme nur zur Hälfte zu. Die App mischt **zwei verschiedene Leitungen**:
- **Hauptleitung (vorgezählt):** HAK → netzseitiger Anschlussraum. Bei PEN-Aufteilung im NAR, wie auf dem Foto, ist sie 4-adrig (L1–L3, PEN). Praxis: NYM-J 4×16 bzw. NYY-J 4×16 oder 4×25. Für sie gilt die 63-A-Auslegung der 4100. Hier hat der Meister recht.
- **Zuleitung zur Unterverteilung (gezählt):** Abgangsklemme oben im AAR → UV, 5-adrig. Hinter einem SLS E35 ist NYM-J 5×10 der Normalfall auf der Baustelle (I_z 10 mm² B2, 3 belastete Adern ≈ 46 A ≥ 35 A).

Wird der Standard pauschal auf 5×16 geändert, plant ein Laie bei jeder UV 16 mm². Das kostet unnötig Geld und passt oft nicht mehr in die Klemmen eines 40-A-FI. **Vorschlag:** zwei Felder. `hak→ss5`: Hauptleitung 4×16. `hlak→uv`: UV-Zuleitung, Standard 5×10, geprüft gegen den SLS-Nennstrom. Evidenz: mittel (Praxis; die TAB kann mehr verlangen).

**W2. Meister A1: Verlegeart als frei wählbares Feld, Standard B2**
Fachlich richtig. Ein Laie weiß aber nicht, was A1, B2 oder C bedeutet. Und der Fall, der in der Praxis wirklich wehtut, fehlt: **Häufung**. Liegen in einem Kabelkanal oder Rohrbündel 4 bis 6 NYM 3×1,5 nebeneinander, reicht B16 rechnerisch nicht mehr (Faktor ≈ 0,65 → 16,5 A × 0,65 ≈ 10,7 A). Das betrifft fast jede Verteilung im Altbau und jeden Kabelkanal im Stall. **Vorschlag:** drei Laien-Optionen statt Buchstaben:
1. „auf der Wand / unter Putz“ (C)
2. „im Rohr, Kanal oder in Hohlwand“ (B2)
3. „in gedämmter Wand / Dämmung“ (A1)

Dazu ein Feld „wie viele Stromkreise liegen hier zusammen?“, das automatisch abmindert (Tabelle 3.2c). Evidenz: mittel.

**W3. Lehrer B6: „Baustahlmatte im Boden“ als Maßnahme beim Potentialausgleich im Stall**
Richtig für den Neubau. Patricks Stall ist aber vermutlich ein bestehender. Dort reißt niemand den Boden auf. In der Praxis geht man dann so vor: Alle Metallteile (Boxenwände, Fressgitter, Tränken, Wasserleitung) bekommen einen PA-Leiter. Wo neu betoniert wird (Stallgasse, Waschplatz), legt man eine Matte oder einen Bandstahl mit Anschlussfahne ein. Optional wird ein Edelstahl-Rundleiter in den Estrich geschlitzt. **Die App soll nicht so tun, als ginge es nur mit Matte.** Text: „Bei Neubau oder neuem Boden: Bewehrung bzw. Gitter anschließen. Im Bestand mindestens alle Metallteile verbinden; ob mehr nötig ist, entscheidet die Elektrofachkraft.“ Evidenz: mittel.

**W4. Querschnitt des zusätzlichen Potentialausgleichs**
In Runde 1 habe ich pauschal „mind. 4 mm²“ geschrieben. Genauer gilt nach meiner Kenntnis VDE 0100-540: zusätzlicher PA mindestens **2,5 mm² Cu mit mechanischem Schutz, 4 mm² ohne** (mittel, aus dem Gedächtnis). Im Stall nehme ich trotzdem **6 mm²** und Klemmen aus **Edelstahl (V4A)**. Ammoniak und Mist fressen verzinkte Schellen und Messing in wenigen Jahren auf. Die App soll 6 mm² und V4A vorschlagen. Ich korrigiere damit meinen eigenen Runde-1-Text.

**W5. Lehrer C5: Reihenfolge PE–N–L im geführten Aufbau „missverständlich“**
Inhaltlich richtig: PE zuerst an, zuletzt ab. Aber das betrifft nur die Reihenfolge im spannungsfreien Zustand. Wichtiger ist, dass im geführten Aufbau gar nicht unter Spannung gearbeitet wird (Lehrer A4). Ist A4 umgesetzt, ist C5 nur noch ein Hinweis. Keine echte Gegenposition, nur eine Priorisierung.

**W6. Lehrer D12: Feuchter Keller IP44 „nur Empfehlung“**
Da widerspreche ich. In der Praxis setzt man im feuchten Keller **immer** IP44-Geräte, und die Raumart „Keller (feucht)“ soll das als Regel behalten. Ein Laie entscheidet sonst „Empfehlung = brauche ich nicht“. Normativ mag die Mindestanforderung je nach Raum anders sein (unsicher, schwach). Für die App ist die strengere Regel aber sicher und billig.

**W7. Meister B4 / Lehrer D3: 3+0-Ableiter an der PE-Schiene**
Keine Gegenposition, aber das Foto bestätigt den Normalfall: Der Ableiter sitzt im NAR direkt auf demselben Schienensystem, auf dem der PEN aufgeteilt wird. Dort ist 3+0 die übliche Herstellerlösung. Die App soll das so zeichnen und **nicht** auf 3+1 umstellen. Evidenz: mittel.

**W8. Meister C7: Begriff „Hauptleitungsabzweigklemme“ im AAR**
Auf der Baustelle sagt jeder „Hauptleitungsabzweigklemme“ zu den weißen 5-poligen Klemmblöcken oben im Zählerschrank (siehe Foto). So heißen sie auch im Großhandelskatalog. Ich würde den Namen lassen und nur den Untertitel „Abgang zur Unterverteilung“ ergänzen. Evidenz: mittel.

---

## 3. Praxis-Sicht

### 3.1 Der Zählerschrank auf dem Foto

**Wichtig vorweg:** Das ist ein **neuer, noch nicht angeschlossener Schrank**. Die Leiter am Zählerplatz hängen frei mit Aderendhülsen bzw. Stiftkabelschuhen, und im Zählerfeld ist kein Zähler gesetzt. Es ist ein Ausstellungs- oder Lieferzustand nach VDE-AR-N 4100 mit zwei Feldern nebeneinander. Den Hersteller kann ich nicht sicher erkennen.

**Linkes Feld, von unten nach oben (= Zählerfeld):**

1. **Netzseitiger Anschlussraum (NAR), unten links:**
   - **5 waagerechte Kupfer-Sammelschienen** (40-mm-System) mit weißen Schienenhaltern. Die Hauptleitung vom HAK kommt von unten und wird an den Schienen angeschlossen (im Foto noch nicht vorhanden).
   - **SLS 3-polig**, links, über einen Sammelschienenadapter direkt auf die Schienen geklemmt, mit transparenter plombierbarer Abdeckung über den Anschlüssen. Die drei Abgänge oben gehen zum Zählerplatz.
   - **Überspannungsschutz Typ 1(+2)**, rechts daneben, ebenfalls über Adapter auf den Schienen. Man sieht ein grünes Statusfenster und einen angesetzten Modulteil darunter. Oben geht ein **grün-gelber Leiter** ab: der PE/PEN-Anschluss des Ableiters. Die Außenleiter bekommt er über den Adapter von den Schienen. Das ist die 3+0-Anordnung L1–L3 gegen PEN/PE direkt am Aufteilungspunkt. Funkenstreckentyp: lesen kann ich es nicht; laut 4100 im NAR Pflicht (Meister B3).
   - **PEN-Aufteilung:** Unten an der untersten Schiene sitzt eine Anschlussklemme mit einem **grün-gelben Leiter, der nach unten geht**. Ich lese das so: Unterste Schiene = PE/PEN. Der PEN der Hauptleitung landet dort, die N-Schiene ist mit der PE-Schiene gebrückt. Der gn-ge Leiter nach unten geht zur **Haupterdungsschiene** (Potentialausgleich, Fundamenterder). Die Brücke selbst erkenne ich in der Auflösung nicht. Evidenz: **schwach bis mittel**, aber das ist der übliche Aufbau. Für die App heißt das: Die PEN-Aufteilung passiert **auf den Sammelschienen im NAR**, nicht als eigenes Reiheneinbaugerät.
   - **1-poliger LS rechts auf der Schiene**, über einen Adapter mit Kupfer-Abgriff: Das ist sehr wahrscheinlich die **vorgezählte Absicherung für den Raum für Zusatzanwendungen (RfZ)**. Der Abgang (schwarzer Leiter) läuft nach rechts unten in das RfZ-Feld. Von dort werden Smart-Meter-Gateway, Steuerbox bzw. Kommunikationstechnik versorgt. Sie brauchen Strom, auch wenn der Kunde seinen SLS ausschaltet, und hängen deshalb vor dem Zähler. Evidenz: **mittel** (Lage und Leitungsführung passen; Beschriftung nicht lesbar).

2. **Zählerplatz (Mitte links):**
   - Leere Zählertafel mit Tragschiene und Befestigungsbügel. Von unten kommen **3 braune Leiter**: die Zählerzuleitung vom SLS (L1–L3 ein). Nach oben gehen **3 schwarze + 1 blauer Leiter** mit Aderendhülsen/Stiftkabelschuhen: Zählerabgang L1–L3 und N zum AAR.
   - Das sieht nach einem **Zählerplatz mit Dreipunktbefestigung (3.HZ)** zum Verschrauben aus, nicht nach einem gesteckten BKE-Adapter. Evidenz: **schwach**. Es kann auch ein Zählerplatz sein, auf den der BKE-Adapter erst noch gesetzt wird.
   - Rechts: ein **RJ45-Datenkabel (blau)** mit Kupplung. Das ist die Kommunikationsverbindung Zähler/Gateway → RfZ.

3. **Anlagenseitiger Anschlussraum (AAR, oben links):**
   - **Zwei 5-polige Hauptleitungsabzweigklemmen** (weiße Klemmblöcke) auf einer Tragschiene. Die linke ist belegt mit braun, schwarz, grau (von hinten), blau und grün-gelb: der normale Abgang zur Verteilung. Die rechte trägt die Aufschrift **„NOT“**: ein zweiter Abgang für **Ersatz- bzw. Notstrom**. Das passt zum rechten Feld (siehe unten).
   - Oben sitzt eine weitere Tragschiene (leer, Platz für SPD Typ 2 oder Steuergeräte) und eine Kabeleinführung.

**Rechtes Feld (Verteilerfeld) und RfZ:**

4. **Oben:** **N-Klemmen (blau) + PE-Klemme (grün-gelb)** als Schienenklemmenblock, darunter grün-gelbe Leiter. Das ist die N/PE-Verteilung des Verteilerfeldes. Darunter liegen leere Tragschienen für FI und LS (Reserve).

5. **Mitte:** keine normale Stromkreisverteilung, sondern sehr wahrscheinlich eine **Netz-/Ersatzstrom-Umschaltung**:
   - In der Mitte zwei große **4-polige Installationsschütze** nebeneinander, mutmaßlich gegeneinander verriegelt.
   - Links und rechts je ein **3-poliger Schutzschalter** (LS bzw. Leistungsschalter).
   - Oben **Überwachungsrelais** mit roten LEDs (Spannungs-/Phasenüberwachung) und ein Zeitrelais, dazu eine 5-polige Verteilerklemme.
   - Unten zwei **blaue N-Trennklemmen**.
   - Zusammen mit der „NOT“-Klemme heißt das: Bei Netzausfall schaltet die Anlage z. B. auf ein Aggregat oder einen Batteriespeicher um. Die N-Trennung ist dabei wichtig, damit keine Rückspeisung ins Netz geht und die Schutzmaßnahme im Inselbetrieb funktioniert.
   - Evidenz: **mittel** (Aufbau typisch, Typenschilder nicht lesbar).

6. **Unten rechts: Raum für Zusatzanwendungen (RfZ)** mit Lochplatte, Tragschiene, kleiner Verbindungsklemme (Wago), Kabelhalter und blauem Patchkabel. Hier kommen Smart-Meter-Gateway bzw. Steuerbox hinein, versorgt über den 1-poligen LS im NAR.

**Was die App davon übernehmen soll:**

| Punkt | Heute in der App | Vorschlag |
|---|---|---|
| PEN-Aufteilung | eigenes Reiheneinbaugerät „Aufteilung PEN“ auf der Schiene | Aufteilung **auf der 5-poligen Sammelschiene** darstellen: PEN-Klemme an der PE-Schiene, Brücke PE→N, dazu der gn-ge Abgang zur **Haupterdungsschiene** (neues Bauteil, Meister A4). Die Prüfung muss das als Aufteilung erkennen (Meister B5). |
| Hauptleitung | 5×10 am SLS | **4-adrig** HAK → NAR (L1–L3, PEN), getrennt von der 5-adrigen UV-Zuleitung (W1). |
| SPD im NAR | `spdss` 3+0, mit „L–PEN“ beschriftet, an PE angeschlossen | So lassen (passt zum Foto). Beschriftung: „Typ 1 (Funkenstrecke), L1–L3 gegen PEN/PE an der Aufteilung“. |
| **1-poliger LS auf der Sammelschiene** | fehlt | Neues Bauteil „Vorgezählte Absicherung RfZ (1-polig)“ auf `ss5`. Nimmt L von einer Schiene, versorgt den RfZ. Info: „Bleibt an, auch wenn der Hauptschalter aus ist.“ |
| RfZ / APZ | fehlt | Bauteil oder Feld „Raum für Zusatzanwendungen (Smart-Meter-Gateway, Steuerbox)“, nur als Info und Platzhalter, ohne Verdrahtungsübung. |
| Zählerplatz | „mit BKE-Adapter“ | Auswahl „eHZ mit BKE-Adapter“ / „Dreipunkt (3.HZ)“, Info: Zähler setzt der Messstellenbetreiber. |
| Abgang oben | eine `hlak` | Darf mehrfach vorkommen. Optional Beschriftung (z. B. „NOT“) und Hinweis „zweiter Abgang z. B. für Ersatzstrom“. |
| Zählerschrank mit Verteilerfeld | ZS und UV getrennt | Option „Verteilerfeld im Zählerschrank“ (häufig im Einfamilienhaus): UV-Hutschienen im rechten Feld mit eigener N/PE-Klemme oben. |
| Notstrom / Ersatzstrom | fehlt | Nur als Info-Bauteil mit Warnung: „Umschaltung nur mit allpoliger Trennung inkl. N, Planung und Anmeldung durch Fachbetrieb. Spannung kann auch bei abgeschaltetem Netz anliegen.“ (Zusammenhang mit Lehrer D9.) |
| Plombierung | nur beim HAK erwähnt | Plombierte Bereiche markieren (NAR, Zählerplatz). Hinweis „nur eingetragener Installateur“ (Meister B6, Lehrer A6). |

### 3.2 Leitungsschutz-Tabelle zum Einbauen

> **Faustwerte nach DIN VDE 0298-4, Tabelle 3** (Kupfer, PVC-Isolierung 70 °C, z. B. NYM/NYY, Umgebung 30 °C), Werte aus dem Gedächtnis. Sie decken sich mit den Werten im Meister-Bericht und sind vor dem Einbau einmal gegen die Norm zu prüfen. Evidenz: mittel.
> Größter LS ist der nächstkleinere Normwert ≤ I_z. Für Charakteristik B/C gilt dann auch die 1,45-Regel automatisch.
> Gilt **ohne Häufung**. Liegen mehrere Stromkreise zusammen, zuerst mit Tabelle c) abmindern.

**a) Belastbarkeit I_z in A**

| Querschnitt Cu | A1 (in Dämmung, im Rohr) 2 Adern | A1 3 Adern | B2 (Rohr, Kanal, Hohlwand) 2 Adern | B2 3 Adern | C (auf der Wand, unter Putz) 2 Adern | C 3 Adern |
|---|---|---|---|---|---|---|
| 1,5 mm² | 15,5 | 13,5 | 16,5 | 15 | 19,5 | 17,5 |
| 2,5 mm² | 19,5 | 18 | 23 | 20 | 27 | 24 |
| 4 mm² | 26 | 24 | 30 | 27 | 36 | 32 |
| 6 mm² | 34 | 31 | 38 | 34 | 46 | 41 |
| 10 mm² | 46 | 42 | 52 | 46 | 63 | 57 |
| 16 mm² | 61 | 56 | 69 | 62 | 85 | 76 |

„2 Adern“ = Wechselstromkreis (L + N belastet), „3 Adern“ = Drehstrom (L1–L3).

**b) Daraus: größter LS (Normreihe 6/10/13/16/20/25/32/40/50/63 A)**

| Leitung | A1 | B2 | C | Praxis-Standard Wohnhaus | Praxis-Standard Stall (im Rohr/Kanal = B2) |
|---|---|---|---|---|---|
| NYM-J 3×1,5 | **B13** | B16 | B16 | Licht B10, Steckdosen B16 | B16 (B13 bei Häufung) |
| NYM-J 3×2,5 | B16 | B20 | B25 | Steckdose **max B16** (Schuko), sonst B20 | B16/B20 |
| NYM-J 5×1,5 | **3×B13** | **3×B13** | 3×B16 | selten, 3×B13 | 3×B13 |
| NYM-J 5×2,5 | 3×B16 | 3×B20 | 3×B20 | Herd 3×B16 | 3×B16 |
| NYM-J 5×4 | 3×B20 | 3×B25 | 3×B32 | Durchlauferhitzer, Wallbox 11 kW: 3×B20/B25 | 3×B25 |
| NYM-J 5×6 | 3×B25 | 3×B32 | 3×B40 | Unterverteilung klein / Wallbox 22 kW: 3×B32 | 3×B32 |
| NYM-J 5×10 | 3×B40 / E35 | 3×B40 / E35–E40 | 3×B50 | UV-Zuleitung hinter SLS E35 | Zuleitung Stall-UV |
| NYM-J 5×16 | 3×B50 | 3×B50 | 3×B63 | Hauptleitung (4×16), SLS bis E50 (E63 nur bei C) | – |

Die App soll außerdem diese Regeln fest einbauen:
- **Schuko-Steckdose:** LS höchstens 16 A, unabhängig vom Querschnitt.
- **Leitung im Erdreich:** nur NYY (bzw. NAYY), I_z nach eigener Erdtabelle. Diese kenne ich nicht sicher, deshalb verweise ich auf die Fachkraft.

**c) Abminderung bei Häufung (VDE 0298-4, Tabelle 21, gebündelt im Rohr, Kanal oder auf der Wand, Gedächtnis)**

| Stromkreise zusammen | 1 | 2 | 3 | 4 | 5 | 6 | 9 |
|---|---|---|---|---|---|---|---|
| Faktor | 1,0 | 0,80 | 0,70 | 0,65 | 0,60 | 0,57 | 0,50 |

Praxisbeispiel für die App: 4 NYM 3×1,5 in einem Kanal (B2) → 16,5 × 0,65 ≈ 10,7 A → **höchstens B10**. Die App soll hier warnen: „Mehrere Leitungen im Kanal: 1,5 mm² nur noch B10, oder 2,5 mm² verlegen.“

**d) Umgebungstemperatur:** Dachboden oder Stall im Sommer mit 35 °C × 0,94, mit 40 °C × 0,87. Das ist für Patricks Stall nur relevant, wenn Leitungen unter einem Blechdach liegen. Evidenz: mittel.

**e) Spannungsfall (3 % ab Zähler, Faustformel)**, maximale einfache Länge in m bei voller LS-Last:
- Wechselstrom: L_max = 193 × A / I_n
- Drehstrom: L_max = 388 × A / I_n

| Leitung / LS | max. Länge | Hinweis |
|---|---|---|
| 3×1,5 / B10 | 29 m | |
| 3×1,5 / B16 | **18 m** | |
| 3×2,5 / B16 | 30 m | |
| 3×2,5 / B20 | 24 m | |
| 5×2,5 / 3×B16 | 61 m | |
| 5×4 / 3×B25 | 62 m | |
| 5×6 / 3×B32 | 73 m | |
| 5×10 / 3×B40 | 97 m | Zuleitung Stall-UV |

Die Strecke Zähler → UV zieht von diesen 3 % schon etwas ab. Bei einem Stall 40–80 m vom Haus heißt das praktisch: **eigene UV im Stall mit NYY-J 5×10 (bzw. 5×6 bei wenig Last)**, von dort kurze 1,5/2,5er-Kreise.

**JS-Vorschlag (Datenteil):**
```js
// Faustwerte DIN VDE 0298-4 Tab. 3, Cu/PVC 30 °C. Index: [A1, B2, C]
const IZ = {
  '1,5': {2:[15.5,16.5,19.5], 3:[13.5,15,17.5]},
  '2,5': {2:[19.5,23,27],     3:[18,20,24]},
  '4':   {2:[26,30,36],       3:[24,27,32]},
  '6':   {2:[34,38,46],       3:[31,34,41]},
  '10':  {2:[46,52,63],       3:[42,46,57]},
  '16':  {2:[61,69,85],       3:[56,62,76]},
};
const HAEUF = [1,1,0.8,0.7,0.65,0.6,0.57,0.54,0.52,0.5]; // Index = Anzahl Stromkreise
const LS_REIHE = [6,10,13,16,20,25,32,40,50,63];
// maxLS(qs, adern, verlegeart, n) = größter Wert aus LS_REIHE ≤ IZ[qs][adern][va] * HAEUF[min(n,9)]
```

### 3.3 Materialposten mit Mengenregeln (automatisch ergänzen)

Alle Mengen als **„Schätzung, aufgerundet auf Verpackungseinheit“** kennzeichnen. Die Grundlage ist die korrigierte Kabelmeterzahl aus Runde 1, B2: waagerechter Weg + senkrechter Stich je Gerät + Reserve (0,5–1 m UV, 0,15–0,2 m je Dosenende) + 10 % Verschnitt. Bei NYM wird auf Ring 50 m bzw. 100 m aufgerundet.

| Posten | Mengenregel | Wann | Evidenz |
|---|---|---|---|
| **Leitungsschellen** (Kunststoff-Einzelschelle bzw. Nagelschelle passend zum Ø) | Aufputz-NYM: 1 je **0,3 m** + 1 je Geräte-/Dosen-Einführung (≤ 10 cm davor) + 1 je Richtungswechsel (≈ +10 %) | Aufputz (Stall, Keller, Garage) | schwach–mittel (Praxis; Herstellerangaben schwanken 0,25–0,4 m) |
| **Installationsrohr starr M20/M25** (Stangen 3 m) | Leitungsmeter im Stall/Außen × 1,1, auf 3-m-Stangen aufrunden. Dazu **Muffen** 1 je Stange, **Bögen** 1 je Ecke, **Rohrschellen** 1 je **0,6 m** (Klemmschellen M20/M25) | Stall (Tiere, Nager), mechanisch gefährdet | mittel |
| **Kabelkanal** | Grundriss-Meter (vorhanden), dazu Endstücke 2 je Strang, Ecken/T-Stücke je Knick, Befestigung 2 Dübel je **0,5 m** | wenn Kanal gezeichnet | mittel |
| **Dübel + Schrauben** (6 mm, Holz/Beton passend) | 1 je Schelle (nicht bei Nagelschellen) + 2 je Abzweigdose + 2 je Aufputz-Gerät + 4 je Verteiler + Kanal/Rohr wie oben | immer, im Stall rostfrei | mittel |
| **UP-Gerätedosen** | 1 je Schalter, Steckdose oder Herdanschluss; 2 bzw. Doppeldose je Doppelsteckdose/Kombination | nur Raumarten ohne IP-Regel (Wohnräume) | stark |
| **UP-Abzweigdosen** (Ø 70 mm, tief) | 1 je Raum ohne Dosen-Bauteil, sonst je `adose` | Wohnräume | mittel |
| **Kabelverschraubungen / Membrantüllen** | 1 je Leitungseinführung in ein Aufputz-Gehäuse ohne Membran (AP-Verteiler, Stall-UV, Geräte mit Gewindebohrung). Größe nach Leitungs-Ø: NYM 3×1,5…5×2,5 → **M20**; 5×4/5×6 → **M25**; 5×10 → **M32**; 5×16 → **M32/M40** (Klemmbereich im Datenblatt prüfen). Je Verschraubung eine **Gegenmutter**, wenn das Gehäuse kein Gewinde hat | AP-Verteiler, Feuchtraum | mittel (die Zuordnung Ø → M-Größe ist Faustwert) |
| **Aderendhülsen** (isoliert, Farbe nach Querschnitt) | 1 je Ende einer **feindrähtigen** Ader (H07V-K, H05VV-F), also 2 je Verdrahtung im Verteiler. Nicht für eindrähtiges NYM. +20 % Ausschuss, auf 100 bzw. 500 aufrunden. Größen: 1,5 / 2,5 / 10 / 16 mm² je nach Verbindung | Verteiler | stark (Pflicht bei feindrähtig in Schraubklemmen, sofern die Klemme nichts anderes zulässt) |
| **Verdrahtung H07V-K** | 0,4 m je Verbindung im Verteiler, je Farbe und Querschnitt: FI-Einspeisung 10 mm², LS-Abgänge = Leitungsquerschnitt, N/PE-Brücken 10/16 mm² | Verteiler | mittel |
| **Hutschienen-Zubehör** | Blindabdeckstreifen = freie TE je Reihe; Phasenschienen-Endkappen 2 je Schiene; Berührungsschutz = freie Pins; Endhalter 2 je Reihe (wenn kein Schienensystem) | Verteiler | stark |
| **Haupterdungsschiene (HES)** | 1 je Gebäude | wenn HAK im Projekt | stark |
| **Schutzpotentialausgleichsleiter** H07V-U/-K gn-ge | **6 mm²** zu Wasser, Heizung und Gas (je Anschluss Länge aus dem Plan, sonst 3 m Schätzung); **16 mm²** zum Fundamenterder bzw. SPD-Erdung Typ 1 | Hausanschluss | mittel (Querschnitte aus dem Gedächtnis, deckt sich mit Meister A4) |
| **Rohrerdungsschellen / Bandschellen** | 1 je metallenem Rohr an der HES (Wasser, Heizung VL/RL, Gas) | Hausanschluss | stark |
| **Stall: Potentialausgleichsschiene** | 1 je Stallgebäude | Raumart Stall | stark (705) |
| **Stall: PA-Leiter** gn-ge **6 mm²**, geschützt im Rohr | Summe Abstand Schiene → Metallteil; ohne Planangabe 1 m je Box + 5 m | Raumart Stall | mittel |
| **Stall: PA-Klemmen Edelstahl (V4A)** | 1 je Metallteil: je Box Boxenwand/Gitter 1, Tränke 1, Fressgitter 1, Wasserleitung 1 | Raumart Stall | mittel (Korrosion durch Ammoniak, Praxis) |
| **Stall: Anschlussfahne Bewehrung** (Rund-/Bandstahl, V4A-Klemme) | 1 je neu betonierter Fläche | Stall, wenn „neuer Boden“ angehakt | mittel |
| **Kabelbinder** | 2 je Leitung im Verteiler + 1 je 0,3 m im Kanal-Bündel | Verteiler, Kanal | schwach |
| **Beschriftung** | 1 Etikett je Stromkreis (LS), 1 je Abzweigdose (Deckel), 1 Stromkreisverzeichnis je Verteiler | immer | stark |
| **Isolierband** schwarz/braun | 1 Rolle, wenn markierte blaue Adern vorkommen (heute nur in der Dose) | Dose/Schalterleitung | stark |
| **Hebelklemmen** | aus dem Dosenmodell (vorhanden). Ohne Dosenmodell: je Abzweigdose 1× 5-fach PE, 1× 5-fach N, 2× 3-fach L/S als Schätzung | Wohnräume ohne Dosenmodell | schwach |
| **Gips/Schnellputz** (Unterputz) | 1 kg je 5 m Schlitz + 0,2 kg je Dose | Wohnräume, UP | schwach (reine Praxis-Schätzung) |
| **Erdkabel-Zubehör** | bei NYY im Erdreich: Trassenwarnband = Länge; Sand nach Bedarf; Schutzrohr (KG/Kabelschutzrohr) an Hauseinführung + mechanisch gefährdeten Stellen | Zuleitung Stall-UV im Erdreich | mittel |

---

## Kurzfassung für den Koordinator

- **Einig:** LS ↔ Querschnitt, 300-mA-„geschützt“, offener N, HES/Potentialausgleich, Stall-705, NAV-Hinweis, Prüfprotokoll, SPD-Doku.
- **Meine Einwände:** Hauptleitung (4×16) und UV-Zuleitung (5×10) trennen statt pauschal 5×16; Verlegeart für Laien in Alltagsworten und **Häufung** einrechnen (B10 bei 4 NYM im Kanal); PA im Stall-Bestand ohne Pflicht-Baustahlmatte, aber 6 mm² + V4A; feuchter Keller IP44 als Regel beibehalten.
- **Foto:** neuer 4100-Zählerschrank, zwei Felder. Im NAR: 5-polige Schienen, SLS und Typ-1-Ableiter (3+0) auf Adaptern, PEN-Aufteilung auf den Schienen mit gn-ge zur HES, 1-poliger LS = vorgezählte Versorgung RfZ. Dazu Zählerplatz, oben zwei Abgangsklemmen (eine „NOT“), rechts Netz-/Ersatzstrom-Umschaltung und unten RfZ.
