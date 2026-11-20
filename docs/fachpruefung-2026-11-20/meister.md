# Prüfbericht Elektromeister – „Unterverteilung – Baukasten“

Geprüft: `/home/claude/unterverteilung/index.html` (2870 Zeilen, Stand Commit `efe3eb9`), nicht verändert.
Vorgehen: Quelltext gelesen (TYPES, INFO, check(), roomRules(), materials(), renderPaper(), touch(), volt(), GSTEPS, Dosen-Texte), App mit Playwright/Chrome gestartet (exampleHouse(), lichtkreis(true), openDoc()) und gezielt Fehlerfälle durchgespielt. Netzform TN-C-S.

Maßstab: DIN VDE 0100-410/-430/-443/-534/-540/-600/-701/-705, DIN VDE 0298-4, VDE-AR-N 4100, DIN 18015-1, NAV. Normtexte lagen mir nicht im Original vor. Belegt habe ich über Netzbetreiber-, Hersteller- und Fachpresse-Quellen (Links unten). Wo ich aus dem Gedächtnis zitiere, steht das dabei.

Evidenz-Kennzeichnung: **stark** = Normtext/Gesetz eindeutig bzw. mehrere unabhängige Quellen; **mittel** = eine Fach- oder Herstellerquelle oder sicheres Fachwissen ohne Originaltext; **schwach** = Auslegung bzw. ich bin unsicher.

Testergebnisse (Playwright, `check()` je Szenario auf Basis von `exampleHouse()`):

| Szenario | Was die App meldet |
|---|---|
| LS „Licht“ auf B20, Leitung NYM-J 3×1,5 | **nichts** |
| LS3 „Herd“ auf 3×B32, Leitung NYM-J 5×2,5 | **nichts** |
| Herd 3×B16 an NYM-J 5×1,5 | **nichts** |
| SLS E63 mit Hauptleitung NYM-J 5×10 und FI 40 A | **nichts** |
| Steckdosenkreis 120 m lang, 1,5 mm², B16 | **nichts** |
| Überspannungsschutz gelöscht | nur „hint“, obwohl im Text „Pflicht“ steht |
| Nur 300-mA-FI, Gehäuse ohne PE berührt | Meldung „Du bist geschützt.“ |
| N an der Lampe abgeklemmt, Schalter ein, Messung Lampe-N gegen PE | „– – – Kein Messwert … keine Verbindung zum Netz“ |
| Beispiel mit Sammelschienen-SPD (spdss), Kundendoku | „Überspannungsschutz: nicht vorhanden“ |
| PEN klassisch auf PE-Schiene mit Brücke zur N-Schiene | Fehler „N und PE nach der Aufteilung wieder verbunden“ |

---

## A. GEFÄHRLICH falsch (kann zu Brand oder Stromschlag führen)

### A1. Keine Prüfung LS-Nennstrom ↔ Leitungsquerschnitt/Verlegeart → Brandgefahr
- **Fundstelle:** `check()` Z. 657–700, nur Z. 676 prüft „Steckdose > 16 A“. Auswahl `ls1.ratings [6,10,13,16,20]` (Z. 443), `ls3.ratings [16,20,25,32]` (Z. 447), `rcbo.ratings` bis 20 (Z. 451), Leitungsfeld `cable` (CABLES Z. 1209).
- **Was die App zeigt:** B20 an 3×1,5 (Licht), 3×B32 an 5×2,5 (Herd) oder 3×B16 an 5×1,5 laufen ohne Meldung durch. Die Kundendoku zeigt dazu „Keine Fehler im Plan“.
- **Korrekt:** Es muss gelten I_B ≤ I_n ≤ I_z (VDE 0100-430, Abschnitt 433.1). I_z kommt aus VDE 0298-4, Tabelle 3 (Cu/PVC, 30 °C, Werte aus dem Gedächtnis, im Normtext gegenprüfen):
  - 1,5 mm²: 2 belastete Adern A1 15,5 / B2 16,5 / B1 17,5 / C 19,5 A; 3 belastete Adern A1 13,5 / B2 15 / B1 15,5 / C 17,5 A.
  - 2,5 mm²: 2 Adern A1 19,5 / B2 23 / B1 24 / C 27 A; 3 Adern A1 18 / B2 20 / B1 21 / C 24 A.
  - Häufung und Umgebungstemperatur senken die Werte weiter.
  - Damit sind B20 an 1,5 mm² und B25/B32 an 2,5 mm² in **keiner** Verlegeart zulässig. 3×B16 an 5×1,5 geht nur in Verlegeart C ohne Häufung.
- **Evidenz:** stark (Grundregel 430). Tabellenwerte mittel (Gedächtnis; eine Webquelle mit ähnlichen Werten, aber unsauberer Bezugstemperatur).
- **Korrektur (Logik):** Tabelle `IZ = {'1,5':{2:[15.5,16.5,17.5,19.5],3:[13.5,15,15.5,17.5]}, '2,5':{…}, '4':…, '6':…, '10':…, '16':…}` für A1/B2/B1/C anlegen. Ein Feld „Verlegeart“ je LS einführen, Standard **B2** (konservativ für Hohlwand/Rohr). Ob 2 oder 3 Adern belastet sind, ergibt sich aus 1-polig oder 3-polig.
  - `err`, wenn `rating > Iz(querschnitt, verlegeart, adern)`.
  - `warn`, wenn `rating > Iz` bei Verlegeart C, also immer unzulässig.
  - Zusatzhinweis „Häufung: Abminderung nach VDE 0298-4 Tab. 21 beachten“.
