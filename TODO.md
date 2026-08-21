# TODO – Weg zum GoLive

> **Stand:** 2026-08-13 · Fokus: **Morgenroutine als Kundenprodukt** live bringen
> (Scanner, Positionen, Journal sind reine Admin-Werkzeuge für Max). Gepflegt vom
> Developer Agent, gelesen von Max & Sepp. Erledigtes wandert nach unten.

**Kritischer Pfad:** Die Technik steht. Was jetzt noch fehlt, liegt **fast
vollständig bei Max** (Stripe-PriceID, DNS/Zustellbarkeit, Backup, Uptime,
Beta-Nutzer) → danach kostenlose Testphase → bezahlt via Stripe.

**Modell (Entscheidung Max):** bezahlt ab Start, aber mit vorgeschalteter
kostenloser Testphase; Zeitrahmen ~2 Wochen mit Puffer.

---

## 🔴 Max – blockiert / braucht dich

> Alles hier ist für **nächste Woche** verabredet (Stand 10.08.).

### ✅ Domain-Trennung: umgeschaltet am 19.08.2026

> **Erledigt:** nginx-Site + Let's-Encrypt-Zertifikat (gültig bis 17.11.2026),
> `CUSTOMER_BASE_URL=https://dailyoption.de` gesetzt, Dienste neu gestartet,
> Verifikation grün (`scripts/verify_domain_split.sh`). Kundenseite läuft unter
> **dailyoption.de** als „Daily Option", steadyalpha.de ist reine Admin-Domain.
> nginx-Configs liegen jetzt gesichert in `deploy/`.

**Was jetzt noch offen ist:**

- [x] **Stripe-Webhook auf dailyoption.de umgezogen (19.08., 20:42).** Max hat
  die URL des bestehenden Endpoints geändert statt einen zweiten anzulegen —
  das ist der kürzere Weg: Stripe behält beim URL-Wechsel das
  Signaturgeheimnis, das schon in `secrets.env` steht. Verifiziert per
  `stripe trigger checkout.session.completed` aus der Stripe Shell:
  Zustellung von einer Stripe-IP mit **HTTP 200**, keine Signaturwarnung.
  Ein zweites Secret und das Abschalten eines Altendpoints entfielen damit.
- [ ] **Echter Durchstich im Testmodus (B5).** Registrierung auf
  dailyoption.de → Abo buchen → Testkarte `4242 4242 4242 4242`. Prüft
  zusätzlich Checkout-Rückkehradresse und Freischaltung. Legt einen
  Testkunden an, der danach aufgeräumt werden kann.
- [ ] **Live-Modus.** Live-Keys eintragen **und** den Webhook-Endpoint im
  Live-Modus separat anlegen — Stripe führt Test und Live getrennt, der
  umgezogene Endpoint gilt nur für den Testmodus. Beim Live-Endpoint entsteht
  ein neues Secret; `STRIPE_WEBHOOK_SECRET` verträgt seit 19.08. mehrere
  Werte nebeneinander, der Wechsel geht also ohne Lücke.
- [x] **Absender umgestellt (19.08., 20:04).** `SMTP_USER`/`SMTP_PASSWORD`/
  `SMTP_FROM` = `morgenroutine@dailyoption.de`, alle drei Kundenadressen als
  Absender freigegeben, Testmail zugestellt.
- [ ] **🔴 Max · Passwort `noreply@steadyalpha.de` bei IONOS wechseln.** Es
  stand durch ein fehlerhaftes Aufrufmuster im Klartext im systemd-Journal und
  in den sudo-Logs (Prozessliste ist für jeden Systemnutzer lesbar). Die
  Ursache ist behoben (`-p EnvironmentFile=` statt `--setenv=`), das bereits
  geloggte Passwort lässt sich aber nicht selektiv entfernen. Das Postfach
  wird nicht mehr zum Versand genutzt, mit dem Passwort ließe sich aber
  weiterhin als `@steadyalpha.de` mit gültigem SPF/DKIM senden.
- [ ] **Thomas · Warmup** für dailyoption.de starten (täglich kleine Mengen an
  echte Adressen), bevor der Serienversand auf den neuen Absender umzieht.
- [ ] **Später:** DMARC von `p=none` auf `quarantine` hochziehen, sobald
  2–4 Wochen saubere Reports vorliegen.
