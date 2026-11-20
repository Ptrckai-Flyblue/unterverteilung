# Runde 2 – Stellungnahme Elektromeister

Grundlage dieser Stellungnahme:
- mein Bericht `meister.md`
- `lehrer.md` (Elektrotechniklehrer) und `elektriker.md` (Elektriker)
- Foto eines echten Zählerschranks: `/mnt/user-data/uploads/1fb1f1e0-image.png`
- App: `index.html`, nicht verändert

Evidenz: **stark** / **mittel** / **schwach**, Bedeutung wie in Runde 1.

---

## 0. Foto gegen App und gegen meinen Bericht

| Bereich im Foto | Was zu sehen ist | App | Bewertung |
|---|---|---|---|
| Unten links, netzseitiger Anschlussraum (NAR) | 5 waagerechte Kupfer-Sammelschienen. Darauf: SLS 3-polig, ein Überspannungsableiter (SPD), ein 1-poliger LS. Unten links ein grün-gelber Leiter an der untersten Schiene bzw. Klemme. | ss5 + SLS + spdss + eigenes Hutschienengerät „Aufteilung PEN“ | Grundaufbau stimmt. **Abweichung:** Ein eigenes Hutschienengerät „PEN-Aufteilung“ gibt es in so einem Schrank nicht (siehe 3.2). Der 1-polige LS fehlt in der App. |
| Mitte links, Zählerfeld | BKE-Adapter, vorverdrahtete Adern braun/schwarz/grau/blau | Zählerplatz mit BKE | stimmt |
| Oben links, anlagenseitiger Anschlussraum (AAR) | Zwei 5-polige Hauptleitungsabzweigklemmen, eine beschriftet „NOT“. Grün-gelbe Leiter laufen nach rechts oben. | `hlak` oben | Stimmt. Meine Kritik am Begriff (C7 in `meister.md`) **ziehe ich zurück**: Die Klemmen heißen in der Praxis so. |
| Rechts, Verteilerfeld | Oben N/PE/N-Klemmleiste, darunter FI und weitere Geräte, unten N-Klemmen | fehlt im Zählerschrank (die App setzt eine separate UV) | Hinweis: Im EFH sitzt die Verteilung oft im selben Schrank. Die App sollte „Verteilerfeld im Zählerschrank“ erlauben. Dann entfällt die Zuleitung zur UV. |
| Unten rechts | APZ/RfZ-Feld (Raum für Zusatzanwendungen) mit Netzwerkkabel | fehlt | Hinweis, kein Sicherheitsthema |

Zum 1-poligen LS im NAR: Er ist sehr wahrscheinlich die Spannungsversorgung für den APZ/RfZ aus dem ungezählten Bereich. Das ist üblich, in der Ausführung aber **netzbetreiberabhängig**. Evidenz: mittel. Aus dem Foto sehe ich nicht, wohin der LS geht.

Grenzen des Fotos:
- Ob der SPD als 3+0 (TNC) oder 4+0/3+1 (TNS) ausgeführt ist, ist nicht zu erkennen.
- Wo der PEN genau landet, ist nicht eindeutig. Ich sehe einen grün-gelben Leiter an der untersten Schiene. Die Zuordnung der Schienen (welche ist PE, welche N) und eine Brücke sind nicht sicher erkennbar.
- Die Haupterdungsschiene ist nicht im Bild. Üblich ist, dass sie außerhalb des Schranks sitzt, bei Hausanschluss/Fundamenterder. Das Foto widerlegt also nicht, dass sie fehlt.

---

## 1. Zustimmung