- **Korrektur (Text Z. 676):** Die Meldung nennt „Bei 1,5 mm²“, auch wenn 2,5 mm² gewählt ist. Neu: „Schutzkontaktsteckdosen sind für 16 A gebaut. Der LS darf deshalb höchstens 16 A haben, unabhängig vom Querschnitt.“

### A2. „Gehäuse berühren“: „Du bist geschützt“ auch bei 300-mA-FI
- **Fundstelle:** `touch()` Z. 826–828. Gewählt wird *irgendein* FI (`sort((a,b)=>a.idn-b.idn)[0]`), auch einer mit 300 mA.
- **Was die App zeigt (getestet):** Lampe ohne PE, nur 300-mA-FI, Gehäuse berührt. Antwort: „Der FI hat das in ca. 20–40 ms erkannt. Du bist geschützt.“
- **Korrekt:** Nur ein FI mit IΔn ≤ 30 mA gilt als zusätzlicher Schutz bei direktem Berühren (VDE 0100-410, Abschnitt 415.1). 300 mA ist ein Brandschutz-FI. Ein Körperstrom unterhalb von 300 mA löst ihn nicht aus, und dieser Bereich ist lebensgefährlich (Herzkammerflimmern ab etwa 50 mA). Auch der 30-mA-FI verhindert den Schlag nicht, er begrenzt nur die Einwirkdauer.
- **Evidenz:** stark.
- **Korrektur:**
  - `if(f.idn<=30)`: „FI hat ausgelöst. Du hast trotzdem einen Schlag bekommen – der FI begrenzt nur die Dauer. Er ersetzt keinen Schutzleiter: PE anschließen!“
  - `else`: Toast „Lebensgefahr: Der 300-mA-FI ist für Brandschutz, nicht für Personenschutz. Der Strom durch den Körper reicht nicht zum Auslösen, kann aber tödlich sein.“, Stufe `danger`, FI nicht auslösen.

### A3. Spannungsprüfer: unterbrochener N hinter eingeschaltetem Verbraucher zeigt „kein Messwert / keine Verbindung“
- **Fundstelle:** `volt()` Z. 1185–1191, Anzeige in `showMeter()` Z. 1197.
- **Was die App zeigt (getestet):** Im Lichtkreis wird der N der Lampe abgeklemmt, der Schalter ist ein. Die Messung Lampe-N gegen PE ergibt „– – –, Kein Messwert: Mindestens ein Punkt hat keine Verbindung zum Netz (Leiter offen oder abgeschaltet).“
- **Korrekt:** Über den Verbraucher (Glühwendel, LED-Treiber) liegt die offene N-Ader auf Außenleiterpotential, also 230 V gegen PE. Das ist einer der klassischen Unfälle: „Ich habe doch den N abgeklemmt.“ Ein Laie liest „keine Verbindung zum Netz“ als „spannungsfrei“.
- **Evidenz:** stark (Physik/Grundlagen).
- **Korrektur (Logik):** In `buildNets` Verbraucher-Lasten als hochohmige Kante L↔N für die *Potentialbestimmung* mitführen. Ein Netz ohne Quelle, das über eine Last an einer Phase hängt, gilt dann als „über Verbraucher an L1“ und zeigt 230 V.
- **Korrektur (Text, mindestens):** Statt „Kein Messwert“ schreiben: „Kein fester Bezug. Achtung: Ein offener Leiter kann über einen eingeschalteten Verbraucher trotzdem volle Spannung führen. Spannungsfreiheit nur allpolig mit zweipoligem Spannungsprüfer feststellen (5 Sicherheitsregeln).“

### A4. Haupterdungsschiene, Schutzpotentialausgleich und Erder fehlen völlig – Doku sagt „Keine Fehler im Plan“
- **Fundstelle:** Kein Bauteil in TYPES, kein Eintrag in MAT (Z. 1224 ff.), keine Regel in `check()`. Erwähnt wird das Thema nur in `INFO.pek` (Z. 544, „Verbunden mit dem Hauptpotentialausgleich …“).
- **Was die App lehrt:** HAK → PEN-Aufteilung → Verteilung → Verbraucher wird als vollständig und fehlerfrei bewertet.
- **Korrekt:**
  - Schutzpotentialausgleich über die Haupterdungsschiene (HES) ist Teil der Schutzmaßnahme „automatische Abschaltung“ und Pflicht (VDE 0100-410, Abschnitt 411.3.1.2).
  - Einzubinden sind PE/PEN-Aufteilung, Fundament- bzw. Ringerder (DIN 18014), metallene Wasser-, Gas- und Heizungsleitungen, Blitzschutz und Antennenerdung.
  - Schutzpotentialausgleichsleiter mindestens 6 mm² Cu (VDE 0100-540, Abschnitt 544.1.1, aus dem Gedächtnis). SPD-Erdung bei Typ 1 mindestens 16 mm² Cu (DEHN).
  - Im TN-C-S-Netz ist das bei einem PEN-Bruch im Netz die entscheidende Maßnahme gegen gefährliche Berührungsspannung an allen PE-Teilen.
  - Bei PEN-Aufteilung an der Übergabestelle verlangt der Netzbetreiber zusätzlich einen eigenen Schutzpotentialausgleichsleiter zur HES (EAM Netz).
