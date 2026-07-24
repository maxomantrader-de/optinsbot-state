# STATUS.md — Live-Projektstatus (Schnittstelle Sepp ↔ Claude Code)

> **Zweck:** Single Source of Truth für den *aktuellen* Stand. Sepp (Claude Project) liest diese Datei zu Beginn jeder Session und ist damit sofort synchron.
> **Pflege:** Developer- & UI-Agent schreiben bei **jedem** bedeutsamen Commit oben einen neuen Changelog-Eintrag und aktualisieren „Aktueller Fokus" + „Offene Punkte". Kein Prosatext — knappe Stichpunkte.
> **Sync:** Diese Datei (und `TASKS.md`) wird per GitHub Action `mirror-state` automatisch ins öffentliche Read-only-Repo `maxomantrader-de/optionsbot-state` gespiegelt. Keine sensiblen Daten hier eintragen.

---

## 📍 Aktueller Fokus
- **➡️ Zentrale To-do-Liste: [`TODO.md`](TODO.md)** — GoLive-Roadmap nach
  Verantwortlichkeit (Max / Developer Agent), wird ab jetzt dort gepflegt und
  mitgespiegelt. Kritischer Pfad: SMTP + Rechtstexte (Max) → Stripe-Strecke (Agent).
- Kunden-System live: Selbstregistrierung mit Double-Opt-in, Rollen (admin/customer), Pakete (morning/scanner), Landingpage, Profilseite
- Morgenmail an Kunden fertig gebaut & getestet — wartet nur auf SMTP-Zugang (TODO Nr. 1)
- Tägliche Backups (03:30, systemd-Timer) + Rechtsseiten (Entwurf) + lokale Fonts live
- Sync-Mirror: STATUS.md/TASKS.md/TODO.md → öffentliches Repo `optionsbot-state` (Schnittstelle zu Sepp)

## 🎯 Nächste Priorität
- **Max: Morgenroutine-PDF — Variante A oder B wählen** (`docs/morning_pdf_variante_a.pdf` / `_b.pdf`). Commit 2 (Versand + Kundenbereich) hängt daran.
- Max: TODO Nr. 1–4 (SMTP, Rechtstexte, Anwalt, **Stripe-Konto einrichten + Keys eintragen** — Zahlungsstrecke ist codeseitig fertig)
- Developer Agent: Live-Test der Stripe-Strecke im Testmodus, sobald Max die Test-Keys eingetragen hat; danach Widerrufs-Checkbox im Checkout-Flow

## 🚧 Offene Blocker / Entscheidungen (für Max/Sepp)
- **Alle offenen Punkte jetzt in [`TODO.md`](TODO.md)** (einzige gepflegte Liste).
- [x] **DD-Removal-Schwelle 5 % vs. 6 %** — entschieden 2026-07-17 (Backtest): **5 %** bleibt. Alarm-Status differiert nur an 5,4 % der Tage; 5 % ist IBD-Standard und reagiert schneller (weniger Fehlalarme). `RALLY_REMOVAL = 0.05`.
- [x] IVR/DTE-Konflikt — entschieden (Max, 2026-07-08): einheitlich IVR>40 / DTE 45–70, umgesetzt
- [x] Kunden-Delivery-Format Morgenroutine — entschieden & umgesetzt (2026-07-09): E-Mail
- [x] HTTPS auf VPS — erledigt im Security-Audit 2026-07-06

## 📌 Merkliste (im Blick behalten — keine akuten Aufgaben)

> Dinge, die bewusst so entschieden wurden und bei denen wir später ggf.
> nachschärfen. Gepflegt vom Developer Agent, gelesen von Max & Sepp.

- **AD-Linie / Datenquelle:** Läuft auf den offiziell gemeldeten
  Advances/Declines des WSJ/Barron's Markets Diary (kostenlos, kanonisch,
  Barron's-Fallback, Plausibilitätsgate + Screener-Divergenz-Alarm).
  **Aufzeichnungsbeginn 17.07.2026** (Neustart nach behobenem
  Screener-Vorzeichenfehler) — „Hoch seit Aufzeichnungsbeginn" wird erst
  nach einigen Wochen Historie aussagekräftig. Falls je eine garantierte
  Verfügbarkeit (SLA) gewünscht ist: bezahlte API (Polygon/EODHD,
  ~10–30 $/Monat) — bewusst nicht umgesetzt.
- **AD-Divergenz-Alarm:** Kommt per Telegram „Divergenz WSJ vs. Screener",
  die WSJ-Zahlen einmal von Hand gegenprüfen (z. B. Barron's Markets Diary
  im Browser) — WSJ ist maßgeblich, der Screener nur Wächter.
- **Scanner-Abnahmekriterium (Optionsstratege, 16.07.):** Eine Woche
  Funnel-Archiv (`journal/scans/`) beobachten. Liefert der Scanner an Tagen
  mit Trend✓+IVR✓ weiterhin 0 CL-Kandidaten, ist die Greeks-Verfügbarkeit
  um 09:35 ET der nächste Verdächtige — nicht das Regelwerk.
- **B/A-Schwelle Aktien (10 %):** Nach dem Prämien-Fix vermutlich der
  nächste Flaschenhals im Aktienmodul — im Funnel beobachten, ggf. auf
  Prio 2 umstufen.
- **IB-Gateway-2FA:** Nach Session-Abriss wartet der Login auf die
  IBKR-Mobile-Freigabe (Watchdog alarmiert + Auto-Relogin-Push aktiv).
  Dauerhafte Lösung wäre IBKR-Einstellung „2FA seltener abfragen" oder ein
  dedizierter Gateway-Zweituser — Entscheidung Max.
- **Journal-Altzeilen:** Zwei CL-Trades (+425.20 `unverified`,
  +44.57 `estimate`) bei Gelegenheit gegen das IBKR-Statement abgleichen
  (`pnl_source=manual` setzen). Alle künftigen Schließungen kommen
  automatisch aus IBs CommissionReports.
- **Morgenroutine → Kundenmail:** Die neuen Blöcke (Trend-Dauer D/W,
  Sektor-Rotation, 90%-Tage, AD-Linie, F&G-Kipp, PCR-Intraday) sind bewusst
  nur in der Web-Ansicht. Nach Sichtung durch Max entscheiden, was davon in
  Kundenmail/Telegram aufgenommen wird (Two-Commit-Prinzip).
- **AGB-Zielmarkt:** Auf „Verbraucher mit Wohnsitz in Deutschland"
  festgelegt (konservativ). AT/CH-Erweiterung wäre v. a. ein USt-Thema
  (OSS-Verfahren) — vorher Steuerberater fragen.
- **PCR-Intraday-Maximum:** Sammelt seit 17.07. (stündlich 10:50–16:50 ET);
  ab der zweiten Session erscheint das Tagesmaximum in der Morgenroutine.

## 🗂️ Neu angelegte Struktur (in Arbeit)
- `controller/` — Python-Paket-Platzhalter (leeres `__init__.py`), Zweck noch festzulegen
- `journal/morning/`, `journal/positions.json`, `journal/scan_results.json`, `logs/` — Laufzeitdaten, bleiben bewusst untracked

---

## 📝 Changelog (neueste zuerst)

### 2026-07-24 · Developer Agent · Morgenroutine als druckreifes 2-Seiten-PDF (Sepp-Task 5, Commit 1 — Variantenwahl offen)
- **Zwei Layout-Varianten zur Auswahl** (Mockups mit echten Werten vom 24.07., als PDF gerendert):
  **A** tabellarisch-dicht (durchlaufendes Raster) · **B** kartenbasiert mit farbigen Zonenkanten.
  Anmutung Research-Note, nicht Dashboard. **Max wählt A oder B — erst danach Commit 2.**
- Seite 1 „Marktlage": Kopf mit zwei getrennten Zeitangaben, Gesamt-Ampel + Fazit (max. 15 Wörter),
  Volatilität (Wert/Δ Vortag/Δ 5 Tage/Perzentil), VIX-Terminkurve mit Sparkline über 6 Terminmonate,
  Sentiment, Marktbreite, Distribution Days, Panik-Checkliste.
  Seite 2 „Einordnung": Klartext je Indikatorblock, Veränderungstabelle heute/gestern/5 Tage,
  Regelwerk-Bezug (referenzierend), Datenqualitäts-Footer, Risikohinweis.
- **Datenkontrakt** `docs/morning_payload.schema.json` (Version 1.0) trennt Datenbeschaffung und Layout:
  Der Renderer kennt keine Ticker, keine URLs und rechnet nicht — er formatiert nur.
- **Verifier-Gate ist fail-closed**: 8 Checks (Schemakonformität, DD-Gate, Aktualität < 36 h,
  Vollständigkeit, Plausibilität, Vergleichsdaten, Checklisten-Form, Fazit-Satz ohne Handlungssprache).
  FAIL ⇒ kein PDF. Der Renderer verweigert zusätzlich alles, was nicht exakt 2 Seiten ergibt.
- **Zwei-Seiten-Garantie ist vermessen, nicht geschätzt**: Textbudgets im Builder hart begrenzt,
  Deckenwerte experimentell bestimmt (Einordnung 1800/2000 Zeichen, Regeltext 120/130).
  Getestet gegen Budgetmaximum und gegen ausgefallene Datenquellen.
- Barrierefreiheit im Druck: Ampeln über **drei** Kanäle — Farbe, Form (Kreis/Raute/Quadrat/Ring)
  und Kurzcode. Bleibt in Graustufen und bei Farbfehlsichtigkeit lesbar.