**Lehrer, Punkte, die ich übernehme** (davon habe ich A4, A5 und B1–B3 selbst übersehen):
- **A1, 300 mA „Du bist geschützt“:** deckungsgleich mit meinem A2.
- **A2, offener N:** deckungsgleich mit meinem A3.
- **A3, „N und PE ungefährlich“:** Ich teile die Kritik an den Texten. Besonders falsch ist „N–PE direkt miteinander verbunden“. Sie sind nur an der PEN-Aufteilung verbunden.
- **A4, geführter Aufbau:** Er verdrahtet unter Spannung und hat keinen Prüfschritt. Das ist ein gefährliches Vorbild.
- **A5, „SLS schaltet das ganze Haus ab“:** Das ist falsch. Der NAR mit Schienen, SPD und SLS-Eingang bleibt hinter NH 63 unter Spannung, mit hohem Kurzschlussstrom.
- **A6, § 13 NAV:** deckungsgleich mit meinem B6.
- **B1, Schutzmaßnahme im TN:** Die eigentliche Schutzmaßnahme ist automatische Abschaltung über PE und LS (VDE 0100-410, Abschnitt 411.3, 0,4 s). Der 30-mA-FI ist ein *zusätzlicher* Schutz.
- **B2, Herd ohne N:** Sternpunktverschiebung statt „kein Strom“.
- **B3, Überlast-Toast:** Zwischen 1,13 und 1,45 · I_n gibt es keine Auslösung innerhalb einer Stunde. Die Formulierung „Sekunden bis Minuten“ ist falsch.
- **B7, Begriffe:** Kurzschluss und Körperschluss sauber trennen.
- **C3 bis C10:** übernehme ich, besonders C4 („Alles korrekt“ → „Keine Planfehler gefunden“), C5 (PE zuerst), C8 (Spannung statt Strom).

**Elektriker, Punkte, die ich übernehme:**
- **A1:** deckungsgleich mit meinem A1.
- **A2, Auto-Querschnitt 1,5 mm² hinter einer 2,5-mm²-Zuleitung:** Richtig, das habe ich übersehen. Die Automatik darf keinen Zustand erzeugen, den die eigene Prüfung bemängeln müsste.
- **A3–A5, Stall:** Potentialausgleich, Höhen in Reichweite der Tiere, IP54 bei Heu/Stroh. Deckt sich mit meinem B7. Die Höhe ≥ 1,5 m ist eine Praxiszahl, wie dort ehrlich angegeben.
- **A6:** Spannungsfall bzw. Länge, deckungsgleich mit meinem C2.
- **B1, NYY:** für das Erdreich nötig. NYY ist **kein** Nagerschutz. Volle Zustimmung.
- **B2–B5, Materialliste:** Stiche, Verschnitt, Aufputz/IP je Raum, Verdrahtung in Metern und Querschnitt. Das ist praxisgerecht.
- **B9, FI-Überlast:** deckungsgleich mit meinem B9.
- **C1, C2, C6:** Leitungen beschriften, Deckel verschrauben und beschriften, „Phasenprüfer ist kein Spannungsprüfer“.

---

## 2. Widerspruch

| Punkt | Kollege sagt | Mein Einwand | Evidenz |
|---|---|---|---|
| Elektriker A1, Tabelle `IZ={'1,5':16,'2,5':20,'4':25,'6':32,'10':40,'16':63}` | konservativ | Für 16 mm² **nicht** konservativ. Bei 3 belasteten Adern liegt 16 mm² in A1 bei ≈ 56 A und in B2 bei ≈ 62 A, also unter 63 A. Für 1,5 mm² Drehstrom (3 belastete Adern) ist B16 nur in Verlegeart C zulässig. → Tabelle nach Aderzahl und Verlegeart trennen (siehe Top 3). | mittel (VDE 0298-4 aus dem Gedächtnis) |
| Elektriker D11 bzw. App, Leitungsfeld am SLS | Die Leitung am SLS ist die Leitung ZS → UV | Die App zeigt das Feld in der Doku als „**Hauptleitung**“, also HAK → ZS, vorgezählt. Das ist eine andere Leitung. Es braucht **zwei** getrennte Felder, siehe Top 8. | stark |
| Lehrer B4, Bad: Schalter in Bereich 2 erlaubt | Ja, laut Baunetz | Die Quellen widersprechen sich: Haustechnikdialog sagt „in den Bereichen 0, 1 und 2 nicht zugelassen“. Ich lasse die strengere App-Regel stehen. Sie ist sicherheitlich unkritisch. Nicht als „fachlich falsch“ einstufen, solange der Normtext nicht vorliegt. Unstrittig korrigieren: Bereich 0 ist nur SELV ≤ 12 V AC mit IPX7. | schwach (offen) |
| Lehrer C2, „Kein Messwert“ → „0 V“ bei abgeschaltetem Leiter | 0 V anzeigen | Nur dann zustimmen, wenn das Modell den Pfad über Lasten (offener N) kennt. Sonst zeigt „0 V“ am offenen N die gefährliche Falschaussage noch deutlicher. Reihenfolge: erst Top 2 umsetzen, dann 0 V. | stark |
| Lehrer A5, Vorschlag „Diese Spannung schaltet nur der Netzbetreiber ab“ | – | Ergänzen: „bzw. der eingetragene Installateur, durch Ziehen der NH-Sicherungen mit NH-Griff und Schutzausrüstung“. Fachlich genauer, gleiche Aussage. | mittel |