- **Evidenz:** stark (Pflicht nach 410). Querschnitte mittel.
- **Korrektur:**
  1. Bauteil „Haupterdungsschiene (HES)“ (cat 0) mit Anschlüssen PE/PEN, Erder, Wasser, Gas, Heizung, Blitzschutz.
  2. `check()`: `err`, wenn HAK vorhanden, aber keine HES mit dem PE/PEN-Punkt verbunden ist („Schutzpotentialausgleich fehlt – VDE 0100-410 411.3.1.2“). `hint` für jeden freien HES-Anschluss („Wasserleitung metallisch? Dann anschließen“).
  3. MAT: „Potentialausgleichsschiene“, „H07V-K 1×6 bzw. 1×16 gn-ge (m)“, „Erdungsband/Fundamenterder-Anschlussfahne (DIN 18014)“.
  4. Kundendoku Abschnitt 1: Zeile „Schutzpotentialausgleich / Erder: Erdungswiderstand ___ Ω, angeschlossene Teile ___“.

### A5. Hauptleitung: Vorgabe NYM-J 5×10 ohne Prüfung; bei SLS E50/E63 nicht überlastgeschützt
- **Fundstelle:** `sls.def.cable:'NYM-J 5×10'` (Z. 408), `ratings [35,50,63]`. Getestet: SLS E63 mit 5×10 ergibt keine Meldung.
- **Korrekt:**
  - 10 mm² Cu bei 3 belasteten Adern hat I_z ≈ 42 A (A1) / 46 A (B2) / 50 A (B1) / 57 A (C) (Gedächtnis, 0298-4). Ein SLS E63 schützt 10 mm² nicht gegen Überlast, E50 nur in B1/C.
  - Nach VDE-AR-N 4100 muss die Hauptleitung zudem für mindestens 63 A ausgelegt sein (Baunetz-Zusammenfassung). Mit 10 mm² NYM ist das bei üblicher Verlegung nicht erfüllt. Praxis ist 16 mm².
  - Zulässiger Spannungsfall im ungezählten Bereich: 0,5 % (NAV/4100).
- **Evidenz:** mittel (63-A-Vorgabe aus Fachportal, I_z aus dem Gedächtnis). **Unsicher:** ob 10 mm² bei kurzer Leitung und SLS E35 netzbetreiberseitig toleriert wird. Das entscheidet die TAB.
- **Korrektur:**
  - Standard auf `NYM-J 5×16` bzw. bei TN-C-Hauptleitung `NYM-J 4×16` / `NYY-J 4×16` setzen (siehe C1).
  - `check()`: `warn` „Hauptleitung muss nach VDE-AR-N 4100 für mind. 63 A ausgelegt sein; 10 mm² reicht dafür i. d. R. nicht – mit Netzbetreiber/TAB klären“.
  - `err`, wenn SLS-Nennstrom > I_z der gewählten Hauptleitung.

---

## B. Fachlich falsch

### B1. Kundendoku: Überspannungsschutz als „nicht vorhanden“ ausgewiesen, Typ falsch
- **Fundstelle:** `renderPaper()` Z. 1281: `S.items.some(d=>d.type==='spd')?'Typ 1+2 vorhanden':'nicht vorhanden'`.
- **Was die App zeigt (getestet):** Beispiel „ganzes Haus“ mit Sammelschienen-SPD. In der Doku steht „Überspannungsschutz: nicht vorhanden“. Ein `spd` mit `spdt='2'` erscheint als „Typ 1+2 vorhanden“. Das ist eine falsche Angabe zu einer Schutzmaßnahme in einem Kundendokument.
- **Korrektur:** `const sp=S.items.filter(d=>d.type==='spd'||d.type==='spdss')`. Ausgabe je Gerät: `spdss` → „Typ 1+2 (Kombiableiter, Sammelschiene, Vorzählerbereich)“, `spd` → „Typ ${d.spdt==='2'?'2':'1+2'} in ${Gehäusename}“. Evidenz: stark (Programmfehler).

### B2. Überspannungsschutz-Pflicht nur als „Hinweis“
- **Fundstelle:** `check()` Z. 691: `add('hint','Kein Überspannungsschutz. Pflicht für Neuanlagen …')`.
- **Korrekt:** Seit DIN VDE 0100-443:2016-10 (Übergangsfrist bis 14.12.2018) ist der Schutz für die Energieversorgung im Wohngebäude verpflichtend. Mindestens Typ 2 am oder nahe dem Einspeisepunkt, nicht im ungezählten Bereich. Typ 1 ist nötig bei äußerem Blitzschutz oder Freileitungseinspeisung (DEHN DS273).
- **Evidenz:** mittel–stark (Herstellerzusammenfassung der Norm, deckt sich mit Fachpresse).
- **Korrektur:** Stufe auf `warn`. Text: „Kein Überspannungsschutz. Für Neuanlagen und Erweiterungen Pflicht (DIN VDE 0100-443/-534): mindestens Typ 2 im Zählerschrank nach dem Zähler; bei Blitzschutzanlage oder Freileitung Typ 1.“

