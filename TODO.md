# TODO – Weg zum GoLive

> **Stand:** 2026-08-03 · Fokus: **Morgenroutine als Kundenprodukt** live bringen
> (Scanner, Positionen, Journal sind reine Admin-Werkzeuge für Max). Gepflegt vom
> Developer Agent, gelesen von Max & Sepp. Erledigtes wandert nach unten.

**Kritischer Pfad:** Delivery-Strecke + Domain-Klärung (Ziel 1) → Daten-Verifier
(Ziel 2) → Test-Launch mit kostenloser Testphase → bezahlt via Stripe.

**Modell (Entscheidung Max):** bezahlt ab Start, aber mit vorgeschalteter
kostenloser Testphase; Zeitrahmen ~2 Wochen mit Puffer.

---

## 🔴 Max – blockiert / braucht dich

- [ ] **2FA für Admin-Login aktivieren** (~2 Min, empfohlen) — Code steht bereit,
  ist „schlafend". Aktivieren:
  1. `source /home/optionsbot/venv/bin/activate && python scripts/admin_totp_setup.py`
  2. QR mit Authenticator-App scannen (oder Secret manuell eintippen)
  3. ausgegebene `ADMIN_TOTP_SECRET=…`-Zeile in `/etc/optionsbot/secrets.env` eintragen
  4. `sudo systemctl restart optionsbot-web.service`
- [ ] **Stripe für den bezahlten Flow vervollständigen:** `STRIPE_PRICE_ID` fehlt
  (Preis/Produkt in Stripe anlegen → ID eintragen); später **Live-Keys** statt
  der aktuellen Test-Keys. Test-Keys + Webhook-Secret sind bereits gesetzt.
- [ ] **Offsite-Backup aktivieren** (~10 Min, ~4 €/Monat) — Hetzner Storage Box
  bestellen, dann setzt der Agent `BACKUP_REMOTE`. Vor zahlenden Kunden nötig.
- [ ] **Domain-Migration klären (TASK 6):** `www.steadyalpha.de` ist live (HTTPS,
  HSTS, Redirects); Apex→www und Alt-Domain `optionbot.neuner-productions.de`
  (301) stehen. Offen: Was genau soll noch migriert werden + liegt das DNS bei dir?
- [ ] **Externen Uptime-Check** anlegen (UptimeRobot o. ä., kostenlos) auf
  `https://www.steadyalpha.de/healthz`, Keyword `"status":"ok"`.
- [ ] **Beta-Nutzer** finden (5–10) für die kostenlose Testphase.
- [ ] **Restrisiko Geschäftsmodell-Einstufung** (Informationsdienst vs.
  erlaubnispflichtig) — bewusst ohne Anwalt (deine Entscheidung). Optional: einmalige
  Erstberatung nur zu dieser Einstufungsfrage (~150–300 €).

## 🔵 Developer Agent – als Nächstes (Richtung Launch)

- [ ] **Delivery-Strecke Morgenroutine (TASK 5, Ziel 1):** 2-seitiges A4-PDF,
  Render 07:55 / Versand 08:00 (Berlin), byte-identisch für E-Mail-Anhang und
  Portal-Download; geschützter Kundenbereich. Siehe `docs/morning_delivery.md`.
- [ ] **Daten-Verifier (4-Schichten-Gate, fail-closed, Ziel 2):** bis dahin
  Distribution-Day-Werte Kunden NICHT als „verifiziert" zeigen. Abgrenzung zum
  bestehenden `bot/dd_verify.py` (DD-spezifisch) klären.
- [ ] **Checkout-Legal für bezahlt:** Widerruf-Checkbox + § 312j-Button
  („zahlungspflichtig bestellen") + AGB-/Widerrufs-Kenntnisnahme; danach
  Stripe-E2E-Test im Testmodus (sobald `STRIPE_PRICE_ID` gesetzt).
- [ ] **Basistests Kundenstrecke** (Registrierung/Login/Abo/Versand) — aktuell
  keine Abdeckung.
- [ ] **Vorbestehenden Testfehler** `test_ensure_gateway_fast_path_no_restart`
  grün machen oder sauber als umgebungsabhängig markieren.

## 🟠 Optional / Härtung (nicht Launch-blockierend)

- [ ] IB-abhängige Scheduler-Jobs (Scan/Monitor/Sync/Engine/Prewarm) für den
  Morgenroutine-only-Betrieb sauber behandeln (deaktivieren oder als Max-Werkzeug
  belassen — Entscheidung offen).
- [ ] Security-Feinschliff: CSP-Nonce-Härtung (aktuell `'unsafe-inline'`),
  `pip-audit` als CI-/Routine-Job, Legacy-Klartext-Verify ist bereits entfernt.
- [ ] Netz-Isolation Admin-Bereich (Level 2) — nur falls du es doch willst
  (verworfen zugunsten 2FA, da du weltweiten Zugriff brauchst).

## 🟢 Gemeinsam – Launch

- [ ] **Test-/Beta-Phase**: Zustellbarkeit (Spam!), Verständlichkeit, Feedback,
  Testimonials.
- [ ] **GoLive & Marketing** – nach Testphase; Umschalten Testphase → bezahlt.

---

## ✅ Erledigt (Kurzfassung, Details in STATUS.md)

**Session 2026-08-03:**
- [x] **Regelwerk konsolidiert** (`REGELWERK.md`), DTE/Universum-Divergenzen
  aufgelöst (verbindlich: DTE 45–70/55, Universum 30, Positionslimit 4)
- [x] **Rechtstexte finalisiert** – Impressum/Datenschutz/AGB/Widerruf vollständig
  & live (USt-Regelbesteuerung + BayLDA bestätigt)
- [x] **Security-Härtung Stufe 2** – CSP + Permissions-Policy, Admin-Verify
  fail-closed, nginx-Rate-Limit + fail2ban-nginx, Dependency-Scan 0 CVEs
- [x] **2FA (TOTP) implementiert** – schlafend, Aktivierung = Max-Task oben
- [x] **Morgenroutine ↔ Scanner getrennt** (Level 1) – Scanner/Positionen/Journal
  strikt admin-only; Kundenprodukt = nur Morgenroutine
- [x] Bestätigt: **Morgenroutine benötigt kein IB** (läuft bei gestopptem Gateway)

**Früher:**
- [x] SMTP (IONOS), Domain `www.steadyalpha.de` (HTTPS/HSTS), Kündigungsbutton § 312k
- [x] Kunden-System (Registrierung, Double-Opt-in, Rollen/Pakete), Abo-Seite,
  Stripe-ready Datenmodell + Zahlungsstrecke (Code fertig, wartet auf Keys/PriceID)
- [x] Zustellwege E-Mail + Telegram, Kunden-Bot live
- [x] Auto-Trading vollständig entfernt (reines Signal-/Monitoring-System)
- [x] Morgenroutine kundenfertig, `/beispiel` live, Preise öffentlich
- [x] Tägliche Backups + Restore-Probe, lokale Fonts (kein CDN), Betriebs-Monitoring
- [x] Security-Baseline-Check 30/30
