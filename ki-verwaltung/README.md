# KI-Schulungen für die öffentliche Verwaltung

Schulungsangebot rund um den sicheren und produktiven Einsatz generativer KI in Behörden und Kommunen.

## Inhalt

### Einstiegskurs „KI-Kompetenz für die Verwaltung“ (1 Tag)

| Datei | Zweck |
|---|---|
| [kurs-ki-kompetenz/curriculum.md](kurs-ki-kompetenz/curriculum.md) | Lernziele, Tagesablauf, Inhalte je Lerneinheit, Trainerhinweise, Vorbereitungs-Checkliste, Varianten |
| [kurs-ki-kompetenz/uebungen.md](kurs-ki-kompetenz/uebungen.md) | Fallkarten (Ampel), sechs Praxisstationen, Fehlersuche, Transferplan |
| [kurs-ki-kompetenz/handout-ampel.md](kurs-ki-kompetenz/handout-ampel.md) | Handout für Teilnehmende auf einer Seite: Ampel-Regel, R-A-K-F, F-A-K-T |
| [kurs-ki-kompetenz/teilnahmebescheinigung.md](kurs-ki-kompetenz/teilnahmebescheinigung.md) | Vorlage für den Nachweis der KI-Kompetenz (Art. 4 KI-VO) |
| [kurs-ki-kompetenz/folien/KI-Kompetenz-Verwaltung-Folien.pdf](kurs-ki-kompetenz/folien/KI-Kompetenz-Verwaltung-Folien.pdf) | Foliensatz (38 Folien) als PDF zum Präsentieren und Drucken |
| [kurs-ki-kompetenz/folien/sprechernotizen.md](kurs-ki-kompetenz/folien/sprechernotizen.md) | Sprechernotizen zu jeder Folie |
| [kurs-ki-kompetenz/folien/](kurs-ki-kompetenz/folien/) | Quelldateien des Foliensatzes (`deck.json` und je Folie eine HTML-Datei); die bearbeitbare Fassung mit PowerPoint-Export liegt als Slides-Artifact auf claude.ai |
| [kurs-ki-kompetenz/uebungsblaetter.md](kurs-ki-kompetenz/uebungsblaetter.md) | Übungsblätter 1–11 für Teilnehmende plus Lösungen für Trainer |
| [kurs-ki-kompetenz/Uebungsblaetter-KI-Kompetenz.pdf](kurs-ki-kompetenz/Uebungsblaetter-KI-Kompetenz.pdf) | Übungsblätter als druckfertiges A4-PDF, jedes Blatt auf eigener Seite |

### Vertrieb

| Datei | Zweck |
|---|---|
| [vertrieb/seminarbeschreibung.md](vertrieb/seminarbeschreibung.md) | Kurz- und Langtext für Kataloge von Verwaltungsakademien und Studieninstituten, Anschlussangebote |
| [vertrieb/onepager-it-dienstleister.md](vertrieb/onepager-it-dienstleister.md) | Angebot auf einer Seite für kommunale IT-Dienstleister und IT-Abteilungen |
| [vertrieb/preisliste.md](vertrieb/preisliste.md) | Preise aller Formate, Konditionen, Reisekosten, Absagebedingungen |
| [vertrieb/angebotsvorlage.md](vertrieb/angebotsvorlage.md) | Angebotsvorlage für Behörden mit Leistungsbeschreibung, Preistabelle, USt-Varianten und Bedingungen |
| [vertrieb/bestaetigung-ust-befreiung.md](vertrieb/bestaetigung-ust-befreiung.md) | Entwurf: Bestätigung der Behörde zur Umsatzsteuerbefreiung nach § 4 Nr. 21 UStG (mit Steuerberater abstimmen) |

### Aufbaukurs „KI in der Vergabestelle“ (1 Tag, Entwurf)

| Datei | Zweck |
|---|---|
| [kurs-ki-vergabe/curriculum.md](kurs-ki-vergabe/curriculum.md) | Vergabe-Ampel, Tagesablauf, Inhalte je Lerneinheit, Checkliste „KI beschaffen“, offene Punkte für die Abstimmung mit der Praxis |
| [kurs-ki-vergabe/uebungen.md](kurs-ki-vergabe/uebungen.md) | Fallkarten, fünf Praxisübungen (Leistungsbeschreibung, Neutralitätscheck, Bewertungsmatrix, Bieterfrage, Vergabevermerk), Transferplan |

### Websites

Zwei statische Websites, ohne Build direkt hostbar (Webspace oder GitHub Pages), beide „ein Angebot von simplydevelop“:

| Ordner | Domain | Zielgruppe |
|---|---|---|
| [../website/amtsklug/](../website/amtsklug/) | `amtsklug.de` | Behörden und Kommunen |
| [../website/kiklug/](../website/kiklug/) | `kiklug.de` | Unternehmen |

Jede Site hat `index.html`, `ki-regelwerk.html` und eine eigene `style.css`. Deployment (Docker auf dem Optiplex, Reverse-Proxy auf dem VPS): [../website/deploy/README.md](../website/deploy/README.md). Vor dem Livegang: Platzhalter `[...]` ersetzen, `impressum.html` und `datenschutz.html` je Site anlegen, Domains registrieren (`amtsklug.de`, `kiklug.de`, `ki-klug.de`, `kiregelwerk.de`, `ki-in-der-verwaltung.de`; die letzten drei als Weiterleitung), Namen im DPMA-Register prüfen, Google Fonts lokal einbinden.

### KI-Regelwerk

[../ki-regelwerk/gliederung-muster.md](../ki-regelwerk/gliederung-muster.md): Projektablauf und Gliederung der Muster-Regelung (KI-Dienstanweisung für Behörden, KI-Richtlinie für Unternehmen) mit Leitfragen für den Workshop.

## Angebotsleiter

1. **KI-Kompetenz für die Verwaltung** (1 Tag, alle Beschäftigten) – Einstieg, Nachweis nach Art. 4 KI-VO
2. **Führungskräfte-Kompakt** (½ Tag) – Strategie, Richtlinie, Personalrat
3. **Fachbereichs-Werkstatt** (1 Tag) – Anwendungsfälle und Prompt-Bibliothek je Fachbereich
4. **Train-the-Trainer** (2 Tage) – Multiplikatoren und KI-Lotsen
5. **Technische Vertiefung** (2 Tage, IT) – souveräne/lokale Sprachmodelle, RAG, Agenten, Sicherheit

## Pflege

- Den rechtlichen Stand (KI-VO inkl. „Digital Omnibus“, Leitlinien von BSI und Datenschutzbehörden) vor jedem Einsatz prüfen – siehe Abschnitt am Ende des Curriculums.
- Für Inhouse-Termine die Ampel-Regel und Übungen an die Dienstanweisung und die Tools der Behörde anpassen.
- Platzhalter `[...]` in den Vertriebstexten ausfüllen.