### B3. Sammelschienen-SPD Typ 1+2: Vorgaben der VDE-AR-N 4100 für den Vorzählerbereich fehlen
- **Fundstelle:** `INFO.spdss` Z. 533, `MAT.spdss` Z. 1224.
- **Korrekt:** Im netzseitigen Anschlussraum sind nur **spannungsschaltende SPD Typ 1 mit Funkenstrecke** zulässig, keine Varistoren. Weitere Vorgaben: Kurzschlussfestigkeit ≥ 25 kA, kein Folgestrom, der die Hausanschlusssicherung auslöst, Statusanzeige ohne Betriebsstrom, plombierbar. Im HAK sind SPD nicht mehr erlaubt (DEHN-Sonderdruck zur 4100).
- **Evidenz:** mittel–stark (Hersteller zitiert die Anwendungsregel).
- **Korrektur:** In `INFO.spdss.t` ergänzen: „Nur funkenstreckenbasierte Typ-1-Ableiter (kein Varistor), Kurzschlussfestigkeit ≥ 25 kA, plombierbar – Vorgabe VDE-AR-N 4100 für den ungezählten Bereich.“ MAT-Text: „Kombiableiter Typ 1+2, funkenstreckenbasiert, für 40-mm-Sammelschiene, 3+0 (TN-C), I_SCCR ≥ 25 kA, plombierbar“.

### B4. 3+0-Schaltung „L–PEN“ sitzt im Modell nach der PEN-Aufteilung auf der PE-Schiene
- **Fundstelle:** `spdss.busTap ['L1','L2','L3','PE']` (Z. 424), Beschriftung „3+0 · L–PEN“, `INFO.spdss` „im TN-C-Teil vor der PEN-Aufteilung“. Die App teilt den PEN aber vorher über das Bauteil `pen` auf N- und PE-Schiene auf.
- **Korrekt:** 3+0 (Schema 1: jeder Außenleiter gegen PEN/PE) ist richtig, wenn der Ableiter am PEN bzw. unmittelbar am Aufteilungspunkt sitzt. Sitzt er im TN-S-Teil mit Abstand, braucht es auch einen Schutzpfad N–PE (4+0 oder 3+1). Hersteller bieten deshalb getrennte TNC- und TNS-Varianten an (DEHN WPX001). Die 0,5-m-Regel betrifft die Anschlussleitungslänge (a+b+c).
- **Evidenz:** mittel. **Unsicher:** Ob eine Aufteilung direkt auf dem 5-poligen Schienensystem normativ als „am PEN“ gilt, konnte ich ohne Normtext nicht abschließend belegen.
- **Korrektur (Text):** „3+0 ist nur zulässig, wenn der PEN direkt auf diesem Schienensystem aufgeteilt wird (PE- und N-Schiene unmittelbar gebrückt). Sonst TNS-Ausführung (4+0/3+1) wählen.“

### B5. Falscher Fehler bei fachgerechter PEN-Aufteilung über Schienenbrücke
- **Fundstelle:** `check()` Z. 661–663, Prüfung mit `excludePen`, die nur das App-Bauteil `pen` kennt.
- **Was die App zeigt (getestet):** Ein Elektriker legt den PEN wie üblich auf die PE-Schiene und setzt eine Brücke PE→N. Ergebnis: „err: N und PE sind nach der Aufteilung wieder miteinander verbunden“, dazu Farbfehler am PEN.
- **Korrekt:** Das *ist* die Aufteilung. Für die Kundendoku eines realen Schranks ist die Meldung irreführend.
- **Korrektur:** Eine Verbindung N↔PE, die direkt mit dem HAK-PEN verbunden ist, als Aufteilung werten. Nur Verbindungen *hinter* einem FI, einer N-Klemme oder in der UV melden. Alternativ als `hint`: „Aufteilungspunkt erkannt – nach diesem Punkt N und PE nie wieder verbinden.“

### B6. Netzseitiger Anschlussraum: Arbeiten nur durch eingetragenen Installateur – fehlt bei Zählerschrank/SLS/SPD
- **Fundstelle:** `INFO.zs` Z. 529, `INFO.ss5` Z. 534, `INFO.spdss` Z. 533. Nur `INFO.hak` nennt den eingetragenen Installateur.
- **Korrekt:** Arbeiten an der Kundenanlage mit Ausnahme der Messeinrichtungen dürfen nur der Netzbetreiber oder ein im Installateurverzeichnis eingetragenes Unternehmen ausführen (§ 13 Abs. 2 NAV). Der Vorzählerbereich ist plombiert, Inbetriebsetzung und Fertigmeldung laufen über den eingetragenen Betrieb.
- **Evidenz:** stark (Verordnung, aus dem Gedächtnis zitiert).
- **Korrektur:** In `INFO.zs.t` und `INFO.ss5.t` ergänzen: „Netzseitiger Anschlussraum und Zählerplatz sind plombiert. Arbeiten dort nur durch einen beim Netzbetreiber eingetragenen Installateur (§ 13 NAV); Inbetriebsetzung über Netzbetreiber-Portal.“