---

## 3. Offene Streitpunkte – meine Klärung

### 3.1 SPD 3+0 „L–PEN“ auf den NAR-Schienen, wenn die PEN-Aufteilung auf derselben Schiene sitzt

**Ergebnis: Das ist fachlich richtig darstellbar, aber die App muss es konsistent darstellen.**

- Wird der PEN der 4-adrigen Hauptleitung im NAR auf die PE-Schiene gelegt und mit einer Brücke auf die N-Schiene geführt, dann ist die PE-Schiene an dieser Stelle elektrisch der Aufteilungspunkt (PEN-Potential).
- Ein Ableiter, der L1–L3 gegen diese Schiene ableitet, ist Anschlussschema 1 (Schutzpfad je Außenleiter gegen PEN/PE). Das ist die **TNC-Ausführung (3+0)**, wie Hersteller sie für 40-mm-Sammelschienen ausdrücklich anbieten (DEHN WPX001: TNC-Typ für TN-C, TNS/TT-Typ getrennt).
- Ein N–PE-Pfad ist am Aufteilungspunkt überflüssig, weil N und PE dort niederohmig verbunden sind.
- Evidenz: mittel. Belegt ist die Herstellerzuordnung. Den Wortlaut der VDE 0100-534 zur Lage habe ich nicht gesehen.

Was in der App falsch bzw. inkonsistent ist, und wie es richtig lautet:
1. `INFO.spdss` sagt „Im TN-C-Teil **vor** der PEN-Aufteilung“. Richtig ist: „**am** PEN-Aufteilungspunkt auf der Sammelschiene“.
2. Die Aufteilung ist als eigenes Hutschienengerät „pen“ modelliert. Echt ist sie: PEN-Anschlussklemme an der PE-Schiene plus Brücke PE→N, bei manchen Systemen eine PEN-Klemme mit Trennlasche. Gerät und Prüfregel daran anpassen (Top 6). Sonst meldet die App die echte Ausführung als Fehler (mein B5).
3. Liegt die Aufteilung **woanders**, etwa im HAK mit 5-adriger Hauptleitung, sitzt der SPD im TN-S-Teil. Dann ist die **TNS-Ausführung (4+0 oder 3+1)** zu wählen. Die App braucht dafür eine Auswahl bzw. Prüfregel: `spdss` mit 3+0 nur zulässig, wenn ein HAK-PEN direkt (ohne Leitung dazwischen) auf dieser Schiene ankommt, sonst `warn`.
4. Für den NAR gilt nach VDE-AR-N 4100 (DEHN-Sonderdruck): nur funkenstreckenbasierter Typ 1, Kurzschlussfestigkeit ≥ 25 kA, plombierbar. Das in INFO und MAT aufnehmen.

### 3.2 Wo sitzt die PEN-Aufteilung in so einem Schrank?

- **Im netzseitigen Anschlussraum (unten), auf bzw. an dem 5-poligen Sammelschienensystem.**
  - Die 4-adrige Hauptleitung (L1–L3 + PEN) wird an den Anschlussklemmen der Schienen angeschlossen.
  - Der PEN geht zuerst an die **PE-Schiene bzw. PEN-Klemme** (VDE 0100-540, Abschnitt 543.4: PEN an die Schutzleiterklemme).
  - Von dort führt eine Brücke zur N-Schiene.
  - Ab hier gilt TN-S: Zählerfeld, AAR und Verteilerfeld sind 5-leitrig.
- Die Haupterdungsschiene (Fundamenterder, Schutzpotentialausgleich) wird an diesen Aufteilungspunkt bzw. die PE-Schiene angeschlossen. Sie liegt meist außerhalb des Schranks.
- **Alternative:** Aufteilung bereits an der Übergabestelle (HAK). Dann ist die Hauptleitung 5-adrig, und es braucht einen eigenen Schutzpotentialausgleichsleiter von der Übergabestelle zur Haupterdungsschiene. Quelle: EAM Netz, Aufbauhinweise Kap. 4.2.
- Evidenz: stark (Netzbetreiber-Aufbauhinweise), Detail der Brücke mittel (herstellerabhängig).
- Das Foto passt zur Variante „Aufteilung im NAR“: Ich sehe einen grün-gelben Leiter unten links an den Schienen. Sicher bestätigen kann ich das aus dem Bild nicht.

