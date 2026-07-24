# TODO – Weg zum GoLive

> **Stand:** 2026-07-14 · Vollständige GoLive-Analyse (Security-, Produkt- und
> Marketing-Audit, Details in STATUS.md). Gepflegt vom Developer Agent,
> gelesen von Max & Sepp. Erledigtes wird abgehakt und wandert nach unten.

**Kritischer Pfad:** Rechtstexte + Stripe-Konto (Max) → Stripe-Strecke +
Widerrufs-Checkbox (Agent) → Beta-Test → Anwalt-OK → Launch.

---

## 🔴 Max – blockiert den GoLive

- [ ] **2. Rechtstexte vervollständigen** (~30 Min)
  Audit 2026-07-14: noch **26 Platzhalter** auf `/impressum`, `/datenschutz`,
  `/agb`, `/widerruf`. Max muss konkret liefern: ladungsfähige Anschrift,
  öffentliche Kontakt-E-Mail, Telefonnummer, **USt-Status** (USt-ID oder
  § 19 Kleinunternehmer – Grundsatzentscheidung, beeinflusst auch die
  Preisangaben!), Bundesland (→ Datenschutz-Aufsichtsbehörde),
  Kündigungsfrist, Zielmarkt DE/AT/CH. Danach entfernt der Developer Agent
  die „Entwurf“-Banner.

- [ ] **3. Anwaltliche Prüfung der Rechtstexte beauftragen** (parallel!)
  Fachanwalt IT-Recht oder eRecht24 Premium, ~200–500 €. Jetzt beauftragen,
  während der Rest läuft.

- [ ] **4. Stripe-Konto anlegen**
  stripe.com → Gewerbedaten, Bankverbindung. Voraussetzung für die
  Zahlungsstrecke (Punkt 8).

- [ ] **5. Offsite-Backup aktivieren** (~10 Min + ~4 €/Monat) — **hochgestuft:**
  laut Security-Audit blockierend vor zahlenden Kunden (Kundendaten-DB
  existiert bereits; lokales Backup schützt nicht vor Serververlust).
  Hetzner Storage Box bestellen, `BACKUP_REMOTE` in secrets.env setzen.

- [ ] **15. Entscheidung: IB Gateway** — seit 13.07. 13:04 gestoppt (sauber
  beendet). Scanner/Monitor/IB-Sync laufen seither ins Leere (letzter Scan
  10.07.). Absichtlich aus (Morning-only-Launch) oder wieder starten?

## 🔴 Developer Agent – blockierend, sofort machbar (wartet NICHT auf Max)

- [x] **16. Double-Opt-in-Bypass** — ✅ erledigt 2026-07-14: Auto-Verifizierung
  nur noch ohne SMTP-Konfiguration; bei Versandfehler bleibt der Account
  unverifiziert (Hinweis auf Resend im Profil). 3 neue Tests.

- [x] **17. AGB/Datenschutz korrigiert** — ✅ erledigt 2026-07-14: § 5 jetzt
  49 €/Monat + Jahresplan 470,40 € (−20 %), § 2 nennt Telegram, veralteter
  Kündigungsbutton-Platzhalter ersetzt; Datenschutz § 6: IONOS eingetragen
  + Telegram-Zustellung (Drittland-Passus) neu dokumentiert.
  USt-Frage + AVV bleiben Platzhalter für Max/Anwalt (Punkt 2/3).

- [x] **11. Betriebs-Monitoring** — ✅ erledigt 2026-07-15: Versand-Alarme
  an allen stillen Fehlerpfaden, Morgenroutine-Job abgesichert, Retry-Job
  08:20 (idempotent), Heartbeat-Job 08:35, `/healthz`-Endpoint.
  **Rest für Max (~5 Min):** externen Uptime-Check anlegen (z. B.
  UptimeRobot, kostenlos) auf `https://www.steadyalpha.de/healthz`,
  Keyword-Match auf `"status":"ok"`.

- [x] **18. `/beispiel` finalisiert + committet** — ✅ erledigt 2026-07-14:
  Auto-Reload aus, Handlungsanweisungen demo-abhängig neutralisiert
  (inkl. Template-Fallbacks), PNG committet, Route als bewusst öffentlich
  deklariert. Live unter https://www.steadyalpha.de/beispiel

- [x] **12. Preise öffentlich sichtbar machen** — ✅ erledigt 2026-07-15:
  Preissektion auf der Landingpage (`#preis`): 49 €/Monat, Jahresplan
  470,40 € (−20 %), „Endpreise", ehrlicher Hinweis „Online-Buchung folgt".
  USt-Formulierung wird mit Max' USt-Status abgeglichen (Punkt 21).

## 🔵 Developer Agent – Phase 2 (startet nach Punkt 4)