### B7. Stall (VDE 0100-705): wesentliche Anforderungen fehlen
- **Fundstelle:** `RTYPES['Stall / Landwirtschaft']` Z. 1314, `roomRules()` Z. 1556.
- **Was fehlt bzw. korrekt ist:**
  - **Zusätzlicher Schutzpotentialausgleich** in Räumen für Nutztiere. Alle fremden leitfähigen Teile sind zu verbinden, Bewehrung im Boden einbeziehen (Potentialsteuerung). Leiter gegen mechanische Beschädigung und Korrosion schützen (WEKA). Evidenz stark. In der App fehlt das ganz.
  - **TN-S ab Speisepunkt:** Kein PEN im Betrieb. Eine Zuleitung zu einer Stall-UV muss 5-adrig sein (WEKA). Evidenz mittel–stark.
  - **FI-Werte:** Steckdosen ≤ 32 A mit 30 mA, Steckdosen > 32 A mit ≤ 100 mA, alle übrigen Kreise ≤ 300 mA (HD 60364-7-705, belegt über BS 7671; deutsche Fassung inhaltsgleich nach meinem Kenntnisstand). Evidenz mittel. Die App deckt 30 mA (über die 410-Regel) und 300 mA ab, aber nicht > 32 A.
  - **Leuchten in feuergefährdeten Bereichen:** begrenzte Oberflächentemperatur, mindestens IP54 (WEKA). Die App verlangt für „Lager (Heu/Stroh)“ nur IP44 als Regel und IP5X nur „empfohlen“ (Z. 1315).
  - **Kabel:** gegen Tiere und Nager geschützt und außer Reichweite verlegen. Steht als Hinweis drin, gut.
- **Korrektur:**
  - `RTYPES['Stall / Landwirtschaft'].hint += ' Zusätzlicher Schutzpotentialausgleich (Boxen, Tränken, Gitter, Bewehrung) Pflicht. Zuleitung 5-adrig (TN-S, kein PEN).'`
  - `RTYPES['Lager (Heu/Stroh)'].ip='IP54'`.
  - Neue `roomRules`-Prüfung: Steckdose > 32 A ohne FI ≤ 100 mA ergibt `err`.
  - Text der 30-mA-Fehlermeldung im Stall präzisieren (siehe C4).
- Praxisbezug: Laut Projektkontext stehen Elektroarbeiten in einem Pferdestall an. Dieser Punkt ist deshalb praktisch relevant.

### B8. Brandschutzschalter (AFDD) nirgends erwähnt
- **Korrekt:** Nach DIN VDE 0100-420:2019-10 ist für Räume mit Schlafgelegenheiten, feuergefährdete Betriebsstätten (Heu-/Strohlager), Räume aus brennbaren Baustoffen (Holzbau, Holzstall) und Orte mit unersetzbaren Gütern eine Risiko- und Sicherheitsbewertung durchzuführen. Der Einbau von AFDD wird dort empfohlen, nicht pauschal vorgeschrieben (Handwerksblatt, Forum-Verlag).
- **Evidenz:** mittel (Fachpresse).
- **Korrektur:** In `RTYPES` für „Lager (Heu/Stroh)“, „Stall / Landwirtschaft“ und „Schlafraum“/„Kinderzimmer“ einen `hint` ergänzen: „Risikobewertung nach DIN VDE 0100-420 (421.7): Brandschutzschalter (AFDD) empfohlen.“

### B9. FI 40 A hinter SLS E50/E63 ohne Hinweis; FI-Typ fest „A“
- **Fundstelle:** `fi4`/`fi2` immer 40 A (Z. 438, MAT Z. 1228), SLS bis 63 A wählbar.
- **Korrekt:** Ein RCCB hat keinen eigenen Überlastschutz. Entweder liegt I_n(FI) ≥ I_n der Vorsicherung, oder die Überlast ist über die nachgeschalteten LS und den Gleichzeitigkeitsfaktor nachweislich ausgeschlossen. Zusätzlich gilt die vom Hersteller angegebene maximale Vorsicherung für die Kurzschlussfestigkeit. Typ A ist das Minimum. Bei Geräten mit Frequenzumrichter (Wärmepumpe, Waschmaschine mit FU, manche Induktionsherde) ist nach Herstellerangabe Typ F oder B nötig, bei Wallboxen Typ A plus DC-6-mA-Erkennung oder Typ B.
- **Evidenz:** mittel (Herstellerpraxis, VDE 0100-530, aus dem Gedächtnis).
- **Korrektur:**
  - `hint`, wenn `fi.In < sls.rating`: „FI 40 A hinter SLS ${r} A: Überlastschutz des FI über Summe der LS nachweisen oder FI 63 A wählen; max. Vorsicherung laut Hersteller beachten.“
  - FI-Typ wählbar machen (A/F/B) und in die Doku übernehmen.

### B10. Widerspruch „blaue Ader als Außenleiter“: Hauptprüfung sagt Fehler, Dosen-Modul sagt erlaubt
- **Fundstelle:** `check()` Z. 684 meldet `err` „Außenleiter darf nicht blau sein“. Das Dosen-Modul (um Z. 2420) sagt: „dann darf die blaue Ader als Phase genutzt werden (DIN VDE 0100-510)“.
- **Korrekt:** Das Dosen-Modul hat recht. Ist kein N nötig, darf die hellblaue Ader für andere Zwecke außer PE verwendet werden (VDE 0100-510, Abschnitt 514.3.1). Kennzeichnen ist gute Praxis.
- **Evidenz:** stark.
- **Korrektur:** In `check()` bei Außenleiter + `blau`: `warn` statt `err`, wenn im selben Kabel kein N geführt wird. Text: „Blau als Außenleiter nur, wenn kein N gebraucht wird – an beiden Enden schwarz/braun kennzeichnen.“ Bei Grün-gelb bleibt es `err`.