### 3.3 Blaue Ader als Schaltdraht: Fehler oder erlaubt?

- **Erlaubt.** Wird in einer Leitung kein N gebraucht (z. B. Schalterleitung NYM-J 3×1,5), darf die blaue Ader als Außenleiter bzw. Schaltdraht verwendet werden. Sie muss an beiden Enden dauerhaft gekennzeichnet sein, schwarz oder braun (VDE 0100-510, Abschnitt 514.3.1, DIN EN 60445). Grün-gelb ist nie erlaubt.
- Evidenz: stark. Alle drei Prüfer sind sich einig.
- **Die App muss sich angleichen:** Im Schaltplan ist das `warn` statt `err`, und es braucht eine Aderfarbe „blau, gekennzeichnet“.
- Didaktische Empfehlung, kein Muss: lieber NYM-J 5×1,5 ohne zweckentfremdete blaue Ader.

---

## 4. Gemeinsame Top-15 (nach Gefährlichkeit)

**1. Berühren eines Gehäuses ohne PE (`touch()`, Z. 826)**
- **Soll-Logik:** Nur ein FI mit `idn<=30` darf auslösen. Bei FI > 30 mA kein Auslösen, Toast `danger`.
- **Soll-Text bei 30 mA:** „FI hat nach ca. 20–40 ms abgeschaltet. Den Schlag hast du trotzdem bekommen – meist überlebbar, aber nicht harmlos (Schreck, Sturz). Der FI ist zusätzlicher Schutz und ersetzt keinen Schutzleiter.“
- **Soll-Text bei > 30 mA:** „Lebensgefahr: Ein 300-mA-FI ist Brandschutz, kein Personenschutz. Der Körperstrom bleibt unter seiner Auslöseschwelle.“

**2. Offener N (`buildNets`/`volt()`/`check()`)**
- **Soll-Logik:** Lasten als hochohmige Verbindung L↔N in die Potentialbestimmung einbeziehen. Ein offener N hinter einem eingeschalteten Verbraucher zeigt 230 V.
- **check()-Text:** „N unterbrochen: Gerät geht nicht, **aber** N-Anschluss und Ader bis zur Unterbrechung führen 230 V gegen PE.“
- **Abgeschalteter Leiter, Soll-Text:** „0 V – Spannungsfreiheit nur allpolig mit zweipoligem Prüfer feststellen.“

**3. LS ↔ Querschnitt (`check()`)**
- **Soll-Logik:** I_z-Tabelle VDE 0298-4 (Cu/PVC, 30 °C), getrennt nach 2/3 belasteten Adern und Verlegeart A1/B2/C. Feld „Verlegeart“, Standard B2.
- Maximal zulässiger LS-Nennstrom bei B2 (2 / 3 belastete Adern):

  | Querschnitt | 2 Adern | 3 Adern |
  |---|---|---|
  | 1,5 mm² | B16 | B13 |
  | 2,5 mm² | B20 | B20 |
  | 4 mm² | B25 | B25 |
  | 6 mm² | B32 | B32 |
  | 10 mm² | 50 | 40 |
  | 16 mm² | 63 | 50 |

- `err` bei `rating > Iz`. Text: „B20 zu groß für 1,5 mm² – Leitung überhitzt, bevor der LS auslöst. Höchstens B16 oder 2,5 mm².“
- Steckdose zusätzlich: „Schuko ist für 16 A gebaut – höchstens B16, unabhängig vom Querschnitt.“
- Dose: Auto-Querschnitt = Zuleitungsquerschnitt. Der Hinweis „gemischt“ wird `fehler`, wenn der LS > B16 ist oder unbekannt.

**4. Hauptschalter-Texte (`TYPES.sls.sub`, `INFO.sls`, `GSTEPS[2]`)**
- **Soll-Text:** „Schaltet alles **hinter** dem Zähler ab. **Vor** ihm bleiben Hausanschlussleitung, Sammelschienen, Überspannungsschutz und SLS-Eingang unter Spannung (nur über NH-Sicherung, sehr hoher Kurzschlussstrom). Abschalten nur durch Netzbetreiber bzw. eingetragenen Installateur. PV, Speicher oder Aggregat können zusätzlich einspeisen.“