- [ ] **8. Stripe-Zahlungsstrecke** (größtes Stück, ~2–3 Sessions)
  Checkout-Session in `abo_book`, Webhook-Endpoint (CSRF-exempt,
  **Signaturprüfung, Idempotenz**, nginx `limit_req`), Modell-Erweiterung:
  `stripe_customer_id` (Customer), `stripe_subscription_id` (Subscription),
  Unique-Index auf `Invoice.stripe_invoice_id`; Verlängerung/Dunning
  (Zahlungsausfall → Sperre), Stripe-Kündigung in `abo_cancel`,
  Test mit Stripe-Testkarten. Security-Anforderungen: siehe
  `steady-alpha-security/references/review-checkliste.md`.

- [ ] **10. Widerrufs-Checkbox im Checkout** + § 312j-konformer
  Buchen-Button („zahlungspflichtig bestellen“) + AGB-/Widerrufs-
  Kenntnisnahme am Button.

- [x] **9. Kündigungsbutton (§ 312k BGB)** — ✅ fertig (auf `/abo`, Audit bestätigt).

## 🟠 Wichtig – vor/während der Beta

- [ ] **19. Basistests Kundenstrecke** — 53 Tests grün, aber null Abdeckung
  für Registrierung/Login/Abo/Versand.
- [ ] **20. Security-Härtung Stufe 2** (Audit): Legacy-Klartext-Verify beim
  Admin-Login entfernen, nginx `limit_req` für `/customer/login` +
  `/register`, `pip-audit` über requirements als Routine.
- [ ] **21. „inkl. USt.“-Zeile auf `/abo`** mit USt-Status abgleichen
  (bei Kleinunternehmerregelung falsch).
- [ ] **22. Security-Baseline-Doku fortschreiben** (Domain steadyalpha.de,
  `/beispiel` als bewusst öffentliche Route eintragen).
- [ ] **6. NVDA-Trade c561729f nachtragen** (Max: echten Schlusskurs aus IBKR).
- [ ] **7. Restentscheidung:** Mirror-Repo-Tippfehler `optinsbot-state`
  umbenennen ja/nein?

## 🟢 Gemeinsam – Phase 3 (nach Phase 2)

- [ ] **13. Beta-Test**: 5–10 echte Nutzer, 2–4 Wochen kostenlos.
  Prüfen: Mail-Zustellbarkeit (Spam!), Verständlichkeit, Feedback,
  Testimonials.
- [ ] **14. GoLive & Marketing** – erst nach Anwalt-OK (Punkt 3) und Beta.

## 🗄️ Nice-to-have / Aufräumen (niedrige Priorität)

- [ ] CSP-Header (seit lokalen Assets günstig umsetzbar), fail2ban-Jail für
  nginx, `telegram_log.json`-Trimming (30 Tage, wie mail_log),
  `Invoice.pdf_url` aus Stripe befüllen, Beispiel-Morgenmail als Vorschau,
  Marketing-Check der generischen „handelbar“-Tooltips auf `/beispiel`.
- [ ] `TASKS.md` (Stand Mai, verweist auf gelöschten executor) archivieren
  oder von Sepp neu aufsetzen lassen.

---

## ✅ Erledigt (Kurzfassung, Details in STATUS.md)

- [x] **1. SMTP-Zugang** — IONOS `noreply@steadyalpha.de`, Port 587/STARTTLS; Double-Opt-in + Morgenmail scharf (12.07.)
- [x] Eigene Domain **www.steadyalpha.de** live: DNS, Let’s Encrypt, HTTPS-Redirects, HSTS (12.07.)
- [x] Kündigungsbutton § 312k BGB auf `/abo` (13.07.)
- [x] Abo-Seite + Empfehlungssystem, Stripe-ready Datenmodell (13.07.)
- [x] Zustellwege E-Mail + Telegram als Mehrfachauswahl (13.07.)
- [x] Kunden-Bot @SteadyAlphaServiceBot live, Telegram-Foto-Dashboard (12.07.)
- [x] Security-Baseline-Check 30/30 bestanden (Audit 14.07.)
- [x] Auto-Trading vollständig entfernt – Bot ist reines Signal-/Monitoring-System (08.07.)
- [x] Voll-Review: Datenquellen-Bugs (F&G/PCR), P&L-Multiplikator, DD-Zählung, Gesamtampel (08.–09.07.)
- [x] Morgenroutine kundenfertig: EUR/USD-Vola, TL;DR-Bullets, Sprungnavigation, Rot übersteuert Kompass (09.07.)
- [x] Journal-Bug behoben: manuelle Schließungen nicht mehr als Maximalgewinn (09.07.)
- [x] Rechtstexte als Entwurf live: Impressum, Datenschutz, AGB, Widerruf (09.07.)
- [x] Tägliche Backups + Restore-Probe, systemd-Timer 03:30 (10.07.)
- [x] Google Fonts/CDN lokal – kein Drittanbieter-Request mehr (10.07.)
- [x] Double-Opt-in bei Registrierung, E2E getestet (10.07.)