---

## C. Unsauber

### C1. Hauptleitung im TN-C-Teil: 4-adrig, nicht 5-adrig
- **Fundstelle:** Im Beispiel kommt der HAK mit L1–L3 + PEN, also 4 Leiter. Die Hauptleitung ist aber als „NYM-J 5×10“ dokumentiert (Doku-Übersicht: „Hauptleitung NYM-J 5×10, 6 m“).
- **Korrekt:** Bei PEN-Aufteilung im netzseitigen Anschlussraum ist die Hauptleitung 4-adrig. Bei Aufteilung an der Übergabestelle ist sie 5-adrig, mit separatem Schutzpotentialausgleichsleiter zur HES (EAM Netz, Aufbauhinweise Kap. 4.2).
- **Evidenz:** stark (Netzbetreibervorgabe).
- **Korrektur:** CABLES um `NYM-J 4×16`, `NYY-J 4×16`, `NYY-J 4×25` ergänzen. Standard je nach Lage der PEN-Aufteilung wählen. In der Doku „Hauptleitung (TN-C, 4-adrig)“ ausweisen.

### C2. Kein Spannungsfall und keine Abschaltbedingung trotz erfasster Längen
- **Fundstelle:** `len` je LS wird erfasst (Z. 968, Doku), aber nicht ausgewertet. Getestet: 120 m bei 1,5 mm² und B16 ergibt keine Meldung.
- **Korrekt:**
  - Spannungsfall vom Zähler bis zum Verbraucher ≤ 3 % nach DIN 18015-1, gerechnet mit dem Nennstrom des LS. Ab Zähler gerechnet ergibt das bei 1,5 mm² und B16 ca. 18 m: ΔU = 2·L·I/(κ·A) = 2·18·16/(56·1,5) ≈ 6,9 V ≈ 3 %.
  - Abschaltbedingung TN: 0,4 s für Endstromkreise ≤ 32 A (VDE 0100-410, Tab. 41.1). Bei B16 gilt Z_s ≤ 230 V / (5·16 A) ≈ 2,9 Ω.
- **Evidenz:** mittel (Gedächtnis; 3 %-Wert verbreitet zitiert, Normtext nicht eingesehen).
- **Korrektur:** In `check()` `dU = 2*len*In/(56*A)/230*100` je Kreis. `warn` ab 3 %, `hint` ab 2 % („Querschnitt erhöhen“). Für Drehstrom `√3·L·I/(κ·A)/400`. Abschaltbedingung nur als Hinweis „Schleifenimpedanz bei der Erstprüfung messen“, weil die Netzimpedanz unbekannt ist.

### C3. Kundendoku: erfundener Wert „3 × NH 63 A gG“
- **Fundstelle:** `renderPaper()` Z. 1281, `MAT.hak` Z. 1224, `units()` Z. 598.
- **Korrekt:** Die Größe der Hausanschlusssicherung legt der Netzbetreiber fest. Die App weiß sie nicht. Sie in die Kundendoku zu schreiben, ist ein erfundener Wert und widerspricht der Projektregel „fehlende Daten als fehlend anzeigen“.
- **Korrektur:** Feld `hak.nh` (leer = „vom Netzbetreiber, eintragen“). In der Doku „Hausanschlusssicherung: ___ A gG (Angabe Netzbetreiber)“.

### C4. 30-mA-Fehlermeldung zitiert „in Wohnungen“ auch für Stall, Lager und Gewerbe
- **Fundstelle:** `check()` Z. 673.
- **Korrekt:** VDE 0100-410 verlangt 30 mA für Steckdosen ≤ 32 A (Laien, allgemeine Verwendung), für Außen-Endstromkreise ≤ 32 A und für Beleuchtungs-Endstromkreise **in Wohnungen**. Eine Lichtleiste im Stall fällt unter 705 (≤ 300 mA). Die Meldung ist sicherheitlich auf der richtigen Seite, die Begründung ist aber falsch.
- **Korrektur:** Für Leuchten in Räumen mit `RTYPES` ∉ Wohnräume: `warn` „30 mA empfohlen; Pflicht nach 705: FI ≤ 300 mA“.

### C5. Bad: Bereich 0 und Schutzpotentialausgleich
- **Fundstelle:** `RTYPES['Bad / Dusche']` Z. 1309: „Leuchten dort mindestens IPX4“ für Bereiche 0–2.
- **Korrekt:**
  - Bereich 0: nur fest angeschlossene SELV-Verbraucher (12 V AC / 30 V DC), mindestens IPX7.
  - Bereich 1 und 2: IPX4, bei Strahlwasser zur Reinigung IPX5.
  - Bereich 2: außer Rasiersteckdosen-Einheiten und SELV keine Steckdosen.
  - Zusätzlicher örtlicher Schutzpotentialausgleich: eine Quelle (Baunetz) nennt ihn als vorgeschrieben, entbehrlich bei Hauptpotentialausgleich. Die zweite Quelle (haustechnikdialog) erwähnt ihn nicht.