**5. Geführter Aufbau (`lichtkreis(false)`, `GSTEPS`)**
- **Soll-Logik:** Start mit SLS, FI und LS auf AUS.
- **Schritt 0, Soll-Text:** „In echt nie unter Spannung arbeiten: 5 Sicherheitsregeln …“
- **Vorletzter Schritt, Soll-Text:** „Erstprüfung nach DIN VDE 0100-600 durch die Elektrofachkraft: Sichtprüfung, Schutzleiter-Durchgang, Isolationswiderstand – erst dann einschalten; danach Schleifenimpedanz, FI, Drehfeld, Prüfprotokoll.“
- **Danach:** einschalten, dann Prüftaste.
- Reihenfolge an der Lampe: PE, dann N, dann L.

**6. Haupterdungsschiene, Potentialausgleich, PEN-Aufteilung**
- **Neues Bauteil:** „Haupterdungsschiene“ mit Anschlüssen für Erder, Wasser, Gas, Heizung, Blitzschutz.
- **Soll-Logik:** `err`, wenn HAK vorhanden, aber keine HES am PE/PEN-Aufteilungspunkt hängt. Text: „Schutzpotentialausgleich fehlt (VDE 0100-410, 411.3.1.2).“
- **Bauteil „pen“ umbauen:** „PEN an PE-Schiene + Brücke PE→N“. Diese echte Ausführung nicht als Fehler melden.
- **Soll-Text INFO.pen:** „PEN zuerst an PE-Schiene, Brücke zur N-Schiene; nie schalten oder unterbrechen; hier die Haupterdungsschiene anschließen. Bei PEN-Bruch können alle PE-Teile Spannung führen.“
- **MAT:** Potentialausgleichsschiene, H07V-K 6/16 mm² gn-ge.

**7. Stall / Lager (VDE 0100-705/-420)**
- **Soll-Text:** „Zusätzlicher Schutzpotentialausgleich Pflicht (Tränken, Fressgitter, Boxenstangen, Bodenbewehrung). Ab Einspeisepunkt TN-S, Zuleitung 5-adrig, kein PEN. FI in allen Kreisen: Steckdosen ≤ 32 A 30 mA, > 32 A ≤ 100 mA, übrige ≤ 300 mA. Leuchten bei Heu/Stroh mind. IP54, begrenzte Oberflächentemperatur. Geräte außer Reichweite der Tiere (Stallgasse, ≥ ca. 1,5 m). Brandschutzschalter (AFDD): Risikobewertung nach DIN VDE 0100-420.“
- **Soll-Logik:** `RTYPES['Lager (Heu/Stroh)'].ip='IP54'`. Stall-Option „Heu/Stroh“ setzt ebenfalls IP54. `warn`, wenn ein Gerät im Stall unter 1,4 m sitzt. Neue Prüfung „Steckdose > 32 A ohne FI ≤ 100 mA“.

**8. Hauptleitung und Zuleitung UV getrennt**
- **Zwei Felder:**
  - `hak→zs` „Hauptleitung (vorgezählt)“: 4-adrig bei Aufteilung im NAR, Standard NYM-J 4×16 / NYY-J 4×16, Auslegung ≥ 63 A, Spannungsfall ≤ 0,5 %.
  - `hlak→uv` „Zuleitung UV“: 5-adrig, `I_z ≥ SLS-Nennstrom`.
- **Soll-Logik:** `err`, wenn SLS > I_z einer der beiden Leitungen. `hint`, wenn `fi.In(40) < sls.rating`. Text: „FI-Überlast: 63-A-FI wählen oder Gleichzeitigkeit nachweisen; max. Vorsicherung laut Hersteller.“

**9. Drehstrom ohne N**
- **Soll-Text:** „N fehlt: Sternpunktverschiebung – einzelne Platten bekommen bis zu 400 V, Überhitzung möglich. Gleiches gilt für ein ganzes Haus bei unterbrochenem N/PEN.“
- **Soll-Logik:** Status „Überspannung“ statt „kein Strom“, Stufe `err`.

**10. Spannungsfall und Länge**
- **Soll-Logik:** `dU% = 2·L·In/(56·A)/230·100`, bei Drehstrom `√3·L·In/(56·A)/400·100`. `warn` über 3 % (DIN 18015-1, ab Zähler), mit Vorschlag des nächsten Querschnitts.
- **Soll-Text ab ca. 60 m bei 1,5 mm²:** `warn` „Abschaltbedingung fraglich – Schleifenimpedanz messen lassen.“