- Offline: Schriften lokal aus `web/static/vendor/fonts/`, kein CDN, keine Remote-Assets.
- Bewusst abweichend vom Task: Zeitzonenkürzel wird aus dem echten UTC-Offset abgeleitet
  (**MESZ** im Sommer statt pauschal „MEZ"), sonst stünde im Juli ein falsches Kürzel im Kundendokument.
- Betroffene Dateien: `docs/morning_payload.schema.json`, `docs/morning_payload.example.json`,
  `docs/morning_delivery.md`, `docs/morning_pdf_variante_a.pdf`, `docs/morning_pdf_variante_b.pdf`,
  `bot/morning_payload.py`, `bot/morning_pdf.py`, `web/templates/morning_pdf_a.html`,
  `web/templates/morning_pdf_b.html`, `web/templates/morning_pdf_macros.html`,
  `web/static/css/morning_pdf.css`, `tests/test_morning_pdf.py`, `requirements.txt`
- Tests: 34 neue, gesamte Suite 314 grün.
- Status: **fertig, wartet auf Variantenentscheidung Max**
- Für Sepp relevant: **ja** — Commit 2 (Scheduler, Versand, Kundenbereich, `package_start_date`)
  startet erst nach der Wahl. Alle Delivery-Vorgaben und offenen Punkte in `docs/morning_delivery.md`.
- Nicht enthalten (laut Task): Versandlogik, Kundenzuordnung, Zahlungsanbindung.
- Neue Abhängigkeiten: `weasyprint`, `jsonschema` — auf dem VPS zu installieren (Thomas).

### 2026-07-24 · Developer Agent · Letzte Aufforderungen entfernt + Deploy-Versatz behoben
- Auslöser (Max): In der Telegram-Nachricht standen noch Aufforderungen („dort keine neuen Positionen", „SPS auf Aktien pausiert").
- **Hauptursache war kein Code-Fehler, sondern ein Deploy-Versatz:** Scheduler (`optionsbot.service`) und Web-Dienst liefen seit **23.07. 06:23** — also mit Code von *vor* der Neutralisierung (Phase 2) und vor der Stimmungs-Umstellung. Python hält importierte Module im Prozess; der 08:00-Lauf erzeugte die Nachricht deshalb mit der alten Fassung und schrieb die heutige JSON zurück in die alte Struktur. Beide Dienste neu gestartet, Morgenroutine mit aktuellem Code neu erzeugt.
- Codeseitig verbliebene Aufforderung entfernt: `dd_verdict()` („Kein Neueinstieg – Distribution Days …") ersatzlos gestrichen. Der Aufruf war bereits weg, die Funktion damit tot.
- Dabei eine Informationslücke geschlossen: Aktienkorrelierte Underlyings bei DD-Alarm bekamen ihre Information bisher **ausschließlich** über den Blocker (`dd_note` lieferte für diesen Fall bewusst `None`). `dd_note()` deckt den Fall jetzt beschreibend ab („Verteilungsdruck bei aktiennahen Werten") — heute erhalten alle 18 Underlyings einen Hinweis statt 12.
- Prüfung: 280 Tests grün (inkl. neuem Regressionstest, der alle `dd_note`-Ausgaben gegen eine Verbotsliste von Handlungswörtern prüft). CTA-Sweep über **alle vier Kundenkanäle** mit Echtdaten — Telegram, Mail-HTML, Mail-Text, Betreff und Web-Seite: sauber.
- Betroffene Dateien: `bot/dd_policy.py`, `tests/test_dd_policy.py`
- Status: fertig
- Für Sepp relevant: ja — Kundenkommunikation ist jetzt durchgehend frei von Handlungsaufforderungen; **Lehre: Code-Änderungen an Scheduler/Web erfordern einen Dienst-Neustart, sonst läuft produktiv weiter der alte Stand.**

### 2026-07-24 · Developer Agent · Morgenroutine erkennt Stimmung, statt Strategien zu sperren
- Auslöser (Max): „Heute ein Tag für Short Put Spreads – Bear Call Spreads sind gesperrt (Put/Call 0.61 unter 0.7)" ist zu restriktiv. Die Morgenroutine soll die **Marktstimmung erkennen**, nichts sperren oder freigeben.
- Fachlicher Kern: Das alte Gate-Modell warf zwei Dinge zusammen. Eine bullische Positionierung (Put/Call < 0.7) lief als **Blocker gegen Bear Call Spreads** und färbte den Abschnitt auf „erhöhte Anspannung" — obwohl sie einen **ruhigen**, bullisch positionierten Markt beschreibt.
- Lösung: `derive_gates()` (sps_allowed/bcs_allowed/blockers) ersetzt durch `derive_sentiment()` mit zwei **getrennten** Dimensionen — `mood` (bullisch/neutral/bärisch) und `tension` (ruhig/erhöht/angespannt). Kompass-Verdikte („go/wait/blocked", „heute handelbar", „Kein Neueinstieg") ersetzt durch beschreibende `trend`/`regime`-Felder. Neue neutrale Primitive `derive_trend()`; `derive_setup()` bleibt als Strategie-Zuordnung der Regelwerk-Seite bestehen, wird in der Morgenroutine aber nicht mehr genutzt. `PANIC_PCR = 1.40` in `config/settings.py` zentralisiert (vorher hartkodiert).
- UI: Abschnitt „Marktweite Tagesfilter" → „Marktweite Stimmungslage" (alle drei Kennzahlen werden jetzt immer beschrieben, nicht nur Extreme als Blocker-Bullets); Banner, Sprungnavigation und Methodik-Seite nachgezogen.
- Archiv-Migration: 13 gespeicherte Morgenroutinen auf die neue Struktur überführt (Werte aus dem Archiv gerechnet, nichts geschätzt) — Historie bleibt vollständig anzeigbar.
- Ergebnis für Max' Fall (heutige Echtdaten): „Anspannung: ruhig · Positionierung bullisch" mit „Put/Call 0.61 unter 0.7 – bullische Positionierung, wenig Absicherung nachgefragt".
- Betroffene Dateien: `bot/morning_routine.py`, `config/settings.py`, `web/templates/morning.html`, `web/templates/methodik.html`, `web/app.py`, `tests/test_setup_compass.py`, `CLAUDE.md`
- Prüfung: 282 Tests grün (11 neue für die Stimmungslogik, inkl. Regressionsschutz gegen Freigabe-/Sperr-Keys); Render-Tests für Morgenroutine (alt + neu) und Methodik.
- Status: fertig
- Für Sepp relevant: ja — die Morgenroutine ist jetzt durchgehend zustandsbeschreibend; Gating/Strategiesprache lebt ausschließlich in Scanner/Setup.

### 2026-07-24 · Developer Agent · Morgenroutine kundenseitig neutralisiert (Audit Phase 2, Freigabe Max)
- Entscheidung Max: „grundsätzlich ALLES bei der Morgenroutine neutralisieren" (offene Frage c aus `AUDIT_MORGENROUTINE.md`). Leitsatz: Marktzustand beschreiben statt Handlung vorschreiben (MAR Art. 20 / WpHG).
- Umgesetzt: alle kundenseitigen Strategie-/Handlungsaussagen (SPS/BCS, „handelbar/gesperrt", „keine neuen Trades", „Neueinstiege", „pausiert", Regel-013/001-Bezüge) durch Regime-Sprache ersetzt (Grün=ruhiges Marktregime, Gelb=erhöhte Anspannung, Rot=Stressregime).
- **Quell-Schicht** `build_assessment()` (speist Web + E-Mail + Telegram gleichzeitig): neutrale Headlines/Bullets. Setup-Kompass in der Morning-Ansicht → neutrale Sektion **„Marktweite Tagesfilter"** (nur marktweite Extreme als Regime-Status); Underlying-Check zeigt nur noch Trend/RSI/Prämien-Niveau (SPS/BCS-Spalte + Strategie-Filter entfernt). `/methodik` an die neue Struktur angeglichen.
- **Admin/Scanner unverändert**: `_sector_assessments()` bekam `neutral`-Flag — nur die Morning-Route setzt es; das Scanner-Dashboard behält seine Strategie-Hinweise (per Test verifiziert). `/beispiel`-Ersatzliste hinfällig (Quelle schon neutral).
- Neutralitäts-Hinweis jetzt prominent (Kopf) statt nur im Footer; Abschlusssatz „…ersetzt kein eigenes Regelwerk. Handelsentscheidungen triffst du selbst." in Web/E-Mail/Telegram.
- Betroffene Dateien: `bot/morning_routine.py`, `bot/morning_mail.py`, `bot/morning_telegram.py`, `web/app.py`, `web/templates/morning.html`, `web/templates/methodik.html`, `web/templates/landing.html`, `AUDIT_MORGENROUTINE.md`
- Prüfung: 276 Tests grün; `/morning`, `/methodik`, `/beispiel`, `/welcome` rendern HTTP 200 ohne verbotene Strategiebegriffe; Scanner-Dashboard behält Strategiesprache.
- Status: fertig & **final freigegeben durch Max (2026-07-24)** — Two-Commit-Vorbehalt für die öffentlichen Seiten (`landing.html`, `/beispiel`) aufgehoben, Bewerbung offen. Anwaltliche Prüfung bleibt davon unberührt (TODO „Rechtstexte/Anwalt").
- Standing Guardrail: Falls je ein JSON-Export/eine API der Morgenroutine an Kunden gebaut wird, vorher `verdict_text` neutralisieren (sonst kehrt Strategie-Prosa über den Export zurück).
- Für Sepp relevant: ja — Launch-Produkt ist jetzt durchgängig als Markteinschätzung statt Handlungsempfehlung formuliert; erweitert zugleich den adressierbaren Markt (Kunden mit abweichendem Regelwerk).

### 2026-07-24 · Developer Agent · Error-200-Flut bei Aktien-Optionen (QQQ) behoben — autoritative Strikes je Expiration
- Problem (Log-Befund `optionsbot_2026-07-23.log`): Beim Scan der Aktien/ETFs floss der Scanner in hunderte `Error 200 – No security definition` (z. B. QQQ Sep-2026 Calls, Strikes 826–864 in $1-Schritten). $5-Raster-Strikes (830/835/840…) existieren, die $1-Zwischenstrikes nicht.
- Ursache: dieselbe Merge-Artefakt-Klasse wie beim FOP-Fix vom 2026-07-21. Die über `reqSecDefOptParams` gemergten Chain-Strikes vereinen ALLE Expirations — QQQ/SPY/IWM tragen nahe Geld ein $1-Weekly-Raster, die monatliche Ziel-Expiration weiter OTM aber nur $5. Der $1-Intervallfilter ließ jede Ganzzahl passieren → `qualifyContracts` erzeugte die Fehlerflut.
- Lösung: neue `authoritative_equity_strikes(ib, symbol, expiration, currency, right)` (Analog zu `authoritative_fop_strikes`) fragt via `reqContractDetails` (strike=0) nur das reale Raster GENAU dieser Expiration ab. Put- und Call-Zweig des Aktien-Pfads nutzen sie jetzt; das nun tote `valid_strikes_from_chains` entfernt. Fail-closed (leer → Symbol liefert kein Signal, kein Error-Spam).
- Betroffene Dateien: `bot/scanner.py`
- Prüfung: 276 Tests grün; Syntax/Import ok.
- Status: fertig
- Für Sepp relevant: ja — Scanner-Logs der Aktien-Kategorie sind wieder sauber, kein Error-200-Rauschen mehr.

### 2026-07-22 · Developer Agent · DAX-IVR über VDAX-NEW (V1X@EUREX) statt „nicht berechenbar"
- Problem (Log-Befund Max): DAX lief bei jedem Scan in `Error 162 (HMDS query returned no data … Option_Implied_Volatility)` für FDAX/FDXM/FDXS → „IVR nicht berechenbar – überspringe". **Nicht** der geschlossene Markt (Scanner loggte „Handelszeit aktiv"; die IV-Abfrage ist historisch): IB liefert für die Eurex-DAX-Optionskette schlicht keine IV-Reihe, während TRADES/MA200/RSI funktionieren.
- Lösung: IB führt die Vola-Indizes selbst als IND-Kontrakt — **VDAX-NEW = V1X@EUREX** (direkter DAX-Analog) mit voller Jahres-Level-Historie via `whatToShow='TRADES'` (verifiziert: 253 Tagesbalken). Neuer IVR-Fallback `calc_ib_index_iv_rank(ib, symbol)` + Registry `_VOLA_INDEX_IB = {"DAX": ("V1X","EUREX","EUR")}`, greift genau dann, wenn yfinance keinen Vola-Index hat. OVX//CL, GVZ//GC (yfinance) unverändert.
- Rank-Berechnung in reinen Helfer `_rank_pct(closes, min_len)` refaktoriert (yfinance- und IB-Pfad teilen sich die Logik). Live-Test: DAX-IVR ≈ 19.3 (VDAX-NEW 17.50 in 13.62–33.77) → DAX fließt jetzt durch den Funnel und wird korrekt als „IVR < 40" abgelehnt statt zu erroren.
- Betroffene Dateien: `bot/scanner.py`, `tests/test_scanner_ivr.py` (neu, 9 Tests), `CLAUDE.md`
- Prüfung: 263 Tests grün; Live-Integrationstest gegen Gateway bestätigt V1X-Abruf.
- Status: fertig
- Für Sepp relevant: ja — DAX ist damit vollwertig im Scan-Universum (IVR-Quelle geklärt), keine Error-162-Spam-Zeilen mehr.

### 2026-07-22 · Developer Agent · UI-Feinschliff + Alt-Domain kanonisch auf www.steadyalpha.de
- Domain (Auftrag Max): Alt-Domain `optionbot.neuner-productions.de` proxyte die App bisher direkt durch → man blieb beim Navigieren auf der Alt-Domain hängen. nginx-Site jetzt auf kanonischen **301-Redirect** nach `https://www.steadyalpha.de$request_uri` umgestellt (analog Apex→www). Getestet: `/`, `/dashboard`, `/morning` → 301 mit erhaltenem Pfad; Hauptdomain unverändert. Backup im Scratchpad. **Hinweis: nginx-Config liegt nur auf dem VPS (`/etc/nginx/sites-available/optionsbot`), nicht im Repo.**
- Ampellogo (Sidebar + Mobile-Topbar) verlinkt jetzt auf die Startseite `/`.
- „2026_01" neben dem Logo entfernt (`brand-sub`).
- Dashboard „Marktlage": Label „Regel 013" entfernt.
- Morgenroutine Underlying-Check: Spalte „Heute" (Verdict) entfernt — Header, Body-Zelle, Legende und Tooltip angepasst (Tabelle jetzt 6 Spalten, balanciert).
- Betroffene Dateien: `web/templates/base.html`, `web/templates/dashboard.html`, `web/templates/morning.html`, `/etc/nginx/sites-available/optionsbot` (System, nicht im Repo)
- Prüfung: Jinja-Syntax aller drei Templates ok, Spaltenbalance 6/6 verifiziert, Web-Service neu gestartet (aktiv), Redirect per curl bestätigt.
- Status: fertig
- Für Sepp relevant: ja — Kunden landen jetzt konsistent auf www.steadyalpha.de; UI-Aufräumarbeiten sichtbar in Dashboard + Morgenroutine.

### 2026-07-22 · Developer Agent · Zentraler Verbindungs-Gate (Circuit Breaker) gegen Telegram-Spam
- Problem: Bei totem IB Gateway (ConnectionRefused/Errno 111) verbanden IB-Sync, Gateway-Watchdog und Monitor jeder für sich alle paar Minuten neu und schickten jeder eigene identische Fehler-Telegramme (~alle 1–5 Min).
- Lösung: neues Modul `bot/ib_gate.py` als gemeinsamer Gate. `ensure_connection()` → "ok"/"skip". Zustand CONNECTED/DOWN + Timestamps (persistiert in `journal/ib_gate_state.json`). Retry-Drosselung (frühestens 60 Min erneut verbinden) + flankengesteuerte Alerts: CONNECTED→DOWN = 1× "🔴 …", während DOWN höchstens 1×/60 Min "⚠️ weiterhin down seit …", DOWN→CONNECTED = 1× "✅ …". Alle Jobs teilen sich diesen EINEN Alert-Pfad → ein Ausfall = eine Meldung statt drei.
- Watchdog reimplementiert auf dem Gate; einziger, der neu startet (≤ 1×/Stunde, `restart_due`). IB-Sync + Monitor rufen jetzt zuerst `ensure_connection()` und überspringen bei DOWN still (kein eigener Retry/Telegram). Verbindungsfehler in allen Job-`except`-Blöcken laufen über `ib_gate.mark_down()` statt eigenem `notify_error`. Warning-2110-/RECONNECTACCOUNT-Pfad unverändert.
- Neue Config-Keys (Single Source `config/settings.py`, nichts hardcoded): `CONNECTION_RETRY_INTERVAL_MIN`, `ERROR_ALERT_INTERVAL_MIN`, `GATEWAY_RESTART_INTERVAL_MIN` (je 60), `IB_GATE_STATE_FILE`.
- Normalbetrieb unverändert: lebende Verbindung → nur schneller TCP-Port-Check (sub-ms), keine Zusatzlatenz, keine Meldungen. No-Order-Guard/Exit-Alerts nicht angefasst.
- Betroffene Dateien: `bot/ib_gate.py` (neu), `bot/gateway_watchdog.py`, `scheduler.py`, `config/settings.py`, `tests/test_ib_gate.py` (neu)
- Prüfung: 254 Tests grün. Szenarien a–e (Down/Retry/Recovery/Restart/Normalbetrieb) als Unit-Tests abgedeckt.
- Status: fertig
- Für Sepp relevant: ja — beseitigt den Telegram-Spam bei Gateway-Ausfall; Max bekommt pro Ausfall/Stunde höchstens eine Meldung.

### 2026-07-22 · Developer Agent · Trade-History: neue Spalte „P&L %" (realisiert)
- Was geändert: In der Trade-Historie steht jetzt zwischen „Exit-Grund" und „P&L" die Spalte **„P&L %"** = realisierter P&L als Prozent des Netto-Entry-Credits (`pnl / total_credit × 100`). Anzeige deutsch mit Vorzeichen + 1 Nachkommastelle (`+60,2 %`, `−200,0 %`); Farblogik wie P&L (grün/rot), fehlender/0-Credit → `n/a` (kein Div-by-Zero).
- Datenquelle: `total_credit` (Netto-Entry-Credit, Kostenbasis) und `pnl` aus `journal/trades.csv` — keine erneute Kostenrechnung, nur Verhältnis. Nachkommastellen aus Config (`JOURNAL_PNL_PCT_DECIMALS`).
- Neu: CSV-Export der Trade-Historie (`/journal/export.csv`, Button im Journal-Kopf, Semikolon + BOM für dt. Excel) — enthält „P&L %" an gleicher Position. Gemeinsame Aufbereitung `_prepare_closed_trades()` für Anzeige + Export (DRY).
- Prüfung: 276 Tests grün (13 neue in `tests/test_journal_pnl_pct.py`); Rechen-/Position-/Nullwert-/Export-Tests bestätigt.
- Betroffene Dateien: `web/app.py`, `web/templates/journal.html`, `config/settings.py`, `tests/test_journal_pnl_pct.py`
- Status: fertig
- Für Sepp relevant: ja — Trade-History zeigt/exportiert jetzt die realisierte Rendite je Trade in % des Credits.

### 2026-07-22 · Developer Agent · Netto-Kredit = Kostenbasis (Gebühren abgezogen)
- Was geändert (Befund Max): Der angezeigte Netto-Kredit basierte auf `averageCost` (Brutto-Durchschnittskurs OHNE Gebühren). Jetzt wird die **Kostenbasis** angesetzt: Brutto-Kredit − Eröffnungs-Kommissionen/Gebühren (aus `CommissionReport.commission` der Fills, je conID summiert). `premium_received` und `total_credit` sind damit netto → die /Kontrakt-Anzeige bleibt konsistent (Netto = Brutto − Gebühren).
- Umsetzung: neue Helfer `_commissions_by_conid()` + `_net_premium()` in `bot/ib_sync.py`; angewandt auf Spreads und Einzel-Legs. Fehlt der Gebühren-Report (Altbestand außerhalb des IB-Execution-Fensters), greift transparent der Brutto-Fallback; jeder Abzug wird geloggt (Brutto − Gebühren = Netto).
- Wirkung: gilt für **neu** synchronisierte Positionen. Bestehende offene Positionen behalten ihren gespeicherten Wert, bis sie neu importiert werden (IB-Execution-Fenster vorausgesetzt).
- Prüfung: 242 Tests grün (neue Kostenbasis-Tests). Annahme dokumentiert: `averageCost` ist hier gebührenfrei (Befund Max) → Gebühren werden einmalig abgezogen; gegen die IBKR-Spalte „Kostenbasis" gegenprüfbar.
- Betroffene Dateien: `bot/ib_sync.py`, `tests/test_ib_sync_pnl.py`
- Status: fertig
- Für Sepp relevant: ja — Netto-Kredit/PNL-Basis der Positionsanzeige ist jetzt die tatsächliche Kostenbasis inkl. Gebühren.

### 2026-07-22 · Developer Agent · Exit-Kriterien im gesamten Maxoman-Regelwerk harmonisiert (Konsistenz)
- Was geändert (Auftrag Max „ändere auch das 32-Regel-Regelwerk, sonst durcheinander"): Das ältere Regelwerk (`bot/rules.check_exit`, Monitor-Job) war noch auf 65 % — jetzt überall auf die neue Politik gezogen, damit Engine und Regelwerk identisch melden:
  - **Take-Profit 65 % → 60 %** an ALLEN Stellen: `PROFIT_TARGET_PCT`, `profit_target`/`bcs_profit_target` (FOP+OPT) in `config/settings.py`; Regel-Texte 010/026 (+002/016/029/030) in `bot/notifier.py`; `setup.html` (Exit-Tabelle, Rollier-Tabelle, Regel-Matrix); Dashboard-Tooltip; `CLAUDE.md`.
  - **Break-Even-Bruch als Prio-1-Exit auch in `check_exit`**: neuer `underlying_price`-Parameter (aus dem Manager durchgereicht, `bot/manager.py`), Richtung aus `spread_type` (SPS/BCS). Neuer ExitSignal-Grund `BREAK_EVEN` + Telegram-Label. `check_exit`-Reihenfolge jetzt BREAK_EVEN → PROFIT_TARGET → QUICK_WIN → STOP_LOSS → DELTA → DELIVERY → DTE.
  - Manager reicht jetzt zusätzlich die korrekte `strategy` (SPS/BCS) an `check_exit` (behebt latente BCS-Delta-Richtung).
  - Risiko-Backstops (Stop-Loss 2×, Delta-Watchdog, Quick-Win, Delivery-Exit) bewusst erhalten — Max' drei Kriterien sind die Kern-Trigger, nicht die Streichung der Schutzmechanismen.
- Prüfung: 238 Tests grün (neue BREAK_EVEN-Tests + 60 %-Anpassungen); alle 65 %→60 %-Stellen verifiziert (nur „vorher 65 %"-Historie bleibt); IVP-65 % (Regel 018) korrekt unangetastet. Setup-/Dashboard-Templates rendern sauber.
- Betroffene Dateien: `config/settings.py`, `bot/rules.py`, `bot/manager.py`, `bot/notifier.py`, `web/templates/setup.html`, `web/templates/dashboard.html`, `CLAUDE.md`, `tests/test_rules.py`, `tests/test_notifier.py`
- Status: fertig
- Für Sepp relevant: ja — Regelwerk und Engine sind jetzt deckungsgleich (TP 60 % / BE-Bruch Prio 1 / 21-DTE hart), keine widersprüchlichen Exit-Alerts mehr.

### 2026-07-22 · Developer Agent · Neue Exit-Kriterien (Take-Profit 60 %, Break-Even Prio 1, Zeit-Exit 21 DTE)
- Was geändert (Entscheidung Max): Die Strategy Engine (Live-Management offener Spreads, alle 5 Min, nur lesend) setzt jetzt drei klare Exit-Trigger um und meldet sie per Telegram → manuell in IBKR schließen:
  - **Take-Profit 60 %**: `STRATEGY_TAKE_PROFIT_PCT` 0.50 → **0.60**. Der frühere „GTC-Limit-Reminder" (feuerte immer) ist ersetzt durch einen **echten** Alert, der erst feuert, wenn 60 % des Entry-Credits verdient sind (Rückkaufkosten ≤ 40 % des Credits). Dafür liest die Engine die aktuellen Spread-Rückkaufkosten live (neuer `spread_cost_fn`, short_ask − long_bid; fail-safe stumm ohne Quotes). GTC-Close-Limit wird als Option im Alert genannt.
  - **Break-Even-Bruch = Prio 1** (sofort schließen) — war bereits vorhanden, verifiziert.
  - **Zeit-Exit ≤ 21 DTE, hart** (unabhängig vom G/V) — war bereits vorhanden, Wortlaut geschärft.
- Web „Positionen": Profit-Target-Kachel → **Take-Profit 60 %**, Stop-Loss-Kachel → **Break-Even** (Prio-1-Hinweis), P&L-Gauge-Ziellinie 65 → 60 %, neue Exit-Trigger-Zeile je Karte. Break-Even wird serverseitig je Position berechnet.
- Prüfung: 52 Tests grün; Beispiel-Alerts als Telegram-Testnachricht an den Admin-Kanal gesendet (Zustellung bestätigt, HTTP 200). Telegram-Secrets in `/etc/optionsbot/secrets.env` gesetzt.
- Bewusste Abgrenzung: Das dokumentierte 32-Regel-Maxoman-Regelwerk (Regel 010/026 Profit-Target 65 %, Stop-Loss 2×, `bot/rules.check_exit`, `setup.html`) wurde **nicht** angefasst — die neuen Kriterien betreffen die Engine-Management-Alerts der offenen Positionen. Falls Max das Regelwerk auf 60 % harmonisieren will, separat entscheiden (Hinweis wegen möglicher doppelter Profit-Alerts aus dem älteren Monitor-Job).
- Betroffene Dateien: `config/settings.py`, `bot/strategy_engine.py`, `web/app.py`, `web/templates/positions.html`, `tests/test_strategy_engine.py`
- Status: fertig
- Für Sepp relevant: ja — Exit-Logik der offenen Positionen ist jetzt TP 60 % / BE-Bruch Prio 1 / 21-DTE-hart; Kunden-/Positionsansicht spiegelt das.

### 2026-07-21 · Developer Agent · Fix: „No security definition" bei FOP-Strikes (ES u.a.)
- Was geändert (Befund Max im Scan-Log): Bei ES (und anderen neuen FOPs) warf IB massenhaft „Error 200 – No security definition" für Put-/Call-Strikes wie 6680/6730/7970. Ursache: `reqSecDefOptParams` liefert die Strikes über ALLE Trading-Klassen gemergt (Quarterly + Weeklies); feingranulare Weekly-Strikes existieren aber unter der gewählten Quarterly-Klasse (z. B. ES = 25er-Raster) nicht. Da die neuen Symbole (ES/DAX/BZ/NG/SI/HG/PL/ZW/KC/6J) nicht in `_STRIKE_INTERVALS` stehen, leakten die ungültigen Strikes durch. **Robuster Fix statt Raster-Raten:** neue Funktion `authoritative_fop_strikes()` holt die exakt gültigen Strikes des Zielkontrakts (Expiry + Trading-Klasse) direkt via `reqContractDetails(strike=0)` — eine Abfrage je Seite, self-correcting für jedes Symbol. Scanner nutzt sie für FOP-Puts und -Calls; Aktien bleiben beim bisherigen Chain-Weg. Live verifiziert: ES-Scan jetzt **0 Fehler**, liefert gültigen Vorschlag 2× 6950/6900P (25er-Raster). Tests 231/231.
- Betroffene Dateien: `bot/scanner.py`
- Status: fertig — Randnotiz: 6J (JPY-Future) lässt sich unter CME nicht auflösen und PL/DAX finden aktuell keine 45–70-DTE-Chain → fail-closed ohne Kandidaten (kein Fehler); separat prüfen, falls diese vier gebraucht werden.
- Für Sepp relevant: nein (technischer Scan-Fix).


### 2026-07-21 · Developer Agent · Strategy Engine Live-Adapter + Scheduler-Anbindung
- Was geändert: Den in `bot/strategy_engine.py` bereits getesteten (a–g) Kern um die konkreten **lesenden IB-Reader** und den **Scheduler-Einstieg** ergänzt. `LiveMarketReader` liefert die drei von `manage_cycle` erwarteten Funktionen aus reqMktData/reqAllOpenOrders: Underlying-Preis (aus `modelGreeks.undPrice` der Short-Option), IVR (Front-Month-FOP bzw. Stock via `calc_iv_metrics`, je Symbol gecacht) und Close-Order-Erkennung (openTrades-conIds gegen die Spread-Legs) — **alles nur lesend**, der Import-Guard bleibt scharf (kein Order-Literal im Modul, verifiziert). `run_strategy_engine()` verbindet mit eigener Client-ID 25, liest Positionen als Ground Truth, matcht gegen die Kandidaten des letzten Scans (`journal/scans/`) und feuert entprellte Alerts. Scheduler-Job `job_strategy_engine` (alle 5 Min 09–16 ET, Gateway-Watchdog davor, fail-closed). Verifiziert: Suite 231/231, Import-Guard live bestanden, Scheduler-Import ok; Live-Verbindungstest bestätigt korrektes fail-closed bei (aktuell 2FA-bedingt) geschlossenem Gateway.
- Betroffene Dateien: `bot/strategy_engine.py`, `scheduler.py`, `config/settings.py` (`CLIENT_ID_ENGINE`), `CLAUDE.md`
- Status: fertig — Monitoring ist scharf, sobald das Gateway verbunden ist (2FA). Entry-Tickets (Aufgabe 1) sendet weiterhin der Scan-Job über `notify_signal`; `emit_entry_tickets` steht für eine spätere separate Ticket-Zustellung bereit, ist bewusst noch nicht in den Scan-Job verdrahtet (Doppelversand vermeiden).
- Für Sepp relevant: ja — offene Spreads werden jetzt automatisch auf Break-Even/21-DTE/TP/IVR-Kollaps überwacht (nur Hinweis, nie Order); die harte „Nie-Order"-Regel ist per Start-Guard erzwungen.


### 2026-07-21 · Developer Agent · Scanner-Umbau: Prämienband 400–500 $, Delta 10–15, EV-Top-2, Universum-Neuordnung
- Was geändert (Entscheidungen Max A–D auf die Umbau-Vorlage): **(1) Universum neu (30 Symbole):** 19 FOPs in 5 Kategorien — ES/MES/DAX (Eurex, EUR, experimentell), CL/MCL/BZ/NG, GC/MGC/SI/HG/PL, ZW/ZC/ZS/KC, 6E/6J/6B; ZN/ZB/MNQ bewusst raus (abschließende Liste Max). Aktien = Top-10-OI (NVDA TSLA AAPL AMD AMZN META MSFT GOOGL NFLX PLTR) + QQQ. Config jetzt generativ (`_FOP_SPECS` + Templates) statt 500 Zeilen Copy-Paste. **(2) Delta 10–15** (−0.125 ± 0.025, Entscheidung Max) mit konsistent nachgezogenem Effizienz-Gate 8 % (Leitprinzip Gate < Bandunterkante — sonst wieder der unerfüllbare Filter vom 16.07.); Strike-Fenster erweitert (0.72–0.97 / Calls bis 1.32). **(3) Prämienband 400–500 $** je Vorschlag mit Stückzahl-Skalierung (max. 5) für Micros/Aktien, EUR-Umrechnung für DAX; Band nicht erreichbar → fail-closed mit Funnel-Grund. **(4) Max. 2 Vorschläge je Underlying** nach EV-Score (POP-gewichtet, auf Einsatz normiert): bester konservativ (|Δ|≤0.125) + bester offensiv; Telegram/UI zeigen Stückzahl, Gesamtprämie, geplanten Stop (−2×) und POP. Scanner unterstützt jetzt Fremdwährungs-Kontrakte (currency in Future/FOP). **Live verifiziert:** CL liefert exakt das Zielformat — 2× 64.5/62.5P (Δ −0.104, POP 90 %, 440 $) und 2× 67.5/66P (Δ −0.146, POP 85 %, 500 $); Funnel zählt 37× „Prämienband nicht erreichbar" transparent; MES korrekt am IVR-Gate; NG/AAPL-Pipeline sauber; DAX wird mangels IVR-Daten fail-closed übersprungen (bekannte Limitation, VDAX-Proxy wäre Folgearbeit). Tests: 29 neue/angepasste (Suite 212/212).
- Betroffene Dateien: `config/settings.py` (Universum generativ), `bot/rules.py`, `bot/scanner.py` (Band/EV/Top-2/Currency), `bot/notifier.py`, `main.py`, `web/templates/dashboard.html`, `tests/test_premium_band.py` (neu), `tests/test_rules.py`, `CLAUDE.md`
- Status: fertig — ab dem nächsten 09:35-ET-Lauf scannt der Bot 30 Underlyings im neuen Format; Scan-Dauer steigt auf ~30–45 Min. **Dokumentierte Warnung:** Bei Delta 10–15 liegt der theoretische Maximalverlust je Vorschlag bei ~3.500–4.500 $ — der 2×-Stop (−800 bis −1000 $) ist die geplante Begrenzung, Stop-Disziplin ist systemkritisch.
- Für Sepp relevant: ja — grundlegender Strategie-Wechsel von Delta 25 auf Delta 10–15 mit uniformen Prämiengrößen; die Signal-Ausgabe ist jetzt kuratiert (max. 2/Underlying) statt roh.


### 2026-07-21 · Developer Agent · Ampel-Matrix: Seriendauer in den Zellen + „Seit gestern gekippt"
- Was geändert (Auftrag Max, Rabe-Konvention „seit wie vielen Tagen grün/rot"): Die Matrix-Zellen zeigen jetzt **Zahlen statt Icons** — bei Trend-Zeilen (Indizes/Sektoren/Rohstoffe) die vorhandene Trend-Dauer (+über/−unter MA21, Tag und Woche), bei Umfeld-Zeilen (VIX, F&G, PCR, Terminkurve, Breite, DDs) die **Seriendauer der aktuellen Farbe**, ausgezählt aus den archivierten Morgenroutine-JSONs (`color_streaks`, max. 15 Tage Rückblick; Grau beendet die Serie ehrlich). Dazu **`matrix_changes`**: alle Felder, die gegenüber dem Vortag die Farbe gewechselt haben, als gelbe Hinweis-Zeile über der Matrix („Seit gestern gekippt (n): VIX grün→gelb …") — der eigentliche Handlungs-Kontext: frische Wechsel sind anfälliger für Gegenbewegungen als etablierte Serien, viele gleichzeitige Wechsel = Regimewechsel im Gang. Einordnung an Max kommuniziert: Die Dauer „verstärkt" das Signal statistisch nicht, sie liefert Reife-Kontext (frisch vs. etabliert) und macht Veränderung sichtbar. Verifiziert: 12 neue Tests (Suite 195/195), Render live (46 Zahl-Zellen, Kipp-Zeile aktiv), Legacy-Archive + /beispiel ok.
- Betroffene Dateien: `bot/morning_routine.py` (`_umfeld_colors`, `color_streaks`, `matrix_changes`, Matrix-Refactor), `web/app.py` (Historie-Übergabe), `web/templates/morning.html`, `tests/test_matrix_streaks.py` (neu), `tests/test_ampel_matrix.py`
- Status: fertig
- Für Sepp relevant: ja — der Ampel-Überblick trägt jetzt Rabes Kern-Feature (Seriendauer) für ALLE Signale, nicht nur die Trend-Zeilen, plus automatische Kipp-Erkennung, die es im manuellen Vorbild nicht gab.


### 2026-07-21 · Developer Agent · Letzte drei Rabe-Lücken geschlossen: Wochenkalender, Rohstoff-Index, DD-Cluster
- Was geändert (Auftrag Max nach Abgleich mit dem vollständigen Rabe-Excel 2017): **(1) Termine der Woche:** neues Modul `bot/econ_calendar.py` — ForexFactory-Wochenfeed, gefiltert auf High-Impact USD/EUR, Zeiten nach Europe/Berlin; eigene Sektion in der Morgenroutine (heute live: 3 EZB-Termine am Do). **(2) Rohstoff-Index:** `^BCOM` (Bloomberg Commodity — $CRB ist bei Yahoo delisted) mit derselben Daily/Weekly-MA21-Logik als eigene Karte unter den Hauptindizes + eigene „Rohstoffe"-Gruppe in der Ampel-Matrix; bewusst getrennt von den Aktien-Breite-Signalen (fließt NICHT in DD-Bestätigung/weak_breadth). Erster Live-Wert zeigt direkt eine D/W-Divergenz (Daily +4.1 %/8T über, Weekly −0.75 %/6W unter MA21). **(3) DD-Cluster:** `dd_cluster_len` (längste Serie aufeinanderfolgender DD-Sessions aus den dd_verify-Audit-Daten); ≥ 4 in Folge = eigene Alarm-Bestätigung in `assess_dd` (auch ohne Breite-Einbruch, Rabe-Konvention) + roter Hinweis in der DD-Kachel. Heute: ES/NQ je 2 → kein Cluster, Hinweis korrekt verborgen. Verifiziert: 12 neue Tests (Suite 183/183), Live-Backfill des heutigen JSON, Render heute + Legacy-Archiv + /beispiel.
- Betroffene Dateien: `bot/econ_calendar.py` (neu), `bot/breadth_extra.py`, `bot/dd_policy.py`, `bot/morning_routine.py`, `web/templates/morning.html`, `tests/test_rabe_extras.py` (neu), `CLAUDE.md`
- Status: fertig — damit ist der Rabe-2017-Vergleich vollständig abgearbeitet (alles übernommen oder bewusst verworfen: Vol-ETPs, AMEX, Unchanged, SML)
- Für Sepp relevant: ja — die Morgenroutine deckt jetzt informationell alles ab, was das bekannteste manuelle Vorbild hatte, plus die eigenen Verifikations-/Automatisierungsschichten.


### 2026-07-21 · Developer Agent · Morgenroutine-Review: DD-Overlay, AD-Linie, Ampel-Überblick + Filter
- Was geändert (Entscheidung Max 1A/2A/3A nach Review; UI-Skill konsultiert): **Vorab-Fix (Betriebsfehler):** `ad_line.json` und `journal/scans/` gehörten root (aus manuellen Läufen) → der Dienst konnte sie nicht mehr schreiben, Fehler wurde still verschluckt (AD-Linie blieb bei 1 Eintrag, Scanner schrieb keine Archive). Per `chown` behoben. **1A:** DD-Alarm färbt die Gesamtampel nicht mehr rot (nur gelb) — Rot bleibt echtem marktweitem Stress vorbehalten (Panik/Backwardation). Die gezielte SPS-Sperre auf Aktien-Underlyings bleibt im Kompass (über dd_state, unabhängig von der Ampel). Behebt die „DD blockt alles"-Wahrnehmung (das „Gesamtlage rot"-Banner verschwindet bei reinem DD-Alarm). **2A:** kumulative AD-Linie erst ab ≥ 20 Handelstagen sichtbar (vorher „Hoch seit Aufzeichnungsbeginn" nicht aussagekräftig); Karten klar benannt: „Auf-/Absteiger heute" (Tageswert) vs. „Marktbreite-Trend (AD-Linie)" (Divergenz-Trend) — löst die Doppel-Verwechslung. **3A:** (a) neuer **Ampel-Überblick** ganz oben — kompaktes Tag/Woche-Raster aller Signale (Marktumfeld + Indizes + Sektoren) mit Grün/Gelb/Rot-Zähler, Vorbild Rabe-Excel (`build_ampel_matrix`, testbar); (b) **Strategie-Filter** „Alle · nur SPS · nur BCS" über der Underlying-Check-Tabelle (Vanilla-JS, Design-System-Komponente in base.html). Verifiziert: 30 neue Tests (Suite 171/171), Render heute + Legacy-Archiv + öffentliche /beispiel-Seite.
- Betroffene Dateien: `bot/morning_routine.py`, `web/app.py`, `web/templates/base.html`, `web/templates/morning.html`, `tests/test_ampel_matrix.py` (neu)
- Status: fertig — **Ampel-Überblick ist kundenwirksam (auch auf /beispiel): Freigabe-Sichtung durch Max empfohlen (Two-Commit-Prinzip)**
- Für Sepp relevant: ja — die Morgenroutine ist übersichtlicher (Ein-Blick-Überblick), DDs blocken nicht mehr pauschal, und die AD-Linien-Verwechslung ist aufgelöst.


### 2026-07-17 · Developer Agent · Distribution-Day-Regelwerk gestuft (Backtest-Kalibrierung)
- Was geändert (Auftrag Max, Empfehlung Developer Agent, Schwellen per 11-Jahres-Backtest quantifiziert): DD ≥ 5 auf einem Index war ein pauschaler Blocker (rote Ampel → alle 17 Underlyings + beide Strategien). Backtest-Befund: Dieses Signal allein ist nicht vom Marktdurchschnitt zu unterscheiden (21-Tage-Forward-Return nach Erst-DD≥5: Ø +0.95 % vs. Baseline +1.08 %). **Neu gestuft (`bot/dd_policy.py`):** `watch` (≥ 5 ohne Breite-Bestätigung → Ampel gelb, blockt nichts) vs. `alarm` (≥ 5 UND aktiver Breite-Einbruch: Mehrheit Indizes unter MA21, NH<NL, oder 90%-Down-Tag → Ampel rot). Bei Alarm werden nur **Short Put Spreads auf aktienkorrelierten Underlyings** pausiert (Korrelationsanalyse: MES/MNQ/QQQ/TSLA/NVDA/META/ORCL); **Bear Call Spreads** bleiben erlaubt (richtungsgleich mit Distribution), **unkorrelierte FOPs** (CL/GC/ZC/ZS/… corr≈0) bekommen nur einen Hinweis statt Block. **Removal-Schwelle** 5 % vs. 6 % entschieden: 5 % bleibt (differiert nur an 5,4 % der Tage, IBD-Standard, schneller). Gegenprobe heute: ES 5/NQ 9, 7/8 Indizes unter MA21 → alarm → 13 Aktien-SPS geblockt, BCS + Rohstoffe frei (vorher: alles rot). UI (Kompass-Hinweise, DD-Kachel mit watch/alarm), build_assessment und Telegram-Fußnote angepasst; Legacy-Archive rendern weiter. Tests: 20 neue (`tests/test_dd_policy.py`), Suite 161/161.
- Betroffene Dateien: `bot/dd_policy.py` (neu), `config/settings.py` (`DD_EQUITY_SYMBOLS`), `bot/morning_routine.py`, `bot/morning_telegram.py`, `web/templates/morning.html`, `tests/test_dd_policy.py` (neu), `TASKS_DD_POLICY.md` (neu), `CLAUDE.md`
- Status: fertig — offene Blocker-Frage „Removal 5 % vs. 6 %" damit geschlossen
- Für Sepp relevant: ja — die Morgenroutine sperrt nicht mehr pauschal bei DDs, sondern gezielt und datenbasiert; entfernt fachlich unbegründete Blocks (Öl-/Mais-Spread wegen S&P-Verteilung) und die BCS-Fehlsperre.


<!-- Format pro Eintrag:
### YYYY-MM-DD · <Agent> · <Kurztitel>
- Was geändert: …
- Betroffene Dateien: `pfad/datei.py`, …
- Status: fertig | in Arbeit | blockiert durch <X>
- Für Sepp relevant: <ja/nein + 1 Satz warum>
-->

### 2026-07-21 · Developer Agent · Status-Strip auf allen „Markt"-Seiten, deutsche Zeit (Auftrag Max)
- Was geändert: Der Status-Strip (Uhrzeit, Handelsstatus, letzter Scan, Gateway/Sync/Scan-Buttons) lag bisher nur im `topbar`-Block der Scanner-Seite. Jetzt zentral in `base.html` und über neuen Context-Processor `inject_status_strip()` (`web/app.py`) auf allen Menü-„Markt"-Seiten sichtbar (Scanner, Positionen, Journal, Morgenroutine, Methodik) — System-/Verwaltungsseiten bleiben ohne Strip. Uhrzeit jetzt **deutsche Zeit** („TT.MM.JJJJ HH:MM Uhr", Europe/Berlin) statt ET; ebenso die Anzeige des letzten Scans (`scan_time_de_fmt`). Gateway-Buttons nur für Admin (systemctl-Abfrage sonst übersprungen). Doppeltes Markup aus `dashboard.html` entfernt, Dashboard-Route um `current_time_et`/`gateway_status`/`trading_open` bereinigt. Alle 5 Markt-Seiten Test-Client 200 + Strip vorhanden, System-Seiten ohne Strip.
- Betroffene Dateien: `web/templates/base.html`, `web/templates/dashboard.html`, `web/app.py`
- Status: fertig
- Für Sepp relevant: nein — reine UI-/Frontend-Änderung, keine Logik-/Produktänderung.

### 2026-07-20 · Developer Agent · Accessibility-/UX-Feinschliff nach WIG-Review (Auftrag Max)
- Was geändert: Review aller 20 Templates gegen die Vercel Web Interface Guidelines, Befunde umgesetzt. `base.html`: globaler `:focus-visible`-Fokusring (ersetzt fehlende Sichtbarkeit bei `outline:none`-Inputs), Skip-Link zum `#main`, `aria-current="page"` am aktiven Nav-Eintrag (per JS), Hamburger mit `aria-expanded`/`aria-controls` + Fokus-Rückgabe beim Schließen, `overscroll-behavior:contain` auf der Off-Canvas-Nav, `<meta name="theme-color">`, `transition:all` → explizite Eigenschaften, Smooth-Scroll hinter `prefers-reduced-motion` gegated, Flash-Messages mit `role="alert"/"status"`. Formulare: `autocomplete="current-password"` (Admin-Login), Label-`for`/`id`-Bindungen (account/profile/admin_referrals), `spellcheck="false"` auf E-Mail/Code/Telegram-Feldern, `<fieldset>/<legend>` für die Zustellweg-Gruppe im Register, `autofocus` auf Mehrfeld-Formularen entfernt, Login-Flash kategorieabhängig. `landing.html`: `width/height` am Screenshot gegen Layout-Shift. **Nicht** übernommen (steady-alpha-ui hat Vorrang): Dark-Mode-Regeln (bewusst Light-only), englische Title-Case, `Intl.*` (bewusst deutsche Formate). Alle Templates rendern (Smoke-Test + Test-Client 200), 141 Tests grün.
- Betroffene Dateien: `web/templates/base.html`, `login.html`, `customer_login.html`, `register.html`, `account.html`, `profile.html`, `admin_referrals.html`, `landing.html`
- Status: fertig — **`landing.html` ist öffentlich → zur Sichtung durch Max (Two-Commit-Prinzip)**, Änderung ist aber rein technisch (Bildmaße, kein Text/Claim)
- Für Sepp relevant: nein — reine Frontend-Härtung, keine Logik-/Produktänderung.

### 2026-07-20 · Developer Agent · Skill „web-design-guidelines" installiert (Auftrag Max)
- Was geändert: Drittanbieter-Skill aus `vercel-labs/agent-skills` als Projekt-Skill installiert (npx/Node gibt es auf dem VPS nicht — Skill-Datei direkt aus dem Repo übernommen, inhaltlich identisch). Prüft UI-Code gegen die Vercel Web Interface Guidelines (Accessibility/UX); holt die Regeln zur Laufzeit von GitHub. In CLAUDE.md dokumentiert inkl. Rangfolge: bei Konflikten gilt weiterhin `steady-alpha-ui` (Design-System).
- Betroffene Dateien: `.claude/skills/web-design-guidelines/SKILL.md` (neu), `CLAUDE.md`
- Status: fertig
- Für Sepp relevant: nein — Werkzeug für UI-Reviews, keine Produktänderung.

### 2026-07-20 · Developer Agent · Netto-Theta: BS-Eigenberechnung statt IB-Model-Greeks (Auftrag Max)
- Was geändert: Befund vom Scan 17.07. bestätigt — IB-Model-Greeks je Strike sind off-hours inkonsistent, das Netto-Theta (kleine Differenz zweier ~$40-Werte) wurde Rauschen ($5.20/$1.90/$0.58 für praktisch identische /CL-Spreads statt korrekt ~$2.20). Neues Modul `bot/bs_theta.py`: IV je Leg aus dem Quote-Mid impliziert (Bisektion, reine Stdlib), Theta als numerischer 1-Tages-Zeitwertverfall, Black-76 für FOPs / Black-Scholes für Aktien. Scanner (beide Spread-Builder) und Positions-Manager nutzen das als primäre Quelle; IB-Model-Greeks bleiben Fallback bei fehlenden Quotes/unlösbarer IV. Toleranz ±10–20 % lt. Max akzeptiert. Regressionstest mit den echten Quotes des 17.07.-Laufs: alle drei Spreads jetzt $2.17–2.29 (glatt), Theta-Quality-Note feuert konsistent. 16 neue Tests, gesamt 141 grün.
- Betroffene Dateien: `bot/bs_theta.py` (neu), `bot/scanner.py`, `bot/manager.py`, `tests/test_bs_theta.py` (neu)
- Status: fertig
- Für Sepp relevant: ja — Theta-Werte in Scanner-Signalen und auf der Positionen-Seite sind jetzt belastbar; das Prio-2-Theta-Kriterium bewertet Kandidaten nicht mehr auf Basis von Off-Hours-Rauschen.

### 2026-07-20 · Developer Agent · Positionen-Seite zeigt Netto-Theta in $/Tag (Auftrag Max)
- Was geändert: Der Manager erfasst bei jedem Monitor-Lauf jetzt zusätzlich das Model-Theta beider Legs und speichert das Netto-Theta der Position (Konvention wie Scanner: |Θshort| − |Θlong|, × Multiplikator × Stückzahl) mit Zeitstempel im Journal (`net_theta_usd`, `theta_updated`). Die Positionen-Seite zeigt den Wert im Karten-Kopf neben „Breite" („Θ +X.XX $/Tag", Tooltip mit Stand); fehlt der Wert noch (Altbestand / Greeks nicht verfügbar), steht dort „—" — kein Crash, letzter Stand bleibt bei Greeks-Ausfall erhalten. 3 neue Journal-Tests (inkl. Altbestand ohne Theta-Felder), 125 Tests grün.
- Betroffene Dateien: `bot/manager.py`, `bot/rules.py`, `journal/trades.py`, `web/templates/positions.html`, `tests/test_journal.py`
- Status: fertig — Wert erscheint nach dem nächsten Monitor-Lauf (11:35/13:35/15:35 ET)
- Für Sepp relevant: ja — Positions-Monitoring liefert jetzt auch Theta; Grundlage für spätere Theta-basierte Auswertungen.

### 2026-07-17 · Developer Agent · Profit-Target 60 % → 65 % (Entscheidung Max, Monte-Carlo-Analyse)
- Was geändert (Auftrag Max): Profit-Target-Exit von 60 % auf 65 % der eingenommenen Prämie angehoben — für alle 17 Underlyings (`profit_target` + `bcs_profit_target`) und den globalen Fallback `PROFIT_TARGET_PCT`. Grundlage: Monte-Carlo-Analyse (100k Pfade, Regelwerk-Parameter inkl. 2×-Stop, Jump-/IV-Spike-Szenarien, Transaktionskosten): Optimum-Plateau 60–75 % Rendite je Kapitaltag, 65 % bester Kompromiss aus Rendite/Kapitaltag, Win-Rate (~82 %) und Tail-Risiko; Targets ≤ 50 % kosten messbar Rendite (Zeitwertverfall bei OTM-Spreads ist hinten-lastig). Alle Text-/UI-Referenzen mitgezogen (Regel-Erklärungen 002/010/016/026/029/030, Setup-/Positions-/Dashboard-Seiten inkl. Fortschrittsbalken-Marker), Test angepasst (Schwelle 1.20 × 0.35 = 0.42). 122 Tests grün.
- Betroffene Dateien: `config/settings.py`, `bot/notifier.py`, `web/templates/positions.html`, `web/templates/setup.html`, `web/templates/dashboard.html`, `tests/test_rules.py`, `tests/test_notifier.py`, `CLAUDE.md`, `TASKS_SCANNER_KALIBRIERUNG.md`, `.claude/agents/optionsstratege.md`
- Status: fertig
- Für Sepp relevant: ja — Kernparameter des Exit-Regelwerks geändert (Regeln 010/026); Manager alarmiert ab sofort erst bei ≥ 65 % realisiertem Gewinn.

### 2026-07-16 · Developer Agent · AGB vervollständigt (Anschrift, 19 % USt, Kündigungsfrist)
- Was geändert (Auftrag Max): Alle AGB-Platzhalter aufgelöst. § 1: Anbieter = Neuner Productions, Maximilian Neuner, Tölzer Straße 39c, 82031 Grünwald (wie Impressum). § 5: Preise als Endpreise **inkl. gesetzlicher USt (derzeit 19 %)** — Max ist Regelbesteuerer; B2C-konforme Bruttopreisangabe. § 6: Kündigung wie empfohlen — jederzeit zum Ende des laufenden Abrechnungszeitraums (monatlich: Monatsende, jährlich: Ende des Vertragsjahres). **Autonome Entscheidung (dokumentiert):** Zielmarkt-Platzhalter in § 1 auf „Verbraucher mit Wohnsitz in Deutschland" festgelegt (konservativ; AT/CH-Erweiterung jederzeit möglich, Max im Bericht darauf hingewiesen). Entwurfsbanner entfernt. Render-Smoke-Test grün.
- Betroffene Dateien: `web/templates/agb.html`
- Status: fertig — **öffentliche Rechtsseite, Sichtung durch Max empfohlen (Two-Commit-Prinzip)**; anwaltliche Prüfung bleibt offener TODO-Punkt
- Für Sepp relevant: ja — AGB sind inhaltlich vollständig; bei den Rechtstexten fehlt nur noch die anwaltliche Gesamtprüfung.

### 2026-07-17 · Developer Agent · Scanner-Universum wiederhergestellt (17 Underlyings)
- Was geändert (Auftrag Max: „alle bisher eingegebenen Underlyings wieder scannen"): Die Universum-Verschlankung vom 06.07. (Commit 7d38320, 16→7 Symbole) ist aufgehoben. **Wieder aktiv: MES, MNQ, MCL, MGC, ZC, ZS, ZN, ZB, 6E, 6B** — zusammen mit CL/GC + TSLA/NVDA/META/ORCL/QQQ jetzt 17 Underlyings. Wichtig: Die alten Configs hatten die inzwischen als unerfüllbar erkannte Parametrierung (Delta −0.18 bei 25 %-Gate) — alle wiederhergestellten FOPs wurden auf den **kalibrierten FOP-Standard** gehoben (Delta −0.25 ± 0.05, Effizienz 0.20, DTE 45–70, IVR 40 einheitlich); Struktur-Parameter (Spread-Breiten, Delivery-Exits, Multiplikatoren) aus der Original-Config übernommen. Sektor-Gruppen (max. 1 Position je Sektor) wieder vollständig: Equity Indices, Energy, Metals, Agriculture, Fixed Income, Forex, Technology, Index-ETF. Verifiziert: Konsistenz-Check (ASSET_CONFIG=FUTURES_CONFIG=SECTORS=UNIVERSE), Suite 122/122 (Regressionstest „Gate < |Delta|" deckt automatisch alle 17 ab), Live-Smoke ZN/MES/ZC während der Handelszeit (Pipeline sauber: ZN besteht IVR+Expiration, MES korrekt am IVR-Gate geblockt, ZC korrekt am Trend-Filter; Options-Underlying-Logik greift auch bei CBOT-Kontrakten). Dienst neu gestartet — ab Montag 09:35 ET scannt der Bot alle 17.
- Betroffene Dateien: `config/settings.py`, `CLAUDE.md`
- Status: fertig — Hinweis: Scan-Dauer steigt (~15–25 Min statt ~5); Micro-Kontrakte (MCL/MGC/MES/MNQ) haben dünnere Options-Liquidität, das B/A-Gate filtert das erwartungsgemäß
- Für Sepp relevant: ja — Signalpotenzial deutlich breiter (6 Futures-Sektoren statt 2); Funnel-Beobachtung der Merkliste gilt jetzt für alle 17 Symbole.

### 2026-07-17 · Developer Agent · AD-Linie auf offizielle WSJ-Quelle umgestellt (Datenfehler behoben)
- Was geändert (Auftrag Max, „Werte müssen stimmen"): **Kritischer Datenfehler gefunden und behoben.** Advance/Decline kam bisher aus einer NASDAQ-Screener-Zeilenzählung — Live-Vergleich zeigte für die NYSE **entgegengesetztes Vorzeichen** (offiziell +300 / Screener −946); die kumulative AD-Linie und die A/D-Ampel liefen damit auf falschen Werten. Umstellung auf die **offiziell gemeldeten Advances/Declines aus dem WSJ/Barron's Markets Diary** (dieselbe Quelle, die wir schon für NH/NL nutzen — dieselben Zahlen, aus denen StockCharts sein $NYAD baut; beide Endpunkte heute HTTP 200). Vier Härtungen für die kumulative Linie: (1) **Rohwerte je Tag** in `ad_line.json` gespeichert → gesamte Linie jederzeit neu berechenbar; (2) **fail-closed-Plausibilitätsgate** (NYSE-Emittentenzahl 1.000–6.000, |Netto|≤4.000) → implausible/fehlende Tage werden als Lücke übersprungen statt als 0 verrechnet, mit Telegram-Alarm + UI-Hinweis; (3) **Divergenz-Wächter**: Screener bleibt als unabhängiger Kreuzcheck, widerspricht er dem offiziellen Vorzeichen → Telegram-Alarm (heute live ausgelöst); (4) alte AD-Datei (14 Einträge auf falschen Screener-Werten) entfernt, Linie startet sauber neu. A/D-Ampel + Advancing-% laufen ebenfalls auf den offiziellen Zahlen. Verifiziert: 6 neue Tests (Suite 122/122), Live-Abruf (NYSE 1531/1231, +300, grün), heutiges JSON korrigiert, Render-Test + Archiv-Kompatibilität.
- Betroffene Dateien: `bot/breadth_extra.py`, `bot/morning_routine.py`, `web/templates/morning.html`, `tests/test_breadth_extra.py`, `CLAUDE.md`
- Status: fertig
- Für Sepp relevant: ja — ein live wirksamer Anzeigefehler in der Morgenroutine (falsche Marktbreite-Richtung) ist behoben; die AD-Linie hat jetzt eine kanonische Quelle plus drei Schutzschichten gegen Fehlwerte.

### 2026-07-17 · Developer Agent · Sentiment-Feinheiten: F&G-Kipp-Nuance + PCR-Intraday-Maximum
- Was geändert (Auftrag Max, letzte zwei Bausteine aus dem Morgenroutine-Vergleich): **(1) Fear & Greed „Gier kippt":** `previous_close` der CNN-API wird mitgeführt (Vortag + Δ auf der Karte); ≥ 75 und ≥ 1 Punkt fallend → gelbes Flag „Gier kippt" mit Erklärtext; Extremgier > 80 ist jetzt konsequent GELB statt grün (Regel-013-Zone — Markt läuft heiß). **(2) PCR-Intraday-Maximum:** neuer Scheduler-Job (stündlich 10:50–16:50 ET) sammelt PCR-Stichproben nach `journal/morning/pcr_intraday.json` (rollierend 30 Tage, idempotent je Sample-Zeit); die Morgenroutine zeigt zusätzlich zum EoD-Wert das Tagesmaximum — bei Spike > 1.40, der sich bis Schluss beruhigt, erscheint der Hinweis „Panikspitze im Tagesverlauf" (Panik-Gate bleibt bewusst auf dem EoD-Wert). Neues Modul `bot/sentiment_extra.py` (reine Logik offline testbar). Verifiziert: 16 neue Tests (Suite 117/117), Live-Abruf (F&G 42.6, Vortag 41.7, Δ +0.9), Render-Tests (Vortagszeile sichtbar; Intraday-/Kipp-Hinweise sauber ausgeblendet solange keine Daten/kein Trigger; Alt-Archive ok). Erste Intraday-Daten laufen ab der heutigen US-Session auf.
- Betroffene Dateien: `bot/sentiment_extra.py` (neu), `bot/morning_routine.py`, `scheduler.py`, `web/templates/morning.html`, `tests/test_sentiment_extra.py` (neu), `CLAUDE.md`
- Status: fertig
- Für Sepp relevant: ja — damit ist die komplette Indikatorik des Rabe-Vergleichs (16.07.) integriert oder bewusst verworfen (Vol-ETPs, AMEX); die Morgenroutine hat keine offenen Feature-Lücken mehr aus dem Vergleich.

### 2026-07-16 · Developer Agent · Morgenroutine-Ausbau: Trend-Dauer D/W, Sektor-Rotation, 90%-Tage, AD-Linie
- Was geändert (Auftrag Max nach Vergleich mit klassischer Stillhalter-Morgenroutine; Design abgestimmt mit UI- und Marketing-Skill): Vier neue Bausteine. **(1) Trend-Dauer Daily + Weekly:** `indices_ma21` zeigt je Index zusätzlich, wie viele Tage/Wochen er ununterbrochen über (+)/unter (−) seinem MA(21) notiert (yfinance 2y, Wochen-Resampling) — bestehende Keys unverändert, Alt-Archive rendern weiter. **(2) Sektor-Rotation:** neuer Block `sectors_ma21` mit den 10 S&P-Sektor-ETFs (gleiche D/W-Tabelle) plus automatischer Einordnung („breit getragen" / „nur defensiv getragen" …). **(3) 90%-Volumen-Tage:** Up-Volume-Anteil je Börse aus dem vorhandenen WSJ-Diary-Abruf (advvolume/declvolume); 90%-Down-Tag ist neues Panikzeichen (fließt in die Gesamtampel). **(4) Kumulative AD-Linie (NYSE):** persistiert in `journal/morning/ad_line.json` (idempotent je Datum, Seed aus Archiv seit 26.06.), Hoch-Erkennung ehrlich als „seit Aufzeichnungsbeginn" benannt. Neues Modul `bot/breadth_extra.py` (Berechnung strikt von Abrufen getrennt), UI in `morning.html` mit Design-System-Komponenten + niedrigschwelligen, richtungsneutralen Erklärtexten (Marketing-Leitplanken), Sprungnav-Anker `sec-rotation`. Verifiziert: 17 neue Unit-Tests (Gesamtsuite 101/101), Live-Daten geprüft, Render-Test heute + Archiv-Tag (Rückwärtskompatibilität), Kundenmail-HTML rendert fehlerfrei mit erweitertem JSON. Heutiges JSON bereits um die neuen Blöcke ergänzt (ohne erneuten Kundenversand).
- Betroffene Dateien: `bot/breadth_extra.py` (neu), `bot/morning_routine.py`, `web/templates/morning.html`, `tests/test_breadth_extra.py` (neu), `CLAUDE.md`
- Status: fertig — Kundenmail/Telegram zeigen die neuen Blöcke bewusst noch NICHT (Two-Commit-Prinzip: erst Sichtung der Web-Ansicht durch Max, dann ggf. Aufnahme in die Mail)
- Für Sepp relevant: ja — die Morgenroutine deckt jetzt auch Trend-Persistenz (Daily/Weekly), Branchenbreite und Volumen-Kapitulation ab; Funktionsumfang übertrifft damit die bekannten manuellen Vorbilder im Markt.

### 2026-07-16 · Developer Agent · Impressum final (exakter Wortlaut Max)
- Was geändert: Impressum auf Max' finalen Wortlaut gebracht — „Tölzer Straße 39c" (ß), Abschnitt EU-Streitschlichtung/OS-Plattform entfernt, Abschnittsreihenfolge per Test verifiziert. Live geprüft (Straße gerendert, EU-Abschnitt weg). Damit ist das Impressum von Max freigegeben (er hat den Text selbst vorgegeben).
- Betroffene Dateien: `web/templates/impressum.html`
- Status: fertig
- Für Sepp relevant: ja — Impressum final und live; bei den Rechtstexten bleibt nur die anwaltliche Gesamtprüfung offen.

### 2026-07-16 · Developer Agent · Impressum mit Anbieterdaten befüllt (Auftrag Max)
- Was geändert: Impressum vervollständigt — Neuner Productions / Maximilian Neuner, Tölzer Strasse 39c, 82031 Grünwald; Kontakt info@steadyalpha.de; USt-IdNr. DE323193707; Verantwortlicher n. § 18 Abs. 2 MStV. Entwurfsbanner und alle gelben Platzhalter entfernt. Telefonnummer bewusst weggelassen (Max lieferte „xxx" — E-Mail genügt nach § 5 DDG; Kommentar im Template als Merker). Live verifiziert (HTTP 200, alle Angaben gerendert).
- Betroffene Dateien: `web/templates/impressum.html`
- Status: fertig — **öffentliche Rechtsseite, Sichtung durch Max empfohlen (Two-Commit-Prinzip)**; anwaltliche Prüfung der Rechtstexte bleibt offener TODO-Punkt
- Für Sepp relevant: ja — GoLive-Punkt „Impressum-Angaben" ist erledigt, damit fehlt bei den Rechtstexten nur noch die anwaltliche Prüfung.

### 2026-07-16 · Developer Agent · Scan-Kreuzcheck gegen Tastytrade (Max): 2 Anzeigefehler behoben
- Was geändert: Max verglich einen CL-68/66-Kandidaten mit Tastytrade — Prämie/MaxLoss/Delta stimmten nach Konvention überein (wir: konservativer Natural-Preis Short-Bid−Long-Ask; TT: Mid. TTs „Delta 0.04" = Netto-Positions-Delta, unser −0.204 = Short-Leg — rechnerisch identisch). **Zwei echte Fehler behoben:** (1) Dashboard-Kachel „Theta/Tag" zeigte Brutto-Short-Theta ($34) statt Netto-Spread-Theta (~$5, TT-Größenordnung) → zeigt jetzt `net_theta_usd`. (2) „Kurs" zeigte den Front-Month-Future (CLQ6, verfällt in 5 Tagen) statt des Kontrakts, in den die Option ausübt (CLV6) — bei Backwardation ~1.5 $ daneben. Scanner ermittelt jetzt nach der Expiration-Wahl das echte Options-Underlying (erster Future mit Verfall ≥ Options-Verfall), holt dessen Kurs und nutzt ihn für Anzeige, Strike-Bereich und Kandidaten; Front-Month bleibt bewusst Basis für Trend/MA200/IVR (durchgängige Historie), `front_price` wird mitarchiviert. Zusätzlich: Prio-2-`quality_notes` erscheinen jetzt auch auf der Dashboard-Karte (Parität zum Telegram-Signal), Tooltips korrigiert (Natural vs. Mid erklärt, IVR-Gate 40 statt 35, Netto-Delta-Hinweis). Live verifiziert: CLV6 $77.33 vs. Front $78.90, Netto-Theta $5.13. Tests: 84/84.
- Betroffene Dateien: `bot/scanner.py`, `web/templates/dashboard.html`
- Status: fertig
- Für Sepp relevant: ja — Scan-Werte sind jetzt 1:1 gegen Broker-Plattformen vergleichbar (Konventionen dokumentiert in Tooltips); Vertrauensfrage von Max („Stimmt unser Scan?") beantwortet: Kern stimmte, Anzeige korrigiert.

### 2026-07-16 · Developer Agent · Journal vollständig gegen IBKR verifiziert — alle 4 Trades `manual`
- Was geändert: Zweiter IBKR-Screenshot von Max (CL-Fills + 30-Tage-Summe). **CL 80/85 BCS (13.07.): real −249.46 USD statt +44.57** — der stale Fallback hatte aus einem Verlust einen Gewinn gemacht (Schließung Sonntagnacht-Globex, Trade-Date 13.07., Gateway am 13.07. down → Fills am 14.07. nicht mehr abrufbar; genau diese Kette ist durch Watchdog + estimate-Alarm jetzt abgesichert). **CL 88/93 (+425.20) über die Perioden-Summe verifiziert** (+366.83 € Resttag ≙ 425.20 USD beim EUR-Kurs Ende Juni). Offene CL 75/72: Entry-Credit 1.26 durch Fills bestätigt. **Alle 4 Journal-Zeilen stehen jetzt auf `pnl_source=manual`; echtes Gesamt-P&L: +102.76 USD** (ursprünglich fälschlich ausgewiesen: +723.64).
- Betroffene Dateien: `journal/trades.csv`, `journal/positions.json` (untracked), `STATUS.md`
- Status: fertig — Journal komplett IBKR-abgeglichen; künftige Schließungen kommen automatisch aus IBs CommissionReports
- Für Sepp relevant: ja — die echte Trade-Historie ist deutlich schwächer als das Journal auswies (+102.76 statt +723.64; 2 von 4 Trades Verluste). Wichtig für jede Performance-Betrachtung und ein starkes Argument für die fail-closed-Journal-Pipeline.

### 2026-07-16 · Developer Agent · NVDA-Trades mit echten IBKR-Werten korrigiert (Screenshot Max)
- Was geändert: Max lieferte das IBKR-Statement (Orders & Trades). Echte realisierte Werte inkl. Gebühren eingetragen (`pnl_source=manual`): NVDA 225/250 (01.07.) **+121.80** (statt +339.00 Bug-Wert bzw. +143.00 BS-Schätzung — Schätzung lag im Band), NVDA 210/240 (08.07.) **−194.78** (statt −85.13 aus stalem unrealisiertem Stand — der Fallback war ~110 USD zu optimistisch, genau die Schwäche, die die neue Pipeline behebt). Gesamt-P&L jetzt **+396.79 USD**. Die Leg-Rechnung (Preise ×100 − Kommissionen) geht exakt auf die IB-G&V-Werte auf — bestätigt, dass die neue `realizedPNL`-Quelle künftig identische Zahlen liefert. `positions.json` konsistent nachgezogen.
- Betroffene Dateien: `journal/trades.csv`, `journal/positions.json` (untracked), `STATUS.md`
- Status: fertig — offen nur noch die zwei CL-Altzeilen (unverified/estimate), falls Max sie ebenfalls abgleichen will
- Für Sepp relevant: ja — Journal ist jetzt gegen das IBKR-Statement abgeglichen; künftige Schließungen kommen direkt aus IBs CommissionReports.

### 2026-07-16 · Developer Agent · Journal-P&L-Pipeline repariert (Auftrag Max: „Es muss in Zukunft stimmen")
- Was geändert: Tiefenprüfung des Trade-Journals nach Max' Befund falscher Gewinne/Verluste. **Befunde:** (1) Der am 09.07. gefixte Maximalgewinn-Bug hatte eine **nicht korrigierte Altlast**: NVDA-BCS 225/250 (`c561729f`) stand mit +339.00 USD (volle Prämie, Exit-Preis 0.00 — physisch unmöglich) in der CSV. (2) P&L ignorierte durchgängig Kommissionen. (3) Der Fallback verbuchte stale unrealisierte IB-Stände als endgültiges P&L, unmarkiert. (4) STK-Positionen hätten Multiplikator 100 statt 1 bekommen (P&L ×100, latent). **Fixes:** Neue Prioritätskette beim Schließen in `bot/ib_sync.py`: ① Σ `CommissionReport.realizedPNL` der Schließ-Fills (IB-berechnet, **inkl. Gebühren** — exakteste Quelle, mit UNSET/NaN-Guard und Seiten-Filter gegen Eröffnungs-Fills), ② Schließkurse aus Fills × Multiplikator, ③ letzter unrealisierter Stand — jetzt gekennzeichnet **und mit Telegram-Hinweis an Max** zur Prüfung. Neues Feld `pnl_source` (ib_realized|fills|estimate|manual|unverified) in Position, positions.json und trades.csv; Journal-UI zeigt ≈/?-Marker mit Tooltip + Fußnote. Multiplikator jetzt sec_type-basiert (`_pnl_multiplier`, STK=1). **Historische Korrektur:** `c561729f` per Black-Scholes neu bewertet (NVDA 196.7, 51 DTE, IV 35 % → Spread-Restwert ~1.96): **+143.00 statt +339.00** (Band +81…+206); Gesamt-P&L 723.64 → **527.64 USD**. Alle Altzeilen als estimate/unverified gekennzeichnet (Backup `trades.csv.bak-2026-07-16`). Tests: 84/84 (10 neue für realizedPNL-Summierung, UNSET-Guard, Multiplikatoren).
- Betroffene Dateien: `bot/ib_sync.py`, `bot/rules.py`, `journal/trades.py`, `journal/trades.csv`, `web/templates/journal.html`, `tests/test_ib_sync_pnl.py` (neu)
- Status: fertig — Empfehlung an Max: die zwei estimate-Zeilen (NVDA +143.00 geschätzt, NVDA −85.13) einmal gegen das IBKR-Statement prüfen; Korrektur = pnl anpassen + `pnl_source=manual`
- Für Sepp relevant: ja — realisierte P&L kommen künftig direkt aus IBs CommissionReports (inkl. Gebühren); jede Schätzung ist markiert und wird Max aktiv gemeldet (fail-closed-Prinzip auch im Journal).

### 2026-07-16 · Developer Agent · Earnings-Regel 019 als Prio-2-Hinweis aktiviert (Entscheidung Max)
- Was geändert (Entscheidung Max): Earnings blocken nie, werden aber im Signal benannt — Max entscheidet selbst. Umsetzung: `fetch_next_earnings()` in `bot/scanner.py` holt den nächsten Termin via yfinance (`Ticker.calendar`, nur zukünftige Daten), wird je Aktien-Symbol einmal pro Scan abgefragt und an die Kandidaten durchgereicht; `check_entry()` schreibt bei Termin ≤ Verfall die `quality_note` „Earnings am TT.MM.JJJJ liegen in der Laufzeit" (erscheint im Telegram-Signal unter „Abstriche"). `earnings_check=True` wieder aktiv für TSLA/NVDA/META/ORCL (QQQ bleibt False, ETF). Der frühere Block-Code (Regelkonflikt mit DTE 45–70) ist damit ersetzt. End-to-End verifiziert (TSLA-Earnings 22.07. → Hinweis). Tests: 74/74.
- Betroffene Dateien: `bot/scanner.py`, `bot/rules.py`, `config/settings.py`, `tests/test_rules.py`, `CLAUDE.md`
- Status: fertig
- Für Sepp relevant: ja — der offene Punkt „Earnings-Regel vs. DTE-Fenster" ist entschieden und geschlossen (Hinweis statt Gate).

### 2026-07-16 · Developer Agent · Verifikationsscan erfolgreich: erstmals Kandidaten (10) + Signal-Dedup
- Was geändert: Voller Live-Scan (09:42 ET, alle 7 Underlyings) nach der Kalibrierung: **10 Kandidaten (9× CL, 1× QQQ) — der Scanner liefert zum ersten Mal überhaupt.** Alle neuen Mechanismen griffen live: IVR-Cross-Check (CL-Kontrakt 70.2 vs. OVX-Rank 33 → konservativ 33, Trade via IVP 83.6/Regel 018 trotzdem zulässig), GC findet erstmals eine Expiration (70 DTE, n=5-Fix), NVDA korrekt am IVR-Gate geblockt (34.9/IVP 49), quality_notes (OI unbekannt, Netto-Theta) am Kandidaten, Archiv `journal/scans/2026-07-16_1342.json`, `signal_run=true`. GC/TSLA scheitern aus legitimen Gründen (unter MA200 → Trend-Filter; GC-Effizienz 7–19.5 % = ehrliche Marktlage). Folgeproblem behoben: 9 fast identische CL-Spreads hätten 9 Telegram-Nachrichten ausgelöst → `mode_scan` sendet jetzt je Symbol+Strategie nur den Kandidaten am nächsten am Ziel-Delta (heute: CL 70/69P Δ −0.247, Ratio 29 %), Rest bleibt in Web-UI/Archiv.
- Betroffene Dateien: `main.py`, `STATUS.md`
- Status: fertig — Scanner produktiv, morgen 09:35 ET erster regulärer Lauf mit Prewarm-Watchdog
- Für Sepp relevant: ja — Abnahmekriterium des Audits erfüllt (Kandidaten an einem Tag mit Trend✓+IVR✓); die Signalkette Scan→Telegram ist jetzt scharf, Beobachtungsauftrag: eine Woche Funnel-Archiv prüfen.

### 2026-07-16 · Developer Agent · Scanner-Kalibrierung umgesetzt + Gateway-Wurzelursache gefunden (2FA)
- Was geändert (Freigabe Max; Delta-Entscheidung durch Optionsstratege): **(1) Kalibrierung** `config/settings.py`: CL/GC Delta −0.25 ±0.05 (zurück zum dokumentierten Regelwerk), Effizienz-Gate 0.25→0.20; Aktienmodul mitkalibriert (/CL-Alleinfokus aufgehoben): TSLA −0.18/15 %, NVDA −0.20/16.25 %, META u. ORCL −0.22/17.5 %, QQQ −0.25/18.3 %, B/A Aktien 0.05→0.10. **(2) Prio-1/2-Systematik in `bot/rules.py`**: OI + Netto-Theta blocken nicht mehr, sondern werden als `quality_notes` im Telegram-Signal benannt (behebt OI-Silent-Pass und Brutto-Theta-Messung); Earnings-Regel 019 explizit deaktiviert (war toter Code, Aktivierung = Regelkonflikt mit DTE 45–70). **(3) Scanner-Fixes**: GC-Chain (Front-Months 3→5, live verifiziert — einzige 45–70-DTE-Expiration hängt am 4. Future), IVR-Messung (min. 60 Bars + OVX/GVZ-Cross-Check, Divergenz > 30 → konservativ min()), Greeks-Skips im Funnel gezählt. **(4) Prio 0**: Gateway-Watchdog (`bot/gateway_watchdog.py`, Prewarm 09:15 ET, fail-closed + Telegram-Alarm), Scan-Archivierung `journal/scans/`, Liquid-Hours-Publikations-Gate (diagnostische Kandidaten nie als Signal). **(5) Wurzelursache der Gateway-Ausfälle live gefunden**: Nach Session-Abriss hängt der Login in der IBKR-**2FA** und wartet still auf Max' Handy-Freigabe — Telegram-Alarm an Max gesendet, IBC-Config um `ReloginAfterSecondFactorAuthenticationTimeout=yes` ergänzt (Backup angelegt). Tests: 71/71 grün inkl. neuem Regressionstest „Effizienz-Gate < |Delta-Ziel|".
- Betroffene Dateien: `config/settings.py`, `bot/rules.py`, `bot/scanner.py`, `bot/notifier.py`, `bot/gateway_watchdog.py` (neu), `main.py`, `scheduler.py`, `tests/test_rules.py`, `tests/test_gateway_watchdog.py` (neu), `.gitignore`, `CLAUDE.md`, `TASKS_SCANNER_KALIBRIERUNG.md`
- Status: fertig — Live-Verifikationsscan (CL+GC) wartet auf Max' 2FA-Freigabe in der IBKR-App (läuft dann automatisch); optionsbot.service muss nach Commit neu gestartet werden (erledigt der Agent)
- Für Sepp relevant: ja — der Scanner ist erstmals in einem Zustand, in dem er liefern KANN (unerfüllbarer Filter beseitigt, Gateway-Ausfälle adressiert); Abnahmekriterium des Optionsstrategen: eine Woche Funnel beobachten, bei Trend✓+IVR✓ und weiterhin 0 CL-Kandidaten ist die Greeks-Verfügbarkeit um 09:35 ET der nächste Verdächtige.

### 2026-07-16 · Optionsstratege · Scanner-Audit Woche 09.–16.07.: Flaschenhals gefunden, Kalibrierungs-Entscheidung
- Was geändert (Auftrag Max, Analyse durch Subagent `optionsstratege`, nur Bericht — kein Code): Rejection-Funnel der Woche ausgewertet. **Kernbefunde:** (1) 3 von 5 Scheduler-Scans liefen nie (IB Gateway down, ConnectionRefused 4001 am 09./13./15.07.). (2) `spread_efficiency_min=0.25` in settings.py widerspricht CLAUDE.md (20 %) und ist bei Delta-Band −0.18±0.03 mathematisch unerreichbar (Credit/Breite ≈ |Delta|) → CL kann per Konstruktion nie einen Kandidaten liefern; Aktien-`min_premium_pct` verlangt implizit sogar 27–30 %. (3) Basisrate Trend×IVR über 12 Monate: 30.7 % der Tage → Signalarmut ist Implementierungs-, kein Marktproblem. **Entscheidung:** Effizienz-Gate auf 0.20 senken (vernachlässigbar fürs C/R-Verhältnis, gleiche P(ITM)), IVR/Trend/DTE/Delta-Band bleiben Prio 1. Vollständiger Bericht inkl. Prio-1/2-Tabelle, Umsetzungs-Spec (Gateway-Watchdog, Scan-Archivierung, Liquid-Hours-Gate) und Nebenbefunden (IVR-Messung instabil, GC-Chain-Bug, toter Earnings-Code): `TASKS_SCANNER_KALIBRIERUNG.md`.
- Betroffene Dateien: `TASKS_SCANNER_KALIBRIERUNG.md` (neu), `STATUS.md`
- Status: Analyse fertig — Umsetzung blockiert durch Freigabe Max (2 offene Entscheidungen: Delta-Band settings.py vs. CLAUDE.md; Aktienmodul kalibrieren oder ruhen lassen)
- Für Sepp relevant: ja — erklärt, warum seit Wochen 0 Signale kommen (Gateway-Ausfälle + unerfüllbarer Effizienz-Filter), mit quantifizierter Erwartung nach Fix (~1–3 CL-Kandidaten an ~30 % der Handelstage).

### 2026-07-16 · Developer Agent · optionsstratege erweitert: Scanner-Audit + Kalibrierung + Prio-1/2-Regelwerk
- Was geändert (Auftrag Max): Drei Sonderaufträge im Subagenten verankert. (1) **Scanner-Audit**: kritische Prüfung von `bot/scanner.py` (Regelwerk-Treue, Datenqualität zum Scan-Zeitpunkt, Auswertung des Rejection-Funnels `skip_reason`/`spread_rejections` in `journal/scan_results.json`). (2) **Kalibrierungs-Entscheidung „zu eng geschnürt?"**: erwartete Basisrate historisch herleiten, Soll-Ist-Vergleich, Sensitivitätsanalyse je Filter, klares Urteil zu eng/richtig/zu locker — Leitsatz: ein Scanner ohne Signale ist genauso wertlos wie einer ohne Filter. (3) **Prio-1/Prio-2-Einteilung der Entry-Kriterien**: Prio 1 = Muss-Gates (alle erfüllt → Underlying tradebar), Prio 2 = Qualitätskriterien (Ranking/Sizing, blocken nicht); Exit-Regeln und Portfolio-Limits sind fest Prio 1. Agent hat für (2) und (3) Entscheidungskompetenz, Umsetzung in Code bleibt dokumentierter Developer-Agent-Schritt.
- Betroffene Dateien: `.claude/agents/optionsstratege.md`
- Status: fertig
- Für Sepp relevant: ja — daraus folgt perspektivisch eine zweistufige Entry-Prüfung (Gate + Score) in `bot/rules.py` statt der heutigen Alles-oder-nichts-Filterung; Spezifikation liefert der optionsstratege.

### 2026-07-16 · Developer Agent · Neuer Subagent „optionsstratege" (Optionsstrategie-Analyst)
- Was geändert (Auftrag Max): Projekt-Subagent `.claude/agents/optionsstratege.md` angelegt — hochspezialisierter Optionsstrategie-Analyst (Greeks 1./2. Ordnung, IV/IVR/Terminstruktur/Skew, /CL-Futures-Optionen inkl. SPAN/Delivery-Risiko, quantifizierte Trade- und Positionsanalysen, Regime-Einschätzung über die Morgenroutine-Daten). Verbindlich verankert: Maxoman-Regelwerk als Basis, keine Orders, keine Anlageberatung gegenüber Kunden (WpHG/KWG-Leitplanken analog Marketing-Skill), Vola-Erklärungen richtungsneutral. CLAUDE.md um Abschnitt „Subagenten" ergänzt.
- Betroffene Dateien: `.claude/agents/optionsstratege.md` (neu), `CLAUDE.md`
- Status: fertig
- Für Sepp relevant: ja — es gibt jetzt einen dedizierten Analyse-Agenten für Strategie-/Trade-Fragen; Aufruf über Agent-Tool (`subagent_type: optionsstratege`).

### 2026-07-16 · Developer Agent · DD-Verifier — Silent-Failure-Guards + Audit-Logging (fail-closed)
- Was geändert (Auftrag Sepp, TASKS_DD_VERIFIER.md): Der Distribution-Days-Wert läuft jetzt vor jeder Veröffentlichung durch ein Publikations-Gate. (1) **Task-1-Befund**: Es gab keinen Verifier — bei Bad-Data (None/0-Volumen, eingefrorener oder staler Feed) wurde still `count: 0` bzw. ein zu niedriger Count als plausible Zahl gerendert. (2) **Neues Modul `bot/dd_verify.py`** mit 8 fail-closed-Checks: Vollständigkeit (≥ 26 Sessions), Volumen/Close vorhanden & > 0, Frozen-Detection (Volumen + Close), Stale-Detection (US-Feiertagskalender), Plausibilitätsband 0–12, unabhängiger Cross-Check der Zählung. Bei FAIL: kein Wert (`count: None`), Telegram-Alarm an Max mit konkretem Grund, restliche Morgenroutine läuft weiter; UI zeigt „— / IN PRÜFUNG", Telegram „—". (3) **Berechnung auf IBD-Regel umgestellt**: `^GSPC`/`^IXIC` statt SPY/QQQ, neu die Removal-Regel (DD fällt raus, wenn der Index ≥ 5 % über dem Schlusskurs des DD-Tages notiert). (4) **Audit-Log** `logs/dd_audit_YYYY-MM-DD.json` je Lauf mit allen Roh-Inputs je Session — jede publizierte Zahl ist rekonstruierbar. Tests: `tests/test_dd_verify.py` 6/6, Gesamtsuite 62/62; Live-Lauf verifiziert (PASS, S&P 5 / Nasdaq 9 DDs — alter SPY/QQQ-Code zeigte 7/7, Verschiebung durch echtes Index-Volumen + Removal-Regel erwartbar).
- Betroffene Dateien: `bot/dd_verify.py` (neu), `bot/morning_routine.py`, `web/templates/morning.html`, `tests/test_dd_verify.py` (neu), `CLAUDE.md`, `TASKS_DD_VERIFIER.md`
- Status: fertig — **offene Parameter-Klärung an Max: Removal-Schwelle 5 % vs. 6 %** (implementiert: 5 % Schlusskursbasis, eine Konstante `RALLY_REMOVAL` in `bot/dd_verify.py`)
- Für Sepp relevant: ja — der kritische Silent-Failure-Fall vor Kundenauslieferung ist abgefangen (FAIL = kein Wert, kein Kunden-Versand des DD-Werts, Alarm an Max); Rückmeldung siehe TASKS_DD_VERIFIER.md.

### 2026-07-15 · Developer Agent · Website-Texte auf „Online-Buchung live" umgestellt + Widerrufs-Checkbox
- Was geändert (Auftrag Max): Alle „Buchung folgt in Kürze"-/Mailto-Platzhalter entfernt, die Website spricht jetzt durchgängig von funktionierender Online-Buchung. (1) **Profil**: Morgenroutine verlinkt auf „Abo & Rechnungen" (Online buchen / Abo verwalten) statt Mailto; Scanner bleibt „auf Anfrage" (noch nicht online buchbar); Hinweisbanner umformuliert. (2) **Landingpage**: Schritt 2 („Im Kundenkonto den Plan wählen und online bezahlen – Freischaltung sofort") und Preis-Fußzeile (Buchung online, Zahlungsabwicklung über Stripe). (3) **Rechtstexte**: AGB § 5 (2) nennt jetzt Stripe als Zahlungsdienstleister; Datenschutz Abschnitt 7 „Zahlungsabwicklung" mit dem vorbereiteten Stripe-Passus befüllt (Platzhalter für anwaltliche Prüfung bleibt); Widerrufsbelehrung: Umsetzungs-TODO durch Beschreibung der realen Checkbox ersetzt. (4) **Neu: Widerrufs-Checkbox** (§ 356 Abs. 4 BGB) in beiden Buchungsformularen auf /abo — Pflichtfeld, serverseitig in `/abo/book` geprüft; ohne Häkchen keine Checkout-Session. Offline-Suite: 24 Prüfungen grün; live verifiziert.
- Betroffene Dateien: `web/app.py`, `web/templates/{abo,profile,landing,agb,datenschutz,widerruf}.html`
- Status: fertig — **öffentliche Texte, Freigabe-Sichtung durch Max empfohlen (Two-Commit-Prinzip)**
- Für Sepp relevant: ja — Website kommuniziert jetzt konsistent die live geschaltete Buchungsstrecke; die rechtlich nötige Widerrufs-Zustimmung im Checkout ist umgesetzt (war offener GoLive-Punkt).

### 2026-07-15 · Developer Agent · Stripe-Testmodus live getestet + Webhook-Zustellung repariert (nginx)
- Was geändert: Erster echter End-to-End-Test im Stripe-Testmodus (Max, Apple Pay): Checkout, Preis-Snapshot inkl. Empfehlungsrabatt und Metadata-Zuordnung funktionierten auf Anhieb. Befund: Stripe-Webhooks kamen nie an, weil die Apex-Domain per 301 auf www umleitet und Stripe Redirects als Zustellfehler wertet. Fix in nginx: `location = /stripe/webhook` auf der Apex-Domain proxied jetzt direkt zur App (beide Domain-Varianten funktionieren); zusätzlich `X-Forwarded-Proto` in beiden steadyalpha-Proxy-Blöcken ergänzt (korrekte https-URLs via ProxyFix). Verpasste Freischaltung über die idempotenten Webhook-Handler manuell nachgezogen — Test-Abo ist lokal aktiv und korrekt verknüpft. Stripe-Keys (Test) sind eingetragen, Buchung auf /abo ist im Testmodus scharf.
- Betroffene Dateien: `/etc/nginx/sites-available/steadyalpha` (Server-Konfig, nicht im Repo), `STATUS.md`
- Status: fertig — offen: Max stellt die fehlgeschlagenen Events im Dashboard erneut zu (Rechnungs-PDF), danach Live-Keys beim GoLive
- Für Sepp relevant: ja — die komplette Zahlungsstrecke ist jetzt end-to-end im Testmodus verifiziert (Checkout → Webhook → Freischaltung); für den GoLive fehlen nur noch Live-Keys + Live-Webhook.

### 2026-07-15 · Developer Agent · Stripe-Zahlungsstrecke (Checkout, Webhook, Abo-Sync)
- Was geändert: Die Abo-Buchung ist jetzt vollständig an Stripe angebunden. (1) **Buchung** (`/abo/book`): erstellt eine Stripe-Checkout-Session (Modus Subscription, deutsch); Preis wird strikt serverseitig aus `settings.PRICING` inkl. Empfehlungsrabatt berechnet, Zahlarten wählt Stripe dynamisch nach Dashboard-Konfiguration. (2) **Webhook** (`/stripe/webhook`): legt nach Zahlung das Abo an (Plan/Preis/Rabatt-Snapshot), verbucht jede bezahlte Rechnung inkl. Rechnungs-PDF und Partner-Provision, verlängert die Laufzeit je Zahlperiode (+3 Tage Kulanz für Zahlungs-Retries), synchronisiert Kündigung/Reaktivierung/Abo-Ende — auch wenn im Stripe-Dashboard gekündigt wird — und alarmiert Max per Telegram bei fehlgeschlagenen Abbuchungen. Alle Handler idempotent (doppelt zugestellte Events erzeugen nichts doppelt). (3) **Kündigungsbutton** (§ 312k BGB) kündigt jetzt auch bei Stripe zum Periodenende (fail closed: schlägt Stripe fehl, wird lokal nichts als gekündigt markiert). (4) Datenmodell: `stripe_customer_id`/`stripe_subscription_id` + automatische Migration. Ohne konfigurierte Stripe-Keys bleibt die Buchung wie bisher sauber deaktiviert. Offline-Testsuite (22 Prüfungen, eigene Wegwerf-DB) grün, inkl. signiertem End-to-End-Webhook.
- Betroffene Dateien: `web/app.py`, `web/models.py`, `requirements.txt`
- Status: fertig — **blockiert für Live-Betrieb nur noch durch Max: Stripe-Konto-Setup (Keys eintragen, Webhook im Dashboard anlegen), Anleitung liegt Max direkt vor**
- Für Sepp relevant: ja — der letzte technische GoLive-Blocker (Zahlungsstrecke) ist gebaut; ab Eintrag der Stripe-Keys können Kunden online buchen und bezahlen.

### 2026-07-15 · Developer Agent · Landingpage: öffentliche Preissektion (PAngV)
- Was geändert (Audit-Fund: Preis war nirgends öffentlich sichtbar): Neue Sektion „Preis" auf der Landingpage (zwischen 3-Schritte-Funnel und Abgrenzung, Anker `#preis`): zwei Karten Monatlich 49,00 €/Monat und Jährlich 470,40 €/Jahr (Badge −20 %, „entspricht 39,20 € pro Monat"), Kündigungs-/Abrechnungshinweise, Fußzeile „Endpreise" + ehrlicher Hinweis „Online-Buchung folgt in Kürze" + AGB-Link. Werte kommen aus dem Flask-Context (`_plan_prices()` aus settings.PRICING, `eur`-Filter) — nichts hardcodiert, konsistent mit /abo. Marketing-Skill-konform: Preis als Fakt, keine Ergebnisversprechen, „Endpreise" statt USt-Behauptung (USt-Status = TODO Max; abo.html-Abgleich bleibt TODO 21). Responsive (einspaltig < 760 px). Live verifiziert.
- Betroffene Dateien: `web/templates/landing.html`, `web/app.py`
- Status: fertig — **öffentliche Seite, Freigabe-Sichtung durch Max empfohlen (Two-Commit-Prinzip)**
- Für Sepp relevant: ja — PAngV-Blocker vom Tisch, Preiskommunikation ist jetzt öffentlich; Kampagnen können auf steadyalpha.de/welcome#preis verlinken.

### 2026-07-15 · Developer Agent · Betriebs-Monitoring: Versand-Alarme, Retry 08:20, Heartbeat 08:35, /healthz
- Was geändert (Audit-Fund: 08:00-Zustellung war unüberwacht): (1) **Admin-Alarme (Telegram an Max)** an allen bisher stillen Versand-Fehlerpfaden: SMTP-Verbindungsausfall, nicht ladbare Kundenliste, fehlgeschlagene Einzel-Zustellungen (Mail + Kunden-Telegram) und Exceptions im Kundenversand der Routine — Alarm-Helper werfen selbst nie. (2) **Morgenroutine-Scheduler-Job** vom nackten Lambda auf `job_morning()` mit try/except + notify_error umgestellt (war der einzige Job ohne Fehler-Alarm). (3) **Retry-Job 08:20 Berlin**: holt ausgefallene Zustellungen nach (idempotent dank Doppelsendeschutz je Datum); fehlt die heutige Routine komplett, läuft sie neu. (4) **Heartbeat-Job 08:35 Berlin**: prüft heutige JSON + ob die Morgenmail alle erwarteten Empfänger erreicht hat, alarmiert sonst. (5) **`/healthz`-Endpoint** (bewusst öffentlich, nur Status + Routine-Datum) für externen Uptime-Check — Max muss nur noch z. B. UptimeRobot darauf zeigen lassen. Live getestet: Retry/Heartbeat mit Produktions-Secrets gegen den echten Tageszustand (Dedup greift, Heartbeat ok), beide Services neu gestartet, Jobs registriert, /healthz live. Suite: 56 passed.
- Betroffene Dateien: `scheduler.py`, `bot/morning_mail.py`, `bot/morning_telegram.py`, `bot/morning_routine.py`, `web/app.py`
- Status: fertig — offen nur noch der externe Uptime-Check-Account (Max, ~5 Min)
- Für Sepp relevant: ja — das Produktversprechen „zuverlässige 08:00-Zustellung" ist jetzt überwacht: stiller Ausfall wird spätestens 08:35 gemeldet, transiente Fehler heilen sich 08:20 selbst.

### 2026-07-14 · Developer Agent · Rechtstexte: AGB-Preis korrigiert, Telegram + IONOS ergänzt
- Was geändert (Audit-Funde): AGB § 5 nannte 19 €/Monat statt 49 € (sachliche Falschangabe) — korrigiert auf 49 €/Monat bzw. 470,40 €/Jahr (12 Beiträge −20 %); USt-Frage bleibt Platzhalter (hängt von Max' USt-Status ab). AGB § 2 nennt jetzt Telegram als Zustellweg; veralteter Kündigungsbutton-Platzhalter durch Verweis auf den existierenden Button ersetzt. Datenschutz § 6: IONOS SE als E-Mail-Dienstleister eingetragen (AVV-Bestätigung als Platzhalter) und Telegram-Zustellung neu ergänzt (Username/Chat-ID, /start-Opt-in, Drittland-Hinweis — fehlte, obwohl das Feature live ist; anwaltliche Prüfung als Platzhalter markiert). Live verifiziert (beide Seiten 200, alte Preisangabe weg).
- Betroffene Dateien: `web/templates/agb.html`, `web/templates/datenschutz.html`
- Status: fertig — Restplatzhalter warten auf Max + Anwalt (TODO 2/3)
- Für Sepp relevant: ja — Preis-Falschangabe vom Tisch; Telegram-Zustellung jetzt auch rechtstextlich abgebildet.

### 2026-07-14 · Developer Agent · Fix: Double-Opt-in-Bypass bei SMTP-Versandfehler
- Was geändert: Audit-Fund behoben — schlug der Versand der Bestätigungsmail bei *konfiguriertem* SMTP transient fehl (z. B. IONOS-Wackler), wurde die E-Mail-Adresse bisher ungeprüft als verifiziert markiert (UWG/DSGVO-Risiko). Jetzt: Auto-Verifizierung nur noch, wenn SMTP gar nicht konfiguriert ist (Vor-Launch-Verhalten bleibt); bei Versandfehler bleibt der Account unverifiziert, der Kunde bekommt einen Hinweis auf den Resend-Button im Profil. 3 neue Tests (tests/test_register_opt_in.py, eigene Wegwerf-DB via CUSTOMER_DB_PATH): alle drei Zweige abgedeckt — erste Tests für die Registrierungsstrecke überhaupt. Suite: 56 passed.
- Betroffene Dateien: `web/app.py`, `tests/test_register_opt_in.py` (neu)
- Status: fertig
- Für Sepp relevant: ja — rechtlich relevanter Bugfix vor der Beta; Morgenmail geht weiterhin nur an verifizierte Kunden, jetzt ohne Schlupfloch.

### 2026-07-14 · Developer Agent · Öffentlicher Beispiel-Tag /beispiel live + GoLive-Audit in TODO.md
- Was geändert: (1) Neue **bewusst öffentliche** Route `/beispiel` (kein Login): fest eingefrorener Archivtag 09.07.2026 der Morgenroutine als Marketing-Showcase — roter Tag (DD-Alarm) demonstriert den Warnwert des Systems. Setup-Kompass wird öffentlich nicht gezeigt (vor Render aus der Routine entfernt), Handlungsansagen werden zu Systemaussagen neutralisiert (inkl. Template-Fallback-Headlines und DD-Alarm-Text, die die app.py-Neutralisierung sonst umgangen hätten), Auto-Reload auf der eingefrorenen Seite deaktiviert. Landingpage-Teaser von fiktiven Grün-Werten auf die echten Archivwerte umgestellt (Security-Review: JSON enthält nur Marktdaten, kein User-Input, kein Leak-Pfad; Marketing-Review: Archiv-Kennzeichnung + regulatorisch sauber). base.html: Navigation für öffentliche Besucher (Startseite/Beispiel/Registrieren/Anmelden). (2) TODO.md komplett auf den Stand des GoLive-Audits vom 14.07. gebracht (3 Fach-Agenten: Security 30/30 Baseline, Produkt, Marketing) — neue Funde: Double-Opt-in-Bypass, AGB-Preis-Falschangabe 19 €, fehlendes Versand-Monitoring, Preis nirgends öffentlich.
- Betroffene Dateien: `web/app.py`, `config/settings.py`, `web/templates/morning.html`, `web/templates/base.html`, `web/templates/landing.html`, `web/static/beispiel-morgenroutine-2026-07-09.png` (neu), `TODO.md`
- Status: fertig (53 Tests grün, /beispiel-Smoke-Test grün) — **öffentliche Seite, Freigabe-Sichtung durch Max empfohlen (Two-Commit-Prinzip)**
- Für Sepp relevant: ja — erste öffentliche Produkt-Demo live; TODO.md ist jetzt die aktuelle GoLive-Roadmap mit den Audit-Funden vom 14.07.

### 2026-07-13 · Developer Agent · Abo-Seite + Empfehlungssystem (Stripe-ready)
- Was geändert (Auftrag Max): Neue Kundenseite `/abo` („Abo & Rechnungen"): aktueller Abo-Status, zwei Pläne (monatlich 49 €, jährlich 12 Monate abzgl. 20 % = 470,40 €), Kündigungsbutton gem. § 312k BGB (Kündigung zum Laufzeitende, Bestätigungsmail sobald SMTP aktiv), Rechnungsliste, sichtbarer Risikohinweis. Buchung ist bewusst hinter der Stripe-Konfiguration verriegelt — ohne Zahlungsanbindung keine Freischaltung, UI kommuniziert das transparent; Checkout-Einstiegspunkt und `record_paid_invoice()` (verbucht Rechnung + legt Partner-Provision an) stehen für die Stripe-Webhooks bereit. Empfehlungssystem komplett: Admin-Seite `/admin/referrals` legt Partner-Links an (`/register?ref=CODE`) mit einstellbarer Partner-Provision % und Kunden-Rabatt %, zeigt Anmeldungen und offene/ausgezahlte Provisionen je Partner (inkl. „Auszahlung markieren" und Deaktivieren). Registrierung über Link: Rabatt-Banner, dauerhafte Partner-Zuordnung am Kunden, rabattierte Preise auf der Abo-Seite (Preis-Snapshot beim Buchen). Datenmodell: neue Tabellen referral_partners/invoices/referral_commissions, Subscription um plan/price_cents/discount_pct erweitert, Customer um referral_partner_id; leichtgewichtige Migration. E2E-getestet gegen DB-Kopie (9 Testgruppen: Migration, Preise, Admin-CRUD inkl. Validierung, Registrierung via Link, Abo-Seite, Kündigung, Provision 10 % je Rechnung, Auszahlung, Deaktivierung) — alles grün.
- Betroffene Dateien: `web/models.py`, `web/app.py`, `config/settings.py`, `web/templates/abo.html` (neu), `web/templates/admin_referrals.html` (neu), `web/templates/base.html`, `web/templates/register.html`, `web/templates/no_access.html`
- Status: fertig (Buchung/Bezahlung wartet auf Stripe-Konto = TODO Max; danach Checkout + Webhooks als nächster Agent-Schritt)
- Für Sepp relevant: ja — Preismodell (49 €/Monat, −20 % jährlich) und Referral-Mechanik (Partner-% + Kunden-% pro Link) sind jetzt produktiv im Datenmodell verankert; Stripe-Strecke kann direkt andocken.

### 2026-07-13 · Developer Agent · Zustellwege: Mehrfachauswahl E-Mail + Telegram
- Was geändert (Auftrag Max): Kunden können die Morgenroutine jetzt per E-Mail **und** Telegram gleichzeitig erhalten statt entweder/oder. Registrierung und Profil zeigen Checkboxen statt Radiobuttons (Validierung: mindestens ein Zustellweg; Telegram erfordert Benutzernamen). Datenmodell: `delivery_channel` durch zwei Boolean-Spalten `delivery_email`/`delivery_telegram` ersetzt; leichtgewichtige Migration mit einmaliger Übernahme der bisherigen Wahl (Legacy-Spalte bleibt in Alt-DBs stehen, wird nicht mehr gelesen). Versandlogik angepasst: Mail an alle mit E-Mail-Zustellweg, Telegram an alle mit verknüpftem Chat; der Fallback „reiner Telegram-Kunde ohne verknüpften Chat bekommt ersatzweise Mail" bleibt erhalten (keine Zustelllücke). Admin-Detailansicht zeigt beide Zustellwege. Getestet gegen Kopie der Prod-DB: Migration/Backfill, alle Formular-Flows (beide/keiner/ohne Username/nur Mail) und die komplette Versand-Matrix (5 Fälle) — alles grün.
- Betroffene Dateien: `web/models.py`, `web/app.py`, `web/templates/register.html`, `web/templates/profile.html`, `web/templates/admin_customer_detail.html`, `bot/morning_mail.py`, `bot/morning_telegram.py`
- Status: fertig
- Für Sepp relevant: ja — Produktverbesserung am Zustell-Feature; Datenmodell-Änderung (delivery_email/delivery_telegram statt delivery_channel) betrifft künftige Auswertungen/Exports.

### 2026-07-13 · Developer Agent · Projekt-Skill steady-alpha-security angelegt
- Was geändert: Neuer Projekt-Skill `.claude/skills/steady-alpha-security/` als verbindliche Grundlage für den IT Security Agenten (rein defensiv, nur eigene Systeme). SKILL.md definiert Mandat/Scope, Schutzgüter nach Priorität, realistisches Bedrohungsmodell und Grundprinzipien (Least Privilege, Defense in Depth, fail closed, Verifizieren statt Annehmen, Öffentlichkeits-Regel für gespiegelte Dateien). Dazu drei Referenzen: dokumentierter Soll-Zustand als lebende Baseline inkl. bewusst akzeptierter Restrisiken, stack-spezifische Review-Checkliste für sicherheitsrelevante Code-Änderungen (inkl. vorab definierter Anforderungen an die kommende Stripe-Strecke) und ein Incident-Response-Plan (Eindämmen → Evidenz → Max informieren → DSGVO-Fristen). Plus ausführbares, read-only Prüfskript `scripts/baseline_check.sh`; erster Lauf gegen das Live-System: 30/30 Checks bestanden, Baseline gehalten. CLAUDE.md-Skills-Liste ergänzt.
- Betroffene Dateien: `.claude/skills/steady-alpha-security/` (neu: SKILL.md, 3 References, 1 Skript), `CLAUDE.md`
- Status: fertig
- Für Sepp relevant: ja — Security-Arbeit läuft ab jetzt über ein verbindliches, versioniertes Regelwerk mit automatisiertem Baseline-Check; Erstlauf bestätigt den Härtungsstand vom Juli-Audit.

### 2026-07-13 · Developer Agent · Projekt-Skill steady-alpha-ui angelegt
- Was geändert: Neuer Projekt-Skill `.claude/skills/steady-alpha-ui/` als verbindliche Grundlage für alle UI-/Frontend-Arbeiten (Gegenstück zum Marketing-Skill). SKILL.md kodifiziert: base.html als Single Source of Truth, zwei Seiten-Familien (App-Seiten vs. Standalone), Design-Tokens (CSS-Variablen statt Hex, Schriften-Rollen, Light Theme, lokale Assets, Bootstrap Icons statt Emojis), einheitliche Statusfarben-Semantik, Flask-Context-Regeln (Rollen-Gating, CSRF-Pflicht, Graceful degradation mit „—"), Responsive-Checkliste, Arbeitsweise inkl. Render-Smoke-Test und Two-Commit-Prinzip für öffentliche Seiten, Schnittstelle zum Marketing-Skill (Regulatorik, Vola-Texte richtungsneutral). Dazu `references/komponenten.md` mit Copy-Paste-Snippets aller base.html-Komponenten (Tacho, Ampeln, Pills, Tiles, Tabellen, Karten, Buttons/Formulare, Popover-Makro, Status-Strip, Progress) — alle referenzierten CSS-Klassen gegen base.html verifiziert. CLAUDE.md-Skills-Liste ergänzt.
- Betroffene Dateien: `.claude/skills/steady-alpha-ui/SKILL.md` (neu), `.claude/skills/steady-alpha-ui/references/komponenten.md` (neu), `CLAUDE.md`
- Status: fertig
- Für Sepp relevant: ja — der UI Agent arbeitet ab jetzt mit einem verbindlichen, versionierten Design-System-Regelwerk; Konsistenz-Prüfungen und neue Seiten laufen über diesen Skill.

### 2026-07-13 · Developer Agent · Landingpage: visueller Dashboard-Teaser
- Was geändert (Auftrag Max, Konzept mit UI- + Marketing-Agent abgestimmt): Neben der Text-Beispielkarte zeigt die Landingpage jetzt einen handgebauten CSS/SVG-Mockup des Morgenroutine-Dashboards im Browser-Fenster-Rahmen (URL-Leiste app.steadyalpha.de/morning, Badge „Beispiel", Sidebar-Andeutung, grünes Ampel-Banner mit dezentem Puls, 2 statische Tachos VIX 16,8 grün / Fear&Greed 72 gelb in exakter Geometrie des echten Gauge-Renderers, Kacheln VVIX 95 + Put/Call 0.87, angeschnittene Folgezeile). Kein Screenshot/PNG: ~7 KB Inline-Markup, retina-scharf, keine echten Accountdaten. Marketing-Fund behoben: Demo-VIX 14,6 widersprach dem eigenen Tagesfilter (VIX<15 = Prämien zu gering) → Beispielwerte auf VIX 16,8/VVIX 95 korrigiert, F&G bewusst gelb als Glaubwürdigkeits-Textur (nicht „alles grün"). Caption regulatorisch geschärft: „Beispieldarstellung mit fiktiven Werten, keine aktuelle Marktlage, keine Handlungsempfehlung". Zweispaltig ab 900px, mobil gestapelt. Desktop visuell im Browser verifiziert.
- Betroffene Dateien: `web/templates/landing.html`
- Status: fertig
- Für Sepp relevant: ja — Landingpage zeigt jetzt den „Clean Look" des Produkts (stärkster Conversion-Hebel laut Analyse); Beispielwerte sind jetzt regelwerkskonsistent.

### 2026-07-13 · Developer Agent · Setup-Kompass: Kunden sehen nur Futures + Index-ETFs
- Was geändert (Entscheidung Max): Kunden sehen im Setup-Kompass/Underlying-Check der Morgenroutine nur noch die 8 Futures/Index-ETFs (CL, GC, SI, ES, NQ, SPY, QQQ, IWM) – die 10 Einzelaktien samt Earnings-Hinweisen bleiben der Admin-Ansicht vorbehalten. Begründung: Die Routine misst das Marktumfeld, keine aktienspezifischen Risiken; eine Einstufung für Einzelaktien würde Prüftiefe suggerieren, die nicht existiert, und Earnings-Termine (yfinance) sind fehleranfällig. Neue Konstante `COMPASS_CUSTOMER_KINDS` in settings.py, Filter in der /morning-Route (greift auch im Archiv). Methodik-Seite listet jetzt transparent die beobachteten Underlyings und erklärt die Beschränkung; Event-Text dort auf EIA/FOMC reduziert. Morgenmail + Kunden-Telegram enthielten den Kompass nie – keine Änderung nötig.
- Betroffene Dateien: `config/settings.py`, `web/app.py`, `web/templates/methodik.html`
- Status: fertig
- Für Sepp relevant: ja — Produktentscheidung: Kompass-Universum für Kunden bewusst auf Futures/Index-ETFs beschränkt (Konsistenz Messung ↔ Aussage, weniger regulatorische Angriffsfläche).

### 2026-07-13 · Developer Agent · Methodik-Seite + Kunden-Navigation umgebaut
- Was geändert (Auftrag Max): Neue Kundenseite `/methodik` erklärt alle Kriterien der Morgenroutine (Gesamtampel-Logik, Vola-Schwellwerte, Terminkurve, Fear&Greed, PCR, Distribution Days, Setup-Kompass mit Trend-Filter/Tagesfreigabe/Übersteuerungen, Panik-Checkliste, Marktbreite, Indizes vs. MA21) — statisch, keine Tageswerte, Schwellwerte aus config/settings.py bzw. gespiegelt aus morning_routine.py. Vorab UI- und Marketing-Agent konsultiert: Menüname „Methodik" (Marketing-Empfehlung), Seitenstruktur spiegelt die Morgenroutine-Sektionen (UI-Empfehlung), Texte regulatorisch geprüft (Systemaussagen statt Empfehlungen, Vola richtungsneutral, Disclaimer am Seitenende). Kunden-Sidebar umgebaut: Markt (Scanner/Morgenroutine/Methodik, „Weitere Pakete" nur ohne Morning-Paket) → Konto (Mein Profil) → Footer nur noch Abmelden + Rechtslinks. Admin-Nav: Methodik unter Morgenroutine. Zugriff: Admin + jeder aktive Kunde paketunabhängig (Teaser-Funktion). Morgenroutine-Seite verlinkt dezent auf die Methodik.
- Betroffene Dateien: `web/templates/methodik.html` (neu), `web/templates/base.html`, `web/templates/morning.html`, `web/app.py`
- Status: fertig
- Für Sepp relevant: ja — Transparenz-Seite stärkt die Positionierung „regelbasiert statt Bauchgefühl" und dient als Produkt-Einblick für Kunden ohne Morning-Paket.

### 2026-07-12 · Developer Agent · Telegram-Morgenroutine: Ampel, Struktur + Tacho-Dashboard
- Was geändert (Auftrag Max): Telegram-Zustellung von Plain-Text auf Foto + strukturierte HTML-Caption umgestellt. Bild (serverseitig SVG→PNG via cairosvg): Ampel-Gehäuse mit aktiver Lampe, 7 Tachos (VIX, VVIX, Fear&Greed, Put/Call, OVX, GVZ, EUR/USD — 270°-Bogen, Farbzonen und Schwellwerte identisch zur Web-UI) + Distribution-Days-Kachel, Footer mit Disclaimer. Caption: Ampel-Emoji + Datum, Headline, Bullets, Kennzahlen-Blöcke (Vola/Sentiment/Marktbreite/DD/Panikzeichen), Link, Risikohinweis. Fallbacks: Caption > 1024 Zeichen → Bild + Folgenachricht; Bildfehler → reine Textnachricht. Testversand an Max erfolgreich, Bild visuell geprüft.
- Betroffene Dateien: `bot/morning_telegram.py`, `requirements.txt` (+cairosvg)
- Status: fertig
- Für Sepp relevant: ja — Telegram-Kanal ist jetzt optisch ein Aushängeschild („quantifiziert" sichtbar gemacht); Screenshot davon taugt später als Marketing-Material.

### 2026-07-12 · Developer Agent · Kunden-Bot @SteadyAlphaServiceBot live
- Was geändert: Token des neuen Kunden-Bots in secrets.env eingetragen (TELEGRAM_CUSTOMER_BOT_TOKEN). getMe ✓ („Steady Alpha", @SteadyAlphaServiceBot), getUpdates-Polling ✓ — Telegram-Zustellung der Morgenroutine ist damit komplett einsatzbereit (Kanalwahl → /start beim Bot → automatische Verknüpfung → täglicher Versand). Logo-PNG an Max geschickt für BotFather-Profilbild.
- Betroffene Dateien: keine im Repo (nur /etc/optionsbot/secrets.env)
- Status: fertig
- Für Sepp relevant: ja — beide Zustellkanäle (E-Mail + Telegram) sind jetzt produktiv; Launch-Kommunikation kann Telegram-Zustellung als Feature nennen.

### 2026-07-12 · Developer Agent · SMTP live: Mail-Versand über noreply@steadyalpha.de
- Was geändert: IONOS-SMTP in secrets.env konfiguriert (smtp.ionos.de, Port 587/STARTTLS statt 465 — Hetzner sperrt 465 ausgehend; Absender „Steady Alpha <noreply@steadyalpha.de>"). Testmail an Max erfolgreich versendet. Damit automatisch scharf: Double-Opt-in-Bestätigungsmail bei Registrierung + tägliche Morgenmail an verifizierte Kunden mit E-Mail-Kanal. SPF/DKIM/DMARC via IONOS-Records der Domain vorhanden.
- Betroffene Dateien: keine im Repo (nur /etc/optionsbot/secrets.env)
- Status: fertig
- Für Sepp relevant: ja — GoLive-Blocker Nr. 1 (SMTP) ist gefallen; kritischer Pfad jetzt: Rechtstexte final + Stripe.

### 2026-07-12 · Developer Agent · Umzug auf www.steadyalpha.de (vorbereitet, wartet auf DNS)
- Was geändert: Max' neue Domain www.steadyalpha.de serverseitig komplett vorbereitet: nginx-Site für steadyalpha.de + www (ACME-fähig, Apex → www-Redirect), BASE_URL-Default auf https://www.steadyalpha.de (E-Mail-/Telegram-Links), AGB + Impressum-Platzhalter auf neue Domain. Alte Domain optionbot.neuner-productions.de bleibt parallel aktiv.
- Blocker: DNS zeigt noch auf IONOS-Parking. Max setzt: A `@` → 178.104.116.83, A `www` → 178.104.116.83, AAAA löschen oder → 2a01:4f8:1c18:573c::1. Ein Hintergrund-Watcher (12 h) prüft alle 5 Min und stellt dann automatisch das Let's-Encrypt-Zertifikat aus (inkl. HTTPS-Redirect + Telegram-Meldung an Max).
- Betroffene Dateien: `config/settings.py`, `web/templates/agb.html`, `web/templates/impressum.html`, `TODO.md`; außerhalb Repo: `/etc/nginx/sites-available/steadyalpha`
- Status: **fertig — www.steadyalpha.de ist live.** Max hat die DNS-Records gesetzt (A+AAAA für @ und www), Let's-Encrypt-Zertifikat für beide Namen ausgestellt (gültig bis 2026-10-10, Auto-Renewal via certbot.timer), HTTPS-Redirects aktiv (http→https, apex→www), Security-Header (HSTS etc.) gesetzt. Komplette Redirect-Kette verifiziert. Hinweis: Alte apex-Records (IONOS) können bei manchen Resolvern noch bis TTL-Ablauf (~1 h) nachhängen; www war nie anders belegt und geht sofort überall.
- Für Sepp relevant: ja — offizielle Kundendomain ab jetzt https://www.steadyalpha.de; alle künftigen Links/Marketing-Texte darauf ausrichten.

### 2026-07-12 · Developer Agent · Zustellkanal-Wahl: Morgenroutine per E-Mail oder Telegram
- Was geändert (Auftrag Max): Kunden wählen bei Registrierung und im Profil, ob die Morgenroutine per E-Mail oder Telegram kommt; bei Telegram ist der Benutzername Pflichtfeld.
  1. **Datenmodell**: `delivery_channel` (email|telegram), `telegram_username`, `telegram_chat_id` + leichtgewichtige SQLite-Migration (ALTER TABLE bei Start).
  2. **UI**: Registrierung mit Kanalwahl (Radio + Username-Feld, JS-Toggle); Profil-Karte „Zustellung der Morgenroutine" mit Verknüpfungsstatus und t.me-Link zum Bot.
  3. **Telegram-Versand** (`bot/morning_telegram.py`, neu): Bots dürfen Nutzern erst nach /start schreiben → zweistufig: getUpdates-Polling verknüpft Chats per Username-Match (persistenter Offset + Chat-Cache in data/telegram_updates.json, robust auch wenn /start vor Username-Eingabe passiert), danach täglicher Versand der Tageszusammenfassung (Doppelsendeschutz journal/morning/telegram_log.json). Eingehängt in morning_routine nach dem Mailversand.
  4. **Mail-Fallback**: Telegram-Kunden ohne verknüpften Chat bekommen weiter die E-Mail — keine Zustelllücke.
  5. Nebenbei gefixt: Profilseite zeigte das Scanner-Paket als buchbar (Leck aus „Scanner verbergen"); Admin-Kundendetail zeigt jetzt Zustellkanal + Verknüpfungsstatus.
- Betroffene Dateien: `web/models.py`, `web/app.py`, `web/templates/profile.html`, `register.html`, `admin_customer_detail.html`, `bot/morning_telegram.py` (neu), `bot/morning_mail.py`, `bot/morning_routine.py`
- Status: fertig — Registrierung/Profil/Fallback end-to-end getestet (Flask-Test-Client, DB-Kopie); Telegram-getMe live ok (@maxoptions1bot)
- Für Sepp relevant: ja — neuer Zustellkanal betrifft Launch-Kommunikation; Entscheidung offen, ob vor Launch ein eigener Kunden-Bot (z. B. @SteadyAlphaBot) statt des Admin-Bots angelegt wird (Bot-Wechsel = Kunden müssten neu verknüpfen).

### 2026-07-12 · Marketing Agent · Datenhinweis „ohne Gewähr" an allen Konsumpunkten
- Was geändert (Nachfrage Max): Hinweis, dass Kennzahlen automatisiert aus externen Quellen stammen, auf Konsistenz geprüft werden, aber ohne Gewähr für Richtigkeit/Vollständigkeit/Aktualität sind. War bereits in AGB §7 + Impressum vorhanden; jetzt zusätzlich dort, wo Kunden Werte täglich lesen: Landingpage-Risikokasten, Datenhinweis-Box am Ende der Morgenroutine-Seite (mit AGB-§7-Link), Morgenmail (HTML-Footer + Plain-Text).
- Betroffene Dateien: `web/templates/landing.html`, `web/templates/morning.html`, `bot/morning_mail.py`
- Status: fertig (live)
- Für Sepp relevant: ja — Haftungs-/Erwartungsmanagement jetzt konsistent über alle Kundenkontaktpunkte.

### 2026-07-12 · Marketing Agent · Landingpage-Relaunch + neues Logo (Ampel-Marke)
- Was geändert (Auftrag Max, Basis: Skill `steady-alpha-marketing`):
  1. **Landingpage komplett neu**: Hero mit USP („Die Morgenroutine, die sich selbst rechnet."), Beispiel-Karte der Gesamt-Ampel (klar als Beispiel markiert), Problem→Lösung (manuelle Checkliste vs. System), 3 Kernbotschaften in Skill-Priorität, Feature-Grid (6 Bereiche der Morgenroutine), 3-Schritte-Funnel, Abgrenzungs-Block („Kein Coaching, keine heißen Tipps, kein Depot zum Nachhandeln"), sichtbarer Risikohinweis-Kasten mit AGB-Link (nicht nur Footer), Meta-Description für SEO.
  2. **Neues Logo**: Ampel-Marke (dunkles Gehäuse, grüne Lampe aktiv) ersetzt den Chart-Pfeil (Trading-Kitsch + richtungs-suggestiv, beides laut Skill/Feedback unerwünscht) und das „SA"-Kürzel auf allen Seiten (Landing, Login, Registrierung, Rechtsseiten, App).
  3. **Tagline** vereinheitlicht: „Die automatisierte Morgenroutine für Optionsverkäufer".
  4. Regulatorik geprüft: keine Rendite-/Ergebnisversprechen, Ampel als Markteinschätzung geframt, Schritt 2 ehrlich („Online-Buchung folgt"), Beispieldaten doppelt gekennzeichnet.
- Betroffene Dateien: `web/templates/landing.html`, `base.html`, `register.html`, `customer_login.html`, `legal_base.html`, `web/static/favicon.svg`
- Status: fertig (live) — visuelle Endkontrolle durch Max empfohlen; Social/Posting-Texte folgen als separater Auftrag
- Für Sepp relevant: ja — erste Umsetzung der Marketing-Skill auf der Webseite; Positionierung/Copy jetzt verbindlich sichtbar, Feedback zu Headline/Tonalität willkommen.

### 2026-07-12 · Developer Agent · Scanner-Produkt kundenseitig verborgen (Launch nur mit Morgenroutine)
- Was geändert (Auftrag Max): Kunden sollen vorerst nicht sehen, dass der Scanner als Produkt geplant ist. Alle kundensichtbaren Erwähnungen entfernt/neutralisiert: Landingpage (Titel, Hero-Text, Scanner-Paketkarte raus, Grid einspaltig), Registrierungs-/Login-Footer, Paketübersicht (Scanner-Karte nur noch sichtbar, wenn Paket bereits gebucht), AGB §2 + §5 (Scanner-Paket + Preis-Platzhalter raus), Impressum-Hinweis, Morgenroutine-Tooltip („Scanner-Filter" → „Markt-Filter"). Admin-Bereich und paket-gebundene Scanner-Seite unverändert; Kunden mit gebuchtem Scanner-Paket behalten Zugang.
- Betroffene Dateien: `web/templates/landing.html`, `register.html`, `customer_login.html`, `no_access.html`, `agb.html`, `impressum.html`, `morning.html`
- Status: fertig
- Für Sepp relevant: ja — Launch-Kommunikation läuft bis auf Weiteres ausschließlich über die Morgenroutine; Scanner bleibt internes/kommendes Produkt (betrifft auch AGB-/Preis-TODOs: Scanner-Preis vorerst nicht mehr nötig).

### 2026-07-12 · Developer Agent · Skills-Verzeichnis eingeführt + Marketing-Skill installiert
- Was geändert: Standard-Konvention `.claude/skills/` für Claude-Code-Skills angelegt; erste Skill `steady-alpha-marketing` installiert (Positionierung/USP, Zielgruppe, Tonalität, regulatorische Leitplanken WpHG/KWG, Channel-Guidance, Agenten-Workflow). CLAUDE.md um Skills-Abschnitt ergänzt.
- Betroffene Dateien: `.claude/skills/steady-alpha-marketing/SKILL.md`, `CLAUDE.md`
- Status: fertig
- Für Sepp relevant: ja — die Skill ist ab jetzt die verbindliche Grundlage für alle Marketing-/Werbetexte mit Kunden-Außenwirkung; weitere Skills folgen derselben Konvention.

### 2026-07-10 · Developer Agent · GoLive-Vorbereitung: Backups, lokale Fonts, Double-Opt-in
- Was geändert (GoLive-Roadmap Punkte 3–5, beauftragt von Max):
  1. **Backups (neu!)**: Es gab bisher KEIN Backup der Kundendaten. Jetzt täglich
     03:30 via systemd-Timer `optionsbot-backup.timer`: `scripts/backup.py` sichert
     Kunden-DB (SQLite-Backup-API, WAL-fest), journal/ und Admin-Passwort-Hash als
     tar.gz nach backups/ (0600, 30 Tage Retention), Telegram-Alarm bei Fehlschlag.
     Restore getestet (DB + CSV aus Archiv gelesen). **Offen für Max:** Offsite-Kopie
     – BACKUP_REMOTE in secrets.env setzen (Vorlage da, z.B. Hetzner Storage Box),
     lokales Backup schützt nicht vor Serververlust.
  2. **Fonts/CDN lokal**: Google Fonts (16 woff2), Bootstrap CSS/JS und Icons
     liegen jetzt unter web/static/vendor/ – kein einziger Drittanbieter-Request
     mehr (Abmahnrisiko Google Fonts beseitigt). Alle 6 Templates umgestellt,
     Datenschutz §8 entsprechend vereinfacht.
  3. **Double-Opt-in**: Registrierung sendet Bestätigungslink (stateless
     HMAC-Token, 48h gültig, kein DB-Umbau); /verify/<token>-Route; Resend-Button
     im Profil (rate-limitiert); Morgenmail geht nur noch an verifizierte Kunden.
     Ohne SMTP-Konfiguration wird direkt verifiziert (Vor-Launch-Verhalten).
     Bestandskunde migriert. Gemeinsames Versand-Modul `bot/mailer.py`
     (morning_mail refaktoriert). E2E getestet inkl. Token-Manipulation.
- Betroffene Dateien: `scripts/backup.py` (neu), `bot/mailer.py` (neu),
  `bot/morning_mail.py`, `web/app.py`, `web/templates/profile.html`,
  `web/templates/datenschutz.html`, 6 Templates (Vendor-Assets),
  `web/static/vendor/` (neu), systemd: `optionsbot-backup.{service,timer}`
- Status: fertig · GoLive-Roadmap: als Nächstes Stripe-Zahlungsstrecke (Punkt 7/8)
- Für Sepp relevant: ja — Backup-Timer läuft auf dem VPS, Mailversand jetzt
  zentral in bot/mailer.py, Morgenmail-Empfänger = aktiv + morning-Paket + verifiziert.

### 2026-07-09 · Developer Agent · Rechtstexte: Impressum, Datenschutz, AGB, Widerruf (Entwurf)
- Was geändert: Vier öffentliche Seiten (ohne Login erreichbar, noindex):
  `/impressum` (§ 5 DDG + § 18 MStV + Keine-Anlageberatung-Hinweis),
  `/datenschutz` (DSGVO: Hetzner-Hosting, Server-Logs, Session-Cookies,
  Kundenkonto, Morgenmail, Google-Fonts/CDN-Hinweis, Betroffenenrechte),
  `/agb` (Informationsdienst ≠ Anlageberatung, Risikohinweis, Pakete/Preise,
  Laufzeit, Haftungsklausel), `/widerruf` (Muster-Belehrung + Formular,
  Erlöschens-Hinweis für sofortigen Leistungsbeginn).
  Fehlende Angaben sind farblich markiert (gelb/rot, 28 Platzhalter) –
  Warnbanner „Entwurf" auf jeder Seite. Verlinkt: Landing-Footer,
  Sidebar-Footer (base.html), Registrierung (AGB/Datenschutz-Hinweis beim
  Absenden), Morgenmail-Footer. Neues Layout `legal_base.html`.
- Betroffene Dateien: `web/app.py`, `web/templates/legal_base.html` (neu),
  `impressum.html`/`datenschutz.html`/`agb.html`/`widerruf.html` (neu),
  `landing.html`, `base.html`, `register.html`, `bot/morning_mail.py`
- Status: fertig als Entwurf – **Platzhalter + Anwaltsprüfung durch Max offen**
- Für Sepp relevant: ja — Rechtsseiten existieren jetzt; vor Stripe-Anbindung
  müssen Kündigungsbutton (§ 312k BGB) und Widerrufs-Checkbox umgesetzt werden.

### 2026-07-09 · Developer Agent · Journal-Bug: manuell geschlossene Trades wurden immer als Maximalgewinn verbucht
- Was geändert (Fund Max: NVDA-Trade als +494 $ Gewinn verbucht, war real ein Verlust):
  Beim IB-Sync wurden verschwundene Positionen mit Schließkosten 0,0 geschlossen →
  P&L = volle Prämie, egal wie der Trade wirklich ausging. Fix in `bot/ib_sync.py`:
  1. Schlusskurs wird jetzt aus den **heutigen IB-Fills** ermittelt (mengengewichtet
     je Leg: Short-Rückkauf BOT, Long-Verkauf SLD) → exaktes P&L.
  2. Fallback (Fills nicht mehr verfügbar, z.B. Schließung nach Sync-Ende):
     **letzter unrealisierter P&L** als Schätzung, im Log gekennzeichnet.
  3. `close_position()` akzeptiert `pnl_override` – die alte Formel gilt nur noch
     für Credit-Spreads mit positiver Stückzahl (Singles mit Vorzeichen-Quantity
     wären sonst vorzeichenverkehrt).
  - Datenkorrektur: NVDA e5a932bc (BCS 210/240, 07.–08.07.) von +494,00 auf
    **−85,13 USD** korrigiert (letzter unrealisierter Stand 68 Min. vor Schließung),
    exit_price impliziert 5,79. **Offen:** NVDA c561729f (QUICK_WIN, 01.07.,
    +339 = volle Prämie) ist nach demselben Muster überzeichnet – echter
    Schlusskurs unbekannt, Max müsste den Wert aus IBKR nachtragen.
  - 2 neue Unit-Tests (pnl_override, Cleanup-Ausnahme), Suite: 53 passed.
- Betroffene Dateien: `bot/ib_sync.py`, `journal/trades.py`, `tests/test_journal.py`
- Status: fertig (bis auf Nachtrag c561729f durch Max)
- Für Sepp relevant: ja — alle bisherigen „manuell geschlossen"-P&Ls waren
  Maximalgewinn-Fantasiewerte; ab jetzt Fills-basiert bzw. konservativ geschätzt.

### 2026-07-09 · Developer Agent · Setup-Kompass: rote Gesamtampel übersteuert Tagesfreigabe (Kunden-Feedback Max)
- Was geändert: Max' Einwand – bei roter Gesamtampel zeigten die SPS/BCS-Karten
  weiter grün „Heute möglich" und Underlyings „heute handelbar". Jetzt konsistent:
  1. Neuer Verdict-Status `blocked`: rote Gesamtlage übersteuert jedes „go" im
     Setup-Kompass („Kein Neueinstieg – Gesamtampel steht auf Rot", rot im
     Underlying-Check). Gesamtampel wird dafür VOR dem Kompass berechnet.
  2. Gate-Karten: bei roter Ampel rote Pille „Gesamtlage rot – gesperrt" statt
     grün „Heute möglich" + Erklärzeile in der Karte.
  3. Sichtbare Erklär-Box unter den Karten: Warum können SPS und BCS gleichzeitig
     „möglich" sein? (Tagesfreigabe prüft nur marktweite Extreme, die RICHTUNG
     entscheidet der Trend je Underlying: Gold abwärts → BCS, Aktie aufwärts → SPS.)
     Auch im Tooltip ergänzt.
  - build_assessment zählt `blocked` als „wäre technisch offen" (Banner-Bullet).
    Ältere Archiv-JSONs (nur „go") bleiben lesbar.
- Betroffene Dateien: `bot/morning_routine.py`, `web/templates/morning.html`
- Status: fertig
- Für Sepp relevant: ja — Regelklarstellung: Gesamtampel rot = keine Neueinstiege,
  übersteuert Tagesfreigabe und Trend; neuer Verdict-Wert `blocked` im JSON.

### 2026-07-09 · Developer Agent · Kunden-Delivery: tägliche Morgenroutine-E-Mail
- Was geändert: Neues Modul `bot/morning_mail.py` – nach jedem Morgenroutine-Lauf
  geht eine kompakte E-Mail an alle aktiven Kunden mit Morning-Paket: Ampelfarbe
  als Header, Headline, „Das Wichtigste heute"-Bullets, Kennzahlen-Zeile,
  CTA-Button zur vollständigen Analyse (BASE_URL/morning), Footer mit
  Paket-Hinweis und Risiko-Disclaimer (keine Anlageberatung). HTML + Plain-Text.
  - Doppelsendeschutz je Datum (journal/morning/mail_log.json) – manuelle
    Re-Runs der Routine lösen keine zweite Mail aus; ein Batch nutzt EINE
    SMTP-Verbindung (skaliert auf 1000 Empfänger), Fehler je Empfänger geloggt.
  - Konfiguration über SMTP_* in secrets.env (Vorlage eingetragen); ohne
    Konfiguration wird der Versand mit Warnung übersprungen. BASE_URL neu in
    settings.py (Default: https://optionbot.neuner-productions.de).
  - Test-CLI: `python -m bot.morning_mail --test adresse@example.com`.
  - End-to-End getestet gegen lokalen Debug-SMTP (aiosmtpd): Kundenliste aus
    SQLite, Dedup, Force-Test, HTML/Text-Aufbau, mail_log – alles verifiziert.
- Betroffene Dateien: `bot/morning_mail.py` (neu), `bot/morning_routine.py`,
  `config/settings.py`, `CLAUDE.md`
- Status: Code fertig – **wartet auf SMTP-Zugang von Max** (siehe Blocker oben)
- Für Sepp relevant: ja — Delivery-Entscheidung gefallen (E-Mail), Blocker ist
  nur noch der SMTP-Account.

### 2026-07-09 · Developer Agent · Kundensicht-Ausbau: EUR/USD-Vola statt totem EVZ, TL;DR-Bullets, Sprungnavigation
- Was geändert (Fokus 19-€-Massenkunde, Schwächen aus dem Kunden-Check gelöst):
  1. **Toter EVZ-Index ersetzt**: CBOE hat den EVZ Anfang 2025 eingestellt – die Seite
     zeigte einen 1,5 Jahre alten, eingefrorenen Wert (7.51). Neu: selbst berechnete
     realisierte 30-Tage-Vola des EUR/USD (annualisiert, mit echtem 1-Jahres-Perzentil,
     heute 4.9% / 38. Pzt.). Damit hat jetzt jede Vola-Kachel ein Perzentil.
     Schlüssel bleibt `evz`; Kachel, Tooltip (erklärt die Umstellung ehrlich) und
     Sektor-Lage (<8 grün / >12 rot) konsistent umgestellt. FRED/stooq/CBOE-Historie
     als Quellen geprüft: alle blockiert/tot → eigene Berechnung aus EURUSD=X.
  2. **„Das Wichtigste heute"**: Banner zeigt bis zu 4 automatisch erzeugte Bullets
     (Vola-Extreme je Markt, handelbare Underlyings, Termine nach Dringlichkeit,
     DD-Stand) – die 5-Sekunden-Antwort für eilige Kunden.
  3. **Sprungnavigation**: Chip-Leiste unter dem Banner (8 Anker, auf Mobile
     horizontal scrollbar) – lange Seite bleibt vollständig, wird aber navigierbar.
- Betroffene Dateien: `bot/morning_routine.py`, `web/app.py`,
  `web/templates/morning.html`, `web/templates/dashboard.html`
- Status: fertig
- Für Sepp relevant: ja — EVZ existiert nicht mehr, Kennzahl heißt jetzt
  „EUR/USD-Vola (30 T, realisiert)"; JSON-Schlüssel `evz` bleibt aus Kompatibilität.

### 2026-07-09 · Developer Agent · Kunden-Check Morgenroutine: Distribution Days korrigiert + Konsistenz-Fixes
- Was geändert (kritischer Kunden-Review, alle Werte gegen Originalquellen geprüft):
  1. **Distribution Days waren massiv falsch**: Seite zeigte ES 1 (grün) / NQ 3 (gelb),
     tatsächlich ES **7** / NQ **8** (beide rot = Alarm ≥5). Ursachen: inkrementeller
     Speicher existierte erst seit 06.07. (alle Juni-DDs fehlten), Datums-Label-Bug
     (bereits gestern gefixt) und manuelle Läufe während US-Handelszeit zählten
     unfertige Intraday-Sessions als DD (08.07. schloss +0,28% – kein DD).
     Neu: `_calc_distribution_days()` berechnet bei jedem Lauf komplett aus der
     60-Tage-Historie (deterministisch, selbstheilend), laufende Session wird
     ausgeschlossen. Gesamtampel heute dadurch korrekt **rot** (DD-Alarm).
  2. **Vola-Ampeln OVX/GVZ/EVZ konnten nie grün werden** (kein green_max im Mapping),
     obwohl die Tachos grüne Zonen zeigen – EVZ 7.5 war gelb statt grün. Schwellwerte
     jetzt deckungsgleich mit Tacho-Zonen und Sektor-Lage (OVX<40/GVZ<20/EVZ<10 grün).
  3. **Setup-Kompass widersprach der Sektor-Lage**: „GC: BCS heute handelbar" neben
     rotem GVZ („keine neuen Positionen"). Extreme Vola (rot) des Referenz-Index
     überstimmt jetzt den Trend → „Warten – GVZ 27.6 extrem erhöht …".
  4. **Banner-Widerspruch**: Bei roter Gesamtlage stand darunter „Heute ein Tag für
     SPS und BCS" (Gates offen) → jetzt roter Hinweis „keine Neueinstiege".
     Panik-Pills im Banner zeigen Klartext statt „vvix gt 140".
  - Verifiziert korrekt: VIX/VVIX/OVX/GVZ (yfinance exakt), EVZ (CBOE-Fallback),
    VIX-Terminkurve (vixcentral), Fear&Greed 42.3 (CNN previous_close), PCR 0.86,
    Setup-Kompass-Trendwerte (Stichproben CL/GC/NVDA). EVZ-Perzentil bleibt ohne
    Quelle (yfinance delisted, CBOE-Historie 403) → „—" in der UI.
  - Heutige Routine (2026-07-09.json) mit korrigierten Werten neu erzeugt,
    korrigierte Telegram-Zusammenfassung versendet.
- Betroffene Dateien: `bot/morning_routine.py`, `web/templates/morning.html`
- Status: fertig
- Für Sepp relevant: ja — DD-Zählung war kundenwirksam falsch (1/3 statt 7/8);
  Methodik jetzt: vollständige Neuberechnung je Lauf statt inkrementellem Speicher.

### 2026-07-08 · Developer Agent · Fix: Fear&Greed/PCR im Scanner-Dashboard leer trotz reparierter Quellen
- Was geändert: Das Dashboard liest die Markt-Filter aus dem letzten Scan-Lauf
  (scan_results.json) – der lief heute noch mit den kaputten Fetchern, der nächste
  erst morgen 09:35 ET. Neu: `_fill_market_filters()` in web/app.py ergänzt fehlende
  Werte (Fear&Greed, PCR, VIX) aus der Morgenroutine und EIA/FOMC per Datumslogik.
  Greift auch künftig, wenn beim Scan eine Quelle ausfällt oder noch kein Scan lief.
- Betroffene Dateien: `web/app.py`
- Status: fertig
- Für Sepp relevant: nein — reiner Anzeige-Fallback, keine Logikänderung am Scan.

### 2026-07-08 · Developer Agent · Auto-Trading komplett entfernt + Bugfix-Review über den ganzen Code
- Was geändert (Auftrag Max: kein automatisierter Handel mehr, danach Voll-Review):
  1. **Auto-Trading raus**: `bot/executor.py` gelöscht (Order-Platzierung via IB),
     `mode_trade`/`mode_run` aus main.py entfernt, Scheduler-Job 09:35 ET ist jetzt
     ein reiner **Scan-Job** (`mode_scan(notify=True)`: Kandidaten → Telegram-Signal
     „manuell in IBKR eröffnen"). Manager/IB-Sync bleiben (Alerts + Journal-Tracking).
     Tote Pre-Trade-Gates entfernt: `check_portfolio_limits`, `check_correlation_filter`,
     `check_roll` (rules.py), `check_market_filters` (market_filters.py),
     `notify_trade_placed/position_closed/exit_signal/roll_signal/circuit_breaker/
     market_filter_block` (notifier.py), `report_position` (firestore_reporter.py),
     Settings `ADAPTIVE_ORDER_*`, `DEFAULT_QUANTITY`, `CLIENT_ID_EXECUTOR`.
     CLAUDE.md dokumentiert jetzt: kein Order-Code wieder einführen.
  2. **Echte Bugs gefixt**: (a) market_filters: CBOE-PCR-URL war 404 und CNN blockte
     den minimalen User-Agent (418) → Fear&Greed/PCR waren im Scan-Ergebnis immer None;
     jetzt eine gemeinsame PCR-Quelle (morning_routine nutzt market_filters).
     (b) rules.check_exit: P&L in Alert-Texten rechnete hart ×100 → bei /CL (×1.000)
     Faktor 10 falsch; nutzt jetzt den Asset-Multiplikator. (c) morning_routine:
     Gesamtampel wurde bei fehlender VIX-Terminkurve fälschlich rot (`not None`);
     Distribution Days wurden mit Ausführungs- statt Sessiondatum gespeichert.
     (d) Delivery-Exit wird jetzt vor dem DTE-Exit geprüft (korrektes Alert-Label).
  3. **Kleinere Bereinigungen**: `load_routine()` fällt auf die neueste Routine zurück
     (Wochenende/vor 08:00 keine leere Seite), FOMC-Terminliste warnt bei Ablauf,
     VIX-Abruf mit period=2d, tote Imports/`margin_guard`-Reste/veraltete Docstrings
     entfernt, Einmal-Skript `sepp_fix_004.py` gelöscht, Tests angepasst (51 passed).
- Betroffene Dateien: `bot/executor.py` (gelöscht), `main.py`, `scheduler.py`,
  `bot/notifier.py`, `bot/rules.py`, `bot/scanner.py`, `bot/manager.py`,
  `bot/market_filters.py`, `bot/morning_routine.py`, `config/settings.py`,
  `firestore_reporter.py`, `web/app.py`, `web/templates/setup.html`,
  `tests/test_notifier.py`, `CLAUDE.md`, `sepp_fix_004.py` (gelöscht)
- Status: fertig
- Für Sepp relevant: ja — Grundsatzentscheidung: der Bot ist ein reines
  Signal-/Monitoring-System, Order-Code darf nicht wieder eingeführt werden.
  Außerdem liefern Fear&Greed/PCR im Scan-Ergebnis jetzt wieder Werte.

### 2026-07-08 · Developer Agent · Sektor-Lage in Morgenroutine + neutrale Gold-Vola-Texte
- Was geändert: Die Sektor-Lage-Box des Scanner-Dashboards erscheint jetzt auch auf der
  Morgenroutine-Seite (nach dem Setup-Kompass, nur in der Tagesansicht — im Archiv
  würden die Scanner-Filter nicht zum Datum passen). `_sector_assessments()` nimmt dafür
  optional eine Routine entgegen. Außerdem GVZ-Erklärungstexte richtungsneutral
  formuliert: hohe Gold-Vola = große erwartete Ausschläge in beide Richtungen
  (höhere Prämien, aber Risiko, dass der Kurs durch den Short Strike läuft) — vorher
  stand dort fälschlich, dass Anleger „in Gold drängen" (Preis kann auch fallen).
- Betroffene Dateien: `web/app.py`, `web/templates/morning.html`
- Status: fertig
- Für Sepp relevant: ja — Morgenroutine-Seite hat eine neue Sektion, Feedback von Max
  zu Vola-Erklärungen umgesetzt (allgemein bleiben, keine Richtungsaussagen).

### 2026-07-08 · Sepp · Sepp-Bridge v2 (Verzeichnis-Queue) migriert
- Was geändert: SEPP_QUEUE.md (Regex-basierte Einzeldatei-Queue) abgelöst durch
  sepp_tasks/{queue,done,failed}/ Verzeichnis-Queue. sepp_watcher.py komplett
  neu geschrieben: liest Task-Dateien aus sepp_tasks/queue/, verschiebt sie nach
  Abschluss nach done/ bzw. failed/ (mit Ergebnis/Fehler angehängt) statt eine
  gemeinsame Datei per Regex zu überschreiben. Grund: Race-Conditions bei
  parallelen Tasks vermeiden, klarerer Lebenszyklus pro Task.
- Betroffene Dateien: `sepp_watcher.py`, `sepp_tasks/queue/.gitkeep`,
  `sepp_tasks/done/.gitkeep`, `sepp_tasks/failed/.gitkeep`,
  `SEPP_QUEUE.md` -> `SEPP_QUEUE.md.deprecated`
- Status: Code-Seite fertig und gepusht. VPS-seitig noch offen: `git pull` +
  `systemctl restart sepp-watcher.service` muss noch manuell ausgeführt werden.
- Für Sepp relevant: ja — das bin ich selbst, Zugriff über Fine-grained PAT
  (Contents: Read/Write, nur dieses Repo) direkt von Max erhalten.


### 2026-07-08 · Developer Agent · Prämien-Niveau, Earnings/Events, 18 Underlyings, Datenstand
- Was geändert: (1) IV-Kontext: 12-Monats-Perzentil je Vola-Index (VIX/VVIX/OVX/GVZ/EVZ) als IVR-Proxy — in den Vola-Kacheln und als „Prämien-Niveau"-Spalte im Underlying-Check. (2) Termin-Spalte: nächste Earnings je Aktie (yfinance, ⚠ bei ≤21 Tagen), EIA-Tag (/CL), FOMC-Fenster (Metalle) — Earnings-Parser in market_filters.py für neue yfinance-Version repariert (dict statt DataFrame; Fix nützt auch dem Scanner). (3) Neues Anzeige-Universum `COMPASS_UNIVERSE` in settings.py: 18 Underlyings (CL, GC, SI, ES, NQ, SPY, QQQ, IWM + 10 Aktien) — bewusst getrennt vom Handelsuniversum ASSET_CONFIG, der Bot handelt unverändert. (4) Underlying-Tabelle als eigene Sektion „Underlying-Check" ans Seitenende verschoben, Hinweis-Link im Setup-Kompass. (5) Datenstand-Kennzeichnung an allen Sektionen/Karten (US-Vortagesschluss bzw. F&G live). Bekannt: ^EVZ hat keine yfinance-Historie mehr → EVZ-Perzentil bleibt leer (graceful).
- Betroffene Dateien: `config/settings.py`, `bot/morning_routine.py`, `bot/market_filters.py`, `web/templates/morning.html`, `web/templates/base.html`
- Status: fertig
- Für Sepp relevant: ja — Kompass-Universum jetzt konfigurierbar getrennt vom Handel; Earnings-Fix betrifft auch Scanner-Regel 019. Offen laut Max: Rechtliches (Disclaimer) und Zustellung später.

### 2026-07-08 · Developer Agent · Morgenroutine: 8 Verbesserungen (Max-Feedback)
- Was geändert: (1) Morgenroutine läuft ab jetzt 08:00 deutscher Zeit statt 08:30 ET (Scheduler auf Europe/Berlin; Daten = US-Vortagesschluss), Header zeigt deutsche Zeit. (2) Ampel-Zeitstempel in deutscher Zeit (neuer Jinja-Filter `de_time`). (3) Ampel-Zweitzeile sagt jetzt, ob heute ein SPS- oder BCS-Tag ist (aus Setup-Kompass-Gates). (4) GVZ-Texte fachlich korrigiert (hohe Vola = hohe Prämien, aber Extrembereich = Tail-Risk; „sichere Häfen"-Verwirrung beseitigt). (5) VIX-Terminkurve als SVG eingezeichnet (alle 8 Monate von vixcentral.com, Quelle war schon angebunden — jetzt voll ausgewertet) + Haken/X statt Ampelpunkt. (6) Status-Textbox unter Distribution Days. (7) Erläuterungstexte unter Advance/Decline und NH/NL. (8) Setup-Kompass vor die Panikzeichen-Sektion verschoben.
- Betroffene Dateien: `scheduler.py`, `bot/morning_routine.py`, `web/app.py`, `web/templates/morning.html`
- Status: fertig
- Für Sepp relevant: ja — Morgenroutine-Zeitpunkt geändert (08:00 Berlin statt 08:30 ET) und Terminkurven-Daten jetzt vollständig im Tages-JSON (`curve_months`/`curve_values`).

### 2026-07-08 · Developer Agent · Setup-Kompass in der Morgenroutine (SPS vs. BCS)
- Was geändert: Neue Sektion „Setup-Kompass" oben in der Morgenroutine beantwortet die Kundenfrage „Welche Strategie passt heute?": (1) Tagesfreigabe SPS/BCS aus Regel-013-Filtern (Fear&Greed, PCR, VIX — Schwellwerte aus config/settings.py) mit Klartext-Begründung je Sperre, (2) Trend-Einstufung je Underlying (Kurs vs. MA200, RSI 14 via yfinance, Regel-001-Logik gespiegelt aus rules.py) mit kombiniertem Tagesurteil („handelbar" / „Warten – Grund"). Datenbasis bewusst NUR Morgenroutine/yfinance — kein Scanner/IB (Kunden erhalten den Scanner nicht). Ergebnis wandert ins Tages-JSON (`setup_compass`), heutiges JSON nachträglich gepatcht. 15 neue Unit-Tests (53/53 grün).
- Betroffene Dateien: `bot/morning_routine.py`, `web/templates/morning.html`, `tests/test_setup_compass.py`
- Status: fertig
- Für Sepp relevant: ja — schließt die inhaltliche Lücke „wann SPS, wann BCS" im Kundenprodukt; Entscheidung Max 2026-07-08 (Morgenroutine-Daten statt Scanner).

### 2026-07-08 · UI Agent · Kachelraster ausgerichtet + 3er-Card-Reihe verdichtet
- Was geändert: Auswertungs-Textboxen der 5 Volatilitäts-Kacheln beginnen/enden jetzt exakt bündig (Untertitel reserviert 2 Zeilen, Note füllt Restfläche per flex). Sentiment-Card: Fear&Greed und PCR nebeneinander statt übereinander (halbe Bauhöhe) → deutlich weniger Leerraum in VIX-Terminkurve- und Distribution-Days-Card; Terminkurve-Inhalt vertikal verteilt, DD-Fußzeile unten verankert. Umsetzung nach Beratung durch UI-Designer-Agent (Option „Tachos nebeneinander" statt Grid-Umbau).
- Betroffene Dateien: `web/templates/morning.html`, `web/templates/base.html`
- Status: fertig
- Für Sepp relevant: nein — reine Layout-Verdichtung.

### 2026-07-08 · UI Agent · Auswertungstexte professionalisiert (Sepp-Task Tachos)
- Was geändert: Statements unter den Volatilitäts-Tachos (VIX/VVIX/OVX/GVZ/EVZ) sowie Fear&Greed/PCR fachlich umformuliert (2–3 Sätze, Zielgruppe Kunden ohne Vorwissen): farbige Status-Überschrift (Unkritisch/Erhöht/Kritisch) + neutraler Erklärtext statt eingefärbtem Kurztext. Zonen-Zuordnung kommt weiterhin serverseitig aus `item.color` (morning_routine) — Schwellwert-Änderungen greifen ohne Template-Anpassung. VVIX-Tacho-Skala auf Server-Schwelle synchronisiert (gelb ab 120 statt 140). Hinweis: die im Task genannte `market_ampel_config.py` existiert nicht; die Ampel-Schwellwerte liegen in `bot/morning_routine.py` (VOLA-Dict, Z. 329 ff.).
- Betroffene Dateien: `web/templates/morning.html`, `web/templates/base.html`
- Status: fertig
- Für Sepp relevant: ja — Task „TASKS.md UI Agent 2026-07-08" umgesetzt; einzig der Screenshot fehlt (kein Browser auf dem VPS), Max prüft visuell.

### 2026-07-08 · Developer Agent · Tacho-Skala lesbar + Ergebnis-Statements in Kacheln
- Was geändert: Min/Max-Zahlen am Tacho wurden abgeschnitten (SVG-viewBox zu klein) und waren zu klein — behoben (viewBox 200×160, Labels 12px fett). Alle Morgenroutine-Kacheln jetzt gleich groß; unter jedem Tacho ein kurzes farbiges Auswertungs-Statement zum aktuellen Wert (VIX/VVIX/OVX/GVZ/EVZ nach Ampelfarbe, Fear&Greed und PCR nach Regel-013-Schwellwerten).
- Betroffene Dateien: `web/templates/base.html`, `web/templates/morning.html`
- Status: fertig
- Für Sepp relevant: nein — reine UI-Verbesserung.

### 2026-07-08 · Developer Agent · Einheitliche Entry-Regel IVR>40 / DTE 45–70
- Was geändert: Entry-Kriterien für ALLE Underlyings (CL, GC, TSLA, NVDA, META, ORCL, QQQ) vereinheitlicht auf IVR>40 und DTE 45–70 (Entscheidung Max, 2026-07-08). Ersetzt die bisherigen Aktien-Werte (DTE 30–45, teils IVR 30) und das Futures-Fenster 50–80 (Ziel 65). DTE-Ziel einheitlich 55 (Mitte des Fensters, entspricht Regel-002-Ziel). Globale Fallbacks (MIN_IVR, DTE_MIN/MAX) mitgezogen, Tests angepasst — 38/38 grün.
- Betroffene Dateien: `config/settings.py`, `tests/test_rules.py`, `CLAUDE.md` (Regel-Tabelle); `bot/rules.py` geprüft — check_entry() liest bereits generisch aus der Asset-Config, keine Fallunterscheidung nötig
- Status: fertig
- Für Sepp relevant: ja — der Blocker „IVR/DTE-Konflikt" ist damit entschieden und umgesetzt.

### 2026-07-08 · Developer Agent · Sync-Mirror eingerichtet + Path-Traversal-Fix
- Was geändert: GitHub-Action `mirror-state` aktiviert (spiegelt STATUS.md/TASKS.md ins öffentliche Repo `optionsbot-state`, mit Whitelist + fail-closed Secret-Scan). STATUS.md mit echtem Projektstand befüllt, Pflegeregel in CLAUDE.md ergänzt. Zusätzlich: Datums-Parameter auf `/morning` wird jetzt strikt validiert (Path-Traversal-Schutz).
- Betroffene Dateien: `.github/workflows/mirror-state.yml`, `STATUS.md`, `CLAUDE.md`, `web/app.py`
- Status: fertig
- Für Sepp relevant: ja — STATUS.md ist ab jetzt über `optionsbot-state` öffentlich lesbar und damit dein Einstiegspunkt für jeden Session-Sync.

### 2026-07-07 · Developer Agent · Branding, Favicon & UI-Feinschliff
- Was geändert: Favicon (SVG), Branding „Steady Alpha" auf allen Seiten, Responsive- und Security-Feinschliff (u. a. Security-Header, Meta-Tags).
- Betroffene Dateien: `web/app.py`, `web/models.py`, `web/static/favicon.svg`, diverse `web/templates/*.html`
- Status: fertig
- Für Sepp relevant: nein — rein kosmetisch/Hygiene.

### 2026-07-07 · Developer Agent · Kunden-Profilseite
- Was geändert: Profilseite für Kunden mit Paketübersicht und Passwort-Änderung.
- Betroffene Dateien: `web/app.py`, `web/templates/profile.html`, `web/templates/base.html`
- Status: fertig
- Für Sepp relevant: ja — Kunden-Selbstverwaltung ist damit komplett (Registrierung → Login → Profil).

### 2026-07-07 · Developer Agent · Öffentliche Landingpage + Bot-Schutz
- Was geändert: Öffentliche Landingpage unter `/`, Bot-Schutz (Honeypot/Zeitfalle) für die Registrierung.
- Betroffene Dateien: `web/app.py`, `web/templates/landing.html`, `web/templates/register.html`
- Status: fertig
- Für Sepp relevant: ja — erster öffentlicher Auftritt von Steady Alpha.

### 2026-07-07 · Developer Agent · Kunden-System (Registrierung, Pakete, Admin)
- Was geändert: Selbstregistrierung, Rollen admin/customer, Paket-Zugriff (morning/scanner) via SQLite/SQLAlchemy (`data/customers.db`), Admin-Kundenverwaltung unter `/admin/customers`. Datenmodell Stripe-fähig ausgelegt.
- Betroffene Dateien: `web/app.py`, `web/models.py`, `web/templates/admin_customers.html`, `web/templates/admin_customer_detail.html`, `web/templates/register.html`, `web/templates/customer_login.html`, `web/templates/no_access.html`, `requirements.txt`, `.gitignore`
- Status: fertig
- Für Sepp relevant: ja — Fundament für das Kundengeschäft; Abo-Datenmodell steht.

### 2026-07-07 · Developer Agent · Scanner-UI: Sektor-Lage + Skip-Gründe
- Was geändert: Sektor-Lage-Box im Dashboard, ehrliche Skip-Anzeige je Symbol (warum kein Trade).
- Betroffene Dateien: `bot/scanner.py`, `web/app.py`, `web/templates/dashboard.html`
- Status: fertig
- Für Sepp relevant: ja — Scanner-Entscheidungen sind jetzt pro Symbol nachvollziehbar.

### 2026-07-07 · Developer Agent · UI responsive + Tacho-Fix + helles Redesign
- Was geändert: Komplettes Redesign auf helles Profi-Layout mit Tachos und Ampeln, Tacho-Renderer robust gemacht (Werte wurden teils nicht angezeigt), responsive für iPad/iPhone/Android. (Commits d6ad780, 710e01c, 3d28b46)
- Betroffene Dateien: `web/templates/base.html`, `web/templates/dashboard.html`, `web/templates/morning.html`, `web/templates/login.html`, `web/templates/setup.html`, `web/templates/rules.html`, `CLAUDE.md`
- Status: fertig
- Für Sepp relevant: nein — UI-Konventionen sind in CLAUDE.md dokumentiert.

### 2026-07-07 · Developer Agent · Dienste laufen als Service-User (non-root)
- Was geändert: Alle systemd-Dienste auf dedizierten User `optionsbot` migriert, sudoers nur für Gateway start/stop.
- Betroffene Dateien: `optionsbot.service`, `optionsbot-web.service`, `sepp_runner.py`, `sepp_watcher.py`, `web/app.py`, `.gitignore`
- Status: fertig
- Für Sepp relevant: ja — Voraussetzung für den öffentlichen Kundenbetrieb war Härtung; jetzt erfüllt.

### 2026-07-06 · Developer Agent · Web-App gehärtet
- Was geändert: CSRF-Schutz, Rate-Limiting, Passwort-Hashing (scrypt), sichere Session-Cookies.
- Betroffene Dateien: `web/app.py`, `sepp_receiver.py`, diverse Templates
- Status: fertig
- Für Sepp relevant: ja — Teil des Voll-Audits vom 2026-07-06 (fail2ban, UFW, SSH, HTTPS serverseitig).

### 2026-07-06 · Developer Agent · Journal/Web-Fixes + Universum verschlankt
- Was geändert: Handels-Universum auf CL/GC/Aktien/QQQ reduziert, PT-Balken ab Nullpunkt, Exit-Gründe im Journal, Import-Kennzeichnung, Morgenroutine-UX verbessert. (Commits 7d38320, e75e32b)
- Betroffene Dateien: `config/settings.py`, `bot/scanner.py`, `bot/morning_routine.py`, `bot/ib_sync.py`, `bot/rules.py`, `bot/notifier.py`, `journal/trades.py`, `web/app.py`, Templates
- Status: fertig
- Für Sepp relevant: ja — Universum-Entscheidung (Fokus CL/GC statt breitem FOP-Korb) ist strategierelevant.

### 2026-07-06 · Developer Agent · DTE-Entry-Fenster Futures 50–80 (Ziel 65)
- Was geändert: DTE-Fenster für Futures-Einstiege in der Config erweitert.
- Betroffene Dateien: `config/settings.py`, `CLAUDE.md`
- Status: fertig
- Für Sepp relevant: ja — direkte Regeländerung (Maxoman 2026_01).

### 2026-07-06 · Developer Agent · Scanner-Regelfixes
- Was geändert: BCS-Delta-Band korrigiert, IVP-Zusatzcheck (Regel 018), Quote-Validierung, Multiplikator-Korrektur.
- Betroffene Dateien: `bot/scanner.py`, `bot/rules.py`, `config/settings.py`
- Status: fertig
- Für Sepp relevant: ja — betrifft Einstiegslogik direkt.

### 2026-07-06 · Developer Agent · Morgenroutine-Datenquellen repariert
- Was geändert: NH/NL via WSJ/Barron's Markets Diary (yfinance-Ticker tot), VIX-Drop auf Schlusskursbasis, EVZ via CBOE Delayed Quotes API als Fallback, Doku in CLAUDE.md aktualisiert. (Commits 18a8e38, c6b73e1, 7e3ac14)
- Betroffene Dateien: `bot/morning_routine.py`, `CLAUDE.md`
- Status: fertig
- Für Sepp relevant: ja — Datenquellen-Tabelle in CLAUDE.md ist wieder verlässlich.

### 2026-07-08 · Sepp · Sync-Protokoll eingeführt
- Was geändert: STATUS.md als Schnittstellendatei angelegt.
- Betroffene Dateien: `STATUS.md`, `CLAUDE.md` (Pflegeregel ergänzt).
- Status: fertig
- Für Sepp relevant: ja — ab jetzt Einstiegspunkt für jeden Session-Sync.