- **Evidenz:** IP-Werte mittel–stark, Potentialausgleich **unsicher** (Quellen widersprechen sich teilweise).
- **Korrektur:** Hint: „Bereich 0: nur SELV ≤ 12 V AC, IPX7. Bereich 1–2: IPX4 (IPX5 bei Strahlwasser). Steckdosen frühestens 60 cm neben Wanne/Dusche, alle Kreise über FI ≤ 30 mA Typ A. Zusätzlichen Potentialausgleich mit EFK klären.“

### C6. Selektivität SLS ↔ LS als Tatsache dargestellt
- **Fundstelle:** `simulate()` Z. 622: „Der Hauptschalter davor bleibt drin.“ `INFO.sls` Z. 531.
- **Korrekt:** Eine Herstellertabelle (SLS-Technikinfo) nennt Selektivität des SLS E35 zu LS bis B16, für E40–E63 bis B16/B25 bei > 6 kA. Hinter einem SLS E35 ist ein 3×B20/B25/B32 also nicht sicher selektiv.
- **Evidenz:** mittel (eine Herstellerquelle).
- **Korrektur:** `hint`, wenn `ls.rating > 16 && sls.rating <= 35`: „Selektivität SLS E35 ↔ LS > B16 laut Herstellertabelle prüfen.“ Simulationstext: „… bleibt in der Regel drin (Selektivität laut Herstellertabelle).“

### C7. Begriff „SLS“ und „Hauptleitungsabzweigklemme“ im anlagenseitigen Raum
- „SLS“ ist ein Handelsname. Normbegriff ist „selektiver Hauptleitungsschutzschalter (SH-Schalter), DIN VDE 0645“. Vorschlag: einmal in `INFO.sls` nennen.
- „Hauptleitungsabzweigklemme“ bezeichnet eigentlich die Klemme im Hauptstromversorgungssystem (vorgezählt). Für die Abgangsklemme zur UV im anlagenseitigen Anschlussraum ist „Abgangsklemme 5-polig“ bzw. „Anschlussklemme anlagenseitig“ klarer.
- **Evidenz:** mittel. Rein begrifflich, kein Sicherheitsthema.

### C8. Kundendoku: Badge „Keine Fehler im Plan“
- **Fundstelle:** `renderPaper()` Z. 1277.
- **Problem:** Beim Kunden liest sich das wie eine Prüfaussage. Wegen A1, A4, A5 und C2 ist die Planprüfung nicht vollständig.
- **Korrektur:** Text „Plan-Check der App: keine Auffälligkeiten (keine Normprüfung, keine Messung)“.

---

## D. Hinweise

### D1. Prüfprotokoll fehlt in der Kundendoku (wichtigster Hinweis)
- **Fundstelle:** `renderPaper()`. Es gibt nur den Satz „ersetzt nicht die Erstprüfung …“ (Z. 1282) und das Feld „Unterschrift Elektrofachkraft“ (Z. 1295).
- **Korrekt:** Vor Inbetriebnahme ist eine Erstprüfung nach DIN VDE 0100-600 mit Dokumentation Pflicht. Inhalt:
  - Besichtigen.
  - Durchgängigkeit von Schutzleiter und Schutzpotentialausgleich (R_PE).
  - Isolationswiderstand (R_iso ≥ 1 MΩ bei 500 V DC).
  - Schleifenimpedanz Z_s bzw. Kurzschlussstrom I_k.
  - FI-Prüfung (IΔ, t_A, U_B).
  - Drehfeld, Spannungsfall nach Bedarf.
  - Erdungswiderstand.
  - Übliche Form ist das ZVEH-Formular „Übergabebericht/Prüfprotokoll“.
- **Evidenz:** stark.
- **Korrektur:** Abschnitt „Prüfprotokoll nach DIN VDE 0100-600“ als Tabelle je Stromkreis anlegen:
  - Spalten: Nr. · LS · Leitung · R_PE (Ω) · R_iso (MΩ) · Z_s (Ω) / I_k (A) · FI IΔn · I_Δ (mA) · t_A (ms) · U_B (V) · i. O.
  - Kopf: Messgerät/Seriennr., Prüfer, Datum, Drehfeld rechts ✓, Erdungswiderstand.
  - Leerfelder, keine Platzhalterwerte.
  - Unterschrift „Errichter/Prüfer (Elektrofachkraft)“.

### D2. Überspannungsschutz Typ 2 bei > 10 m
Die App nennt das eine „Herstellerempfehlung“ (Z. 542, 694). Nach DEHN-Zusammenfassung ist der wirksame Schutzbereich eines SPD in der 534 selbst mit 10 m festgelegt. Danach ist zu prüfen und es gibt drei Optionen (zusätzlicher SPD, Schutzpegel ≤ 50 % U_w, Two-Port-SPD). Text auf „DIN VDE 0100-534: Schutzbereich 10 m“ ändern. Evidenz mittel.

