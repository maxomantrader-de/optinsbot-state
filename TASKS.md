# TASKS.md – Auftrags-Schnittstelle Sepp ↔ Developer Agent

**Stand:** 2026-08-13 · **Gepflegt vom:** Developer Agent · **Auftraggeber:** Sepp (Claude Project) / Max

> Diese Datei wird zusammen mit `STATUS.md` und `TODO.md` per GitHub Action
> `mirror-state` in das öffentliche Read-only-Repo gespiegelt
> (`maxomantrader-de/optinsbot-state` — Schreibweise ohne zweites „o", siehe
> `.github/workflows/mirror-state.yml`). Keine Zugangsdaten, keine
> personenbezogenen Daten, keine Kontodaten hier eintragen.

---

## 📚 Welche Datei wofür (Rangfolge)

| Datei | Inhalt | Wer pflegt |
|---|---|---|
| `CLAUDE.md` + `config/settings.py` | **Quelle der Wahrheit** für Regelwerk, Parameter, Infrastruktur | Developer Agent |
| `REGELWERK.md` | daraus abgeleitete, menschenlesbare Fassung | Developer Agent |
| `TODO.md` | **die einzige gepflegte Liste offener Punkte** (GoLive-Roadmap, nach Max / Agent getrennt) | Developer Agent |
| `STATUS.md` | Changelog + aktueller Fokus, neueste Einträge oben | alle Agenten |
| `TASKS.md` (diese Datei) | Auftragsschnittstelle: wie Sepp Aufträge stellt, Status der Auftragsserien | Developer Agent |

**Regel:** Offene Arbeit steht in `TODO.md`, nicht hier. Diese Datei ist der
Index der Auftragsserien und der Beleg, was aus ihnen geworden ist.

---

## 🔌 Wie Sepp heute Aufträge stellt

Der ursprüngliche Weg (Sepp committet Blöcke in eine Datei `SEPP_QUEUE.md`,
ein Watcher pollt sie) ist **abgelöst**. `SEPP_QUEUE.md.deprecated` liegt nur
noch als Beleg im Repo.

Aktueller Weg (seit 07/2026, in Betrieb):

```
Sepp (claude.ai)
   └─► HTTP-Endpoint (Blueprint in web/app.py, sepp_receiver.py, geschützt + Rate-Limit)
         └─► sepp_tasks/queue/<id>.json
               └─► sepp_runner.py  (systemd: sepp-runner.service, Poll 10 s)
                     ├─ führt den Auftrag über die Claude-CLI aus
                     ├─ verschiebt nach sepp_tasks/done/ bzw. sepp_tasks/failed/
                     └─ meldet das Ergebnis per Telegram an Max
```

Ergänzend läuft `sepp-watcher.service` (`sepp_watcher.py`) als Git-seitiger
Wächter. Beide Dienste laufen unter dem Service-User `optionsbot`.

**Umfangreiche Aufträge** (mehrseitige Spezifikationen) kommen weiterhin als
eigene Markdown-Datei in die Repo-Wurzel und werden hier unten verlinkt.

---

## 🟢 Offene Aufträge

| # | Auftrag | Liegt bei | Stand |
|---|---|---|---|
| 6 | **Domain-Migration abschließen** — `www.steadyalpha.de` ist live (HTTPS, HSTS, Apex→www, 301 von der Alt-Domain). Offen ist nur noch, was darüber hinaus migriert werden soll und wo das DNS liegt. | **Max** | wartet auf Rückmeldung, siehe `TODO.md` |

Sonst sind **keine** Sepp-Aufträge offen. Alle weiteren offenen Punkte —
Stripe-PriceID, DNS/Zustellbarkeit, Offsite-Backup, Uptime-Check, 2FA-Aktivierung,
Beta-Nutzer sowie die Agent-Aufgaben (Stripe-E2E-Test, Frozen-Detection für
Nicht-DD-Kennzahlen, Beispiel-Morgenmail, `/morning`-Feinschliff) — stehen in
[`TODO.md`](TODO.md).

---

## ✅ Erledigte Auftragsserien

### Serie B — Produkt & Auslieferung (07–08/2026)

| # | Auftrag | Stand | Nachweis |
|---|---|---|---|
| 1 | **Regelwerk konsolidieren** — eine abgeleitete Quelle, Divergenzen aufgelöst (DTE 45–70/55 verbindlich, Universum 30, Positionslimit 4, Profit-Target 60 %) | erledigt 2026-08-03 | `REGELWERK.md`, STATUS-Eintrag 03.08. |
| 5 | **Morgenroutine als druckreifes 2-Seiten-PDF** inkl. Datenkontrakt, fail-closed Verifier-Gate, Auslieferung an Kunden (Mail + Telegram + Portal-Archiv) | erledigt 2026-08-10, Design-Runde 2026-08-12 | `docs/morning_delivery.md`, `docs/morning_payload.schema.json`, STATUS-Einträge 24.07. / 03.08. / 10.08. / 12.08. |

**Auftragsdokumente mit eigener Datei** (alle abgearbeitet, bleiben als
Herleitung liegen):

- [`TASK_morgenroutine.md`](TASK_morgenroutine.md) — Erstauftrag Morgenroutine (Daten, Speicherung, Web-UI)
- [`TASKS_DD_POLICY.md`](TASKS_DD_POLICY.md) — Kalibrierung der Distribution-Days-Wirkung (`watch` / `alarm`), 11-Jahres-Backtest
- [`TASKS_DD_VERIFIER.md`](TASKS_DD_VERIFIER.md) — Publikations-Gate mit 8 Checks, fail-closed
- [`TASKS_SCANNER_KALIBRIERUNG.md`](TASKS_SCANNER_KALIBRIERUNG.md) — Audit des Optionsstrategen (Rejection-Funnel, Filter-Kalibrierung)
- [`AUDIT_MORGENROUTINE.md`](AUDIT_MORGENROUTINE.md) — Compliance-/Formulierungs-Audit der Kundenansicht
- [`TODO_MORGENROUTINE.md`](TODO_MORGENROUTINE.md) — Restpunkte der Morgenroutine-Auslieferung

### Serie A — Code-Bootstrapping (05/2026)

| # | Auftrag | Stand | Commit |
|---|---|---|---|
| 1 | `FuturesOption` statt `Option` für das FOP-Universum | erledigt | `f7b1887` |
| 2 | Combo-Exchange für Futures-Optionen | erledigt | `f7b1887` |
| 3 | `requirements.txt` vervollständigen | erledigt | `3408a2e` |
| 4 | Sepp-Bridge (Git-Task-Watcher) | erledigt, seither durch den HTTP-Weg abgelöst | `5d085fa` |

Der Volltext dieser Serie liegt in
[`docs/archiv/TASKS_ARCHIVIERT_2026-05-06.md`](docs/archiv/TASKS_ARCHIVIERT_2026-05-06.md).
Er ist **historisch und gilt nicht mehr**: Er enthält u. a. Code für einen
Order-Executor. Automatischer Handel wurde vollständig entfernt — das System
scannt, bewertet und alarmiert, alle Orders platziert Max manuell bei IBKR.
Order-platzierender Code darf nicht wieder eingeführt werden (`CLAUDE.md`,
§ Handelsstrategie).