- [ ] **Optional (Max):** `dailyoption.io` defensiv registrieren — die App
  bedient sie bereits als Alias und leitet per 301 auf die kanonische Domain.
  `dailyoption.com` gehört einem Dritten und ist keine Option.
- [ ] **Anwalts-Review:** Produktname „Daily Option" in den Scope aufnehmen.
- [x] **Ausstiegsseite entschieden und umgesetzt (Max, 20.08.):**
  **TP 50 % · Zeit-Exit 7 DTE · Stop 2×.** Scanner-Planung und laufende
  Überwachung sind seither derselbe Satz — abgeleitet aus einer Quelle in
  `config/settings.py`, per Test gegen erneutes Auseinanderlaufen gesichert.
  Zwei Dinge zum Mitnehmen: Der Zeit-Exit bei 7 DTE hält Positionen deutlich
  tiefer in der Gamma-Zone als die früheren 21 DTE (gewollt bei Short-Δ 0,30),
  und der Quick-Win (25 % Gewinn innerhalb der ersten 7 Tage) greift weiterhin
  **vor** dem Take-Profit.



- [ ] **Stripe: Webhook auf die neue Domain umziehen** (Details unten im
  Domain-Abschnitt), danach **Live-Keys** statt der aktuellen Test-Keys.
  **Korrektur 19.08.: `STRIPE_PRICE_ID` wird gar nicht gebraucht.** Der alte
  Eintrag („PriceID fehlt, Produkt in Stripe anlegen") war falsch — die App
  erzeugt `price_data` je Checkout inline aus `cfg.PRICING`; im Code wird
  `STRIPE_PRICE_ID` nirgends gelesen. `_stripe_ready()` verlangt nur
  `STRIPE_SECRET_KEY` und `STRIPE_WEBHOOK_SECRET`, beide sind gesetzt. Die
  Zahlungsstrecke ist im Testmodus also vollständig einsatzbereit.
- [ ] **Stripe-Event `invoice.upcoming` im Webhook aktivieren** (Dashboard,
  Vorlauf einstellbar) — sonst geht die Erinnerungsmail vor der
  Jahresverlängerung nicht raus. Code steht seit 13.08.
- [ ] **Optional: öffentliches Mirror-Repo umbenennen** — es heißt
  `optinsbot-state` (fehlendes „o" beim Anlegen). Umbenennen auf
  `optionsbot-state` ändert die öffentliche URL (GitHub leitet alt→neu weiter);
  danach passt der Agent `PUBLIC_REPO` in `.github/workflows/mirror-state.yml`
  an. Nur wenn du es sauber haben willst — funktional ist nichts kaputt.
- [ ] **2FA für Admin-Login aktivieren** (~2 Min, empfohlen) — Code steht bereit,
  ist „schlafend". Aktivieren:
  1. `source /home/optionsbot/venv/bin/activate && python scripts/admin_totp_setup.py`
  2. QR mit Authenticator-App scannen (oder Secret manuell eintippen)
  3. ausgegebene `ADMIN_TOTP_SECRET=…`-Zeile in `/etc/optionsbot/secrets.env` eintragen
  4. `sudo systemctl restart optionsbot-web.service`
- [ ] **Offsite-Backup aktivieren** (~10 Min, ~4 €/Monat) — Hetzner Storage Box
  bestellen, dann setzt der Agent `BACKUP_REMOTE`. Vor zahlenden Kunden nötig.
- [ ] **Zustellbarkeit / DNS** (der wichtigste verbleibende Risikoblock):
  - **SPF ergänzen, nicht überschreiben** (ein zweiter `v=spf1` macht die Domain ungültig!)
  - DKIM-Selector des Hosters belassen, ggf. zweiter Selector für den Versandweg
  - **DMARC** anlegen (Start `p=none` → nach 2 Wochen Reportauswertung `p=quarantine`)
  - Reverse-DNS setzen
  - Absender `morgenroutine@steadyalpha.de`, **Reply-To auf eine echt gelesene Adresse**
  - Zustellbarkeitstest Gmail/Outlook/GMX/Web.de/T-Online + **Warmup** vor Massenversand
- [ ] **Domain-Migration klären (TASK 6):** `www.steadyalpha.de` ist live (HTTPS,
  HSTS, Redirects); Apex→www und Alt-Domain `optionbot.neuner-productions.de`
  (301) stehen. Offen: Was genau soll noch migriert werden + liegt das DNS bei dir?
- [ ] **Externen Uptime-Check** anlegen (UptimeRobot o. ä., kostenlos) auf
  `https://www.steadyalpha.de/healthz`, Keyword `"status":"ok"`.
- [ ] **Beta-Nutzer** finden (5–10) für die kostenlose Testphase.
- [ ] **Restrisiko Geschäftsmodell-Einstufung** (Informationsdienst vs.
  erlaubnispflichtig) — bewusst ohne Anwalt (deine Entscheidung). Optional: einmalige
  Erstberatung nur zu dieser Einstufungsfrage (~150–300 €).

**Zur Kenntnis (vom Agent entschieden, Widerspruch jederzeit möglich):**
- **Dezimaltrennzeichen: Komma** (`18,70`) für alle Kundenkanäle. Grund: Die
  Kundenseite mischte bisher deutsche Tausender (`1.234`) mit englischen
  Dezimalstellen (`18.70`) — in derselben Ansicht nicht auflösbar. Admin-Seiten
  bleiben unverändert.
- **Telegram bekommt zusätzlich das PDF** (nicht nur das Dashboard-Foto), damit
  alle Kanäle dasselbe Produkt liefern.
- **Fail-closed-Versand:** Ohne bestandenes Verifier-Gate geht **gar nichts**
  raus (vorher: Mail ohne Anhang). Lieber ein Tag Ausfall mit Alarm als
  ungeprüfte Zahlen beim Kunden.

## 🔵 Developer Agent – als Nächstes

- [ ] **Scanner v2: ersten Lauf in den Liquid Hours auswerten.** Alle bisherigen
  Läufe sind diagnostisch (nachts gestartet) — Open Interest liefert IB
  außerhalb der RTH nicht, Aktien haben dann keine Greeks. Der 09:35-ET-Job ist
  der erste belastbare Lauf. Danach Funnel einige Tage sammeln, **bevor** an
  Parametern gedreht wird.
  *Erwartung nach heutigem Stand:* IVR liegt marktweit im 1-Jahres-Tief
  (VIX 14,9 · MES-IVR 8 · MNQ 2,5 · M2K 3,6), das IVR-Gate ≥ 40 wird also
  vorerst fast alles verwerfen. Das ist das Regelwerk bei der Arbeit, kein Fehler.
- [ ] **Scanner v2: MES passt bei 25k knapp nicht** — MaxLoss 1.188,75 USD je
  Kontrakt gegen ein Budget von 1.168,36 USD (4 %). Fehlbetrag ~20 USD, also
  ~2 %. Kein Handlungsbedarf im Code (die emergente Aktivierung funktioniert
  genau so); nur zur Kenntnis, falls Max sich wundert, warum MES nie erscheint.
- [ ] **Stripe-E2E-Test im Testmodus** — wartet auf `STRIPE_PRICE_ID` (Max).
- [ ] **Frozen-Detection für Nicht-DD-Kennzahlen** (Verifier-Schicht 1, siehe
  `docs/verifier.md` §3): erkennt heute keinen eingefrorenen Feed, der
  plausible, aber unveränderte Werte liefert. Nicht launch-blockierend,
  solange keine Kennzahl als „verifiziert" ausgezeichnet wird (Regel steht).
- [ ] **Beispiel-Morgenmail als Vorschau** für Landingpage/Onboarding.
- [ ] `/morning`-Kundenseite: TL;DR oben, Mobil-Feinschliff.

## 🟠 Optional / Härtung (nicht Launch-blockierend)

- [ ] IB-abhängige Scheduler-Jobs (Scan/Monitor/Sync/Engine/Prewarm) für den
  Morgenroutine-only-Betrieb sauber behandeln (deaktivieren oder als Max-Werkzeug
  belassen — Entscheidung offen).
- [x] Security-Feinschliff erledigt (2026-08-10): CSP-Nonce-Härtung umgesetzt,
  `pip-audit` läuft wöchentlich als Scheduler-Job (Alarm nur bei Funden),
  Legacy-Klartext-Verify war bereits entfernt.
- [ ] Netz-Isolation Admin-Bereich (Level 2) — nur falls du es doch willst
  (verworfen zugunsten 2FA, da du weltweiten Zugriff brauchst).

## 🟢 Gemeinsam – Launch

- [ ] **Test-/Beta-Phase**: Zustellbarkeit (Spam!), Verständlichkeit, Feedback,
  Testimonials.
- [ ] **GoLive & Marketing** – nach Testphase; Umschalten Testphase → bezahlt.

---

## ✅ Erledigt (Kurzfassung, Details in STATUS.md)

**Sessions 2026-08-12/13:**
- [x] **PDF-Design-Runde** der Morgenroutine live (Website-Marke, Warndreieck,
  Sentiment-Tacho, einheitlicher DD-Status) — inkl. Web-Dienst-Neustart, ohne
  den der Commit produktiv nichts ändert
- [x] **Abo-Selbstverwaltung**: Kündigung ist bis zum Laufzeitende zurücknehmbar,
  Zustände sind auf der Abo-Seite eindeutig
- [x] **Automatische Verlängerung durchgängig kommuniziert** (Pflichtangaben,
  AGB § 6, Landingpage, Abo-Karte) + Erinnerungsmail vor der Jahresverlängerung
  — offen dazu nur der Webhook-Punkt bei Max oben
- [x] **Vola-Werte an die erwartete US-Session gebunden** (fail-closed): kein
  Nachweis der Frische ⇒ Gedankenstrich statt alter Zahl + Alarm an Max
- [x] **Öffentlicher Mirror aufgeräumt**: `TASKS.md` (Stand 05/2026, mit
  Executor-Altcode) neu geschrieben und archiviert, Mirror-README zeigt jetzt
  Spiegelzeitpunkt und Quellstand, Repo-Adresse überall korrigiert

**Session 2026-08-10 (Delivery Commit 2 abgeschlossen + Checkout-Legal):**
- [x] **Gestaffelter Tagesablauf** 07:45 Abruf / 07:50 Verifier-Gate / 07:55
  Rendering / 08:00 Versand (`bot/morning_delivery.py`) — vorher lief alles in
  einem Rutsch um 08:00, ein Gate-FAIL fiel erst nach dem Versand auf
- [x] **Fail-closed-Versand**: kein Gate ⇒ kein PDF ⇒ kein Versand (Mail *und*
  Telegram), Telegram-Alarm an Max, kein Fallback auf Vortagswerte
- [x] **Versandprotokoll je Kunde** (gesendet/fehlgeschlagen + Grund) für
  gezielten Nachversand; vom Server abgelehnte Empfänger galten vorher als
  zugestellt — behoben
- [x] **Telegram liefert das PDF** zusätzlich zum Dashboard-Foto
- [x] **Kein US-Handelstag ⇒ kein Lauf** (per Test abgesichert)
- [x] **Dezimaltrennzeichen vereinheitlicht** (Komma, `bot/formatting.py`)
- [x] **Checkout-Legal fertig**: § 312j-Buttonbeschriftung „Zahlungspflichtig
  bestellen", Pflichtangaben über dem Button, AGB-Checkbox serverseitig geprüft
- [x] **Basistests Kundenstrecke** (19 Tests): Archiv-Zugriffsregel inkl.
  IDOR-Fall, Traversal, Rollen-Gating, Bestellstrecke, CSRF
- [x] **Verifier-Abgrenzung dokumentiert** (`docs/verifier.md`): das
  „4-Schichten-Gate" ist kein drittes Modul, sondern der Bauplan der beiden
  vorhandenen; 3 von 4 Schichten stehen, die Lücke ist benannt
- [x] **Mail gekürzt** auf ein Anschreiben zum PDF, Betreff 45 statt 79 Zeichen
  und ohne Emoji (Spamfilter-Risiko bei täglichem Versand)
- [x] Vorbestehender Gateway-Testfehler ist nicht mehr reproduzierbar (grün)

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
- [x] **PDF-Rendering + Mail-Anhang + Portal-Archiv** (`/morgenroutine`,
  geschützte Download-Route, Zugriff über Abo-Fenster statt Startdatum)

**Früher:**
- [x] SMTP (IONOS), Domain `www.steadyalpha.de` (HTTPS/HSTS), Kündigungsbutton § 312k
- [x] Kunden-System (Registrierung, Double-Opt-in, Rollen/Pakete), Abo-Seite,
  Stripe-ready Datenmodell + Zahlungsstrecke (Code fertig, wartet auf Keys/PriceID)
- [x] Zustellwege E-Mail + Telegram, Kunden-Bot live
- [x] Auto-Trading vollständig entfernt (reines Signal-/Monitoring-System)
- [x] Morgenroutine kundenfertig, `/beispiel` live, Preise öffentlich
- [x] Tägliche Backups + Restore-Probe, lokale Fonts (kein CDN), Betriebs-Monitoring
- [x] Security-Baseline-Check 30/30