### D3. FI-Aufteilung
Der Hinweis „mindestens zwei FIs“ greift erst ab 4 Verbrauchern und nur bei genau einem RCCB (Z. 698). Gute Praxis ist die Aufteilung von Licht und Steckdosen je Raumgruppe auf verschiedene FIs, damit nach einer Auslösung nicht alles dunkel ist (VDE 0100-530 bzw. DIN 18015-1, Gedächtnis). Text um „Licht und Steckdosen eines Raums auf verschiedene FIs“ ergänzen. Evidenz mittel.

### D4. Herd
3×B16 an 5×2,5 und Hinweis „30 mA nicht zwingend“ sind korrekt (410). Hinweis ergänzen: „Induktionskochfeld: FI-Typ laut Hersteller (teils Typ F).“

### D5. Korrekt und gut umgesetzt (zur Bestätigung)
- PEN mindestens 10 mm² Cu.
- N und PE nach Aufteilung nie wieder verbinden.
- Eigene N-Klemme je FI, mit Prüfung.
- L und N über denselben FI.
- Schalter schaltet L.
- LS-Kennlinie (1,13/1,45 · I_n; B = 3–5 · I_n).
- SPD vor dem FI.
- SLS vor dem Zähler, mit Prüfung.
- SPD Typ 1 im netzseitigen, Typ 2 im anlagenseitigen Anschlussraum (deckt sich mit EAM Netz).
- 5 Sicherheitsregeln und Prüfschritte im Dosen-Modul.
- HAK-Sicherungstausch nur durch Netzbetreiber bzw. eingetragenen Installateur.

---

## Priorität für die Umsetzung
1. A1 (LS ↔ Querschnitt), A2 (300-mA-„geschützt“), A3 (offener N = „kein Messwert“)
2. A4 (HES/Potentialausgleich), D1 (Prüfprotokoll in Doku), B1 (SPD in Doku falsch)
3. A5/C1 (Hauptleitung), B2/B3/B4 (SPD-Texte), B7 (Stall 705), B6 (NAV)
4. Rest

## Quellen
- EAM Netz: Aufbauhinweise für direkt messende Zähleranlagen (PEN-Auftrennung im NAR, Hauptleitung 4-/5-adrig, SPD Typ 1 im NAR, Typ 1/2 im AAR): https://www.eam-netz.de/fileadmin/user_upload/Fuer_Zuhause/Hausanschluss/Anschlussbedingungen/Aufbauhinweise_fuer_direkt_messende_Zaehleranlagen_im_Netzgebiet_der_EAM_Netz_GmbH_final_1.pdf
- DEHN: Anforderungen VDE-AR-N 4100 an SPD (Funkenstrecke, 25 kA, nicht im HAK): https://www.dehn.de/sites/default/files/media/files/sd-anforderungen-vde-ar-n-4100.pdf
- DEHN: Neuerungen DIN VDE 0100-443/-534 (Pflicht, Übergangsfrist, 10 m, 0,5 m, Querschnitte): https://www.dehn.de/sites/default/files/media/files/Neuerungen-DIN-VDE-0100-443-534-DS273.pdf
- DEHN: Einfamilienhaus (TNC/TNS-Varianten, HES): https://www.dehn.de/sites/default/files/media/files/WPX001-D-Einfamilienhaus.pdf
- Baunetz Wissen: Hauptstromversorgungssystem (Hauptleitung ≥ 63 A, 0,5 % Spannungsfall, 25 kA): https://www.baunetzwissen.de/elektro/fachwissen/grundinstallationen/hauptstromversorgungssystem-152994
- Hager/SLS-Technikinfo (Selektivität SLS ↔ LS): https://cdn.shopify.com/s/files/1/0656/6894/0044/files/22DE0001-TECHNIK-SLS-SCHALTER.pdf
- WEKA: DIN VDE 0100-705 (zusätzlicher Potentialausgleich, IP44, TN-S, IP54 Leuchten): https://www.weka.de/elektrosicherheit/vde-0100-705-landwirtschaftliche-und-gartenbauliche-betriebsstaetten/
- Tradeskills4u (BS 7671, FI-Werte 705: 30/100/300 mA): https://www.tradeskills4u.co.uk/wp-content/uploads/2023/04/Requirements-for-RCD-Protection-PDF-CG-v3.pdf
- Handwerksblatt: AFDD nach 0100-420:2019: https://www.handwerksblatt.de/themen-specials/hitzige-debatte-um-brandschutzschalter/neue-regelung-fur-brandschutzschalter
- Forum Verlag: AFDD-Pflicht? https://forum-verlag.com/fachwissen/elektrosicherheit-und-elektrotechnik/afdd-brandschutzschalter/
- Baunetz Wissen: Bad nach 0100-701: https://www.baunetzwissen.de/drucken/elektroinstallation-in-raeumen-mit-badewanne-oder-dusche-152978
- Haustechnikdialog: Schutzbereiche Nassräume: https://www.haustechnikdialog.de/SHKwissen/1111/Schutzbereiche-in-Nassraeumen
- Hinweis: VDE-AR-N 4100 liegt laut Hager inzwischen als Fassung 2026-04 vor (https://hager.com/de/wissen/normen/vde-ar-n-4100). Die Inhalte der neuen Fassung konnte ich nicht einsehen. Die Aussagen oben beruhen auf Quellen zur Fassung 2019. Vor Umsetzung gegen die 2026er-Fassung und die TAB des örtlichen Netzbetreibers prüfen.