**11. Rollen von LS, PE und FI richtigstellen**
- **INFO.ls1, Soll-Text:** „Schützt die Leitung – und zusammen mit PE auch den Menschen: Körperschluss → hoher Strom über PE → LS schaltet in ≤ 0,4 s ab (VDE 0100-410). Der 30-mA-FI ist zusätzlicher Schutz.“
- **FI-Untertitel:** „30 mA: zusätzlicher Personenschutz · 300 mA: Brandschutz“.
- **Simulation:** Körperschluss mit PE lässt FI **und** LS auslösen können. Das Wort „nur“ streichen.

**12. Kundendoku (`renderPaper`)**
- **SPD-Zeile:** aus `spd`+`spdss` mit Typ und Ort.
- **NH:** Feld „Hausanschlusssicherung ___ A (Netzbetreiber)“ statt festem Wert 63 A.
- **Neuer Abschnitt „Prüfprotokoll DIN VDE 0100-600“:** Leertabelle je Stromkreis (R_PE, R_iso, Z_s/I_k, IΔ, t_A, U_B, i. O.), dazu Drehfeld, Erdungswiderstand, Messgerät, Prüfer, Datum.
- **Badge:** „Plan-Check der App: keine Auffälligkeiten (keine Normprüfung, keine Messung)“. Der Prüfknopf heißt entsprechend „Keine Planfehler gefunden“.

**13. NAV-Hinweis (`INFO.hak`/`zs`/`ss5`/`sls`, Hilfe)**
- **Soll-Text:** „Am Hausanschluss und im Zählerschrank vor dem Zähler arbeiten nur Netzbetreiber oder ein im Installateurverzeichnis **eingetragener** Betrieb (§ 13 NAV). Elektrofachkraft allein reicht nicht. Plomben nie öffnen; Änderungen beim Netzbetreiber anmelden.“

**14. Überspannungsschutz vereinheitlichen**
- Fehlender SPD wird `warn` statt `hint`. Text: „Pflicht (DIN VDE 0100-443/-534): mind. Typ 2 nach dem Zähler; Typ 1 bei Blitzschutzanlage/Freileitung.“
- **INFO.spdss, Soll-Text:** „am PEN-Aufteilungspunkt; nur funkenstreckenbasierter Typ 1, ≥ 25 kA, plombierbar (VDE-AR-N 4100); 3+0 nur hier, sonst TNS-Ausführung 4+0/3+1“.
- `INFO.hlak` nicht mehr „hier wird der SPD angeschlossen“. Stattdessen: „Typ 2 alternativ im AAR“.
- Prüfregel „SPD vor dem FI“ ergänzen.

**15. Texte und Farben**
- **Blau als Außenleiter:** `warn` mit „nur ohne N in der Leitung, beidseitig schwarz/braun kennzeichnen“, Farbe „blau, gekennzeichnet“ zulassen.
- **volt() N–PE:** „≈ 0 V; N und PE sind nur an der PEN-Aufteilung verbunden, mehr als ein paar Volt = Fehler“.
- **Überlast-Toast:** „1,13–1,45 · I_n: Auslösung evtl. erst nach über 1 h; hier stark gerafft“.
- **Bad:** „Bereich 0: nur SELV ≤ 12 V AC, IPX7“.

---

## Quellen (ergänzend zu Runde 1)
- EAM Netz, Aufbauhinweise direkt messende Zähleranlagen (PEN-Aufteilung im NAR oder an der Übergabestelle, Hauptleitung 4-/5-adrig): https://www.eam-netz.de/fileadmin/user_upload/Fuer_Zuhause/Hausanschluss/Anschlussbedingungen/Aufbauhinweise_fuer_direkt_messende_Zaehleranlagen_im_Netzgebiet_der_EAM_Netz_GmbH_final_1.pdf
- DEHN WPX001 Einfamilienhaus (TNC-/TNS-Varianten des Sammelschienen-Ableiters): https://www.dehn.de/sites/default/files/media/files/WPX001-D-Einfamilienhaus.pdf
- DEHN, Anforderungen VDE-AR-N 4100 an SPD: https://www.dehn.de/sites/default/files/media/files/sd-anforderungen-vde-ar-n-4100.pdf
- Haustechnikdialog, Schutzbereiche (Gegenposition Bad Bereich 2): https://www.haustechnikdialog.de/SHKwissen/1111/Schutzbereiche-in-Nassraeumen
- Hinweis: VDE-AR-N 4100 liegt inzwischen als Fassung 2026-04 vor. Die Inhalte habe ich nicht gesehen. Vor der Umsetzung gegen diese Fassung und die TAB des Netzbetreibers prüfen.
