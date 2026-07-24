# TASKS.md – Aufgaben für Developer Agent
**Zuletzt aktualisiert:** 2026-05-06 | **Von:** Sepp (Projekt-Agent)

---

## ⚠️ Arbeitsanweisung

Lies zuerst `CLAUDE.md` für den vollständigen Projekt-Kontext.
Arbeite die Tasks **in der Reihenfolge** ab. Markiere jeden abgeschlossenen Task mit `[DONE]` und committe + pushe danach.

---

## 🔴 TASK 1 – FuturesOption statt Option (KRITISCH)

**Problem:** `executor.py` und `manager.py` verwenden `Option(...)` für alle Symbole. Das ist falsch. Futures-Optionen müssen als `FuturesOption` mit `secType="FOP"` bei IB angefragt werden. Mit `Option` schlägt jede Kontrakt-Qualifizierung für das gesamte Universum (MES, CL, GC etc.) fehl.

**Dateien:** `bot/executor.py`, `bot/manager.py`

### executor.py – Funktion `_qualify_option`

**IST:**
```python
def _qualify_option(ib: IB, symbol: str, expiration: str,
                     strike: float, right: str = "P") -> Optional[Contract]:
    contract = Option(symbol, expiration, strike, right, "SMART", "100", "USD")
    try:
        qualified = ib.qualifyContracts(contract)
        return qualified[0] if qualified else None
    except Exception as e:
        log.error(f"Qualify fehlgeschlagen {symbol} {strike}{right}: {e}")
        return None
```

**SOLL:**
```python
def _qualify_option(ib: IB, symbol: str, expiration: str,
                     strike: float, right: str = "P") -> Optional[Contract]:
    """Qualifiziert einen Optionskontrakt – OPT für Aktien/ETFs, FOP für Futures."""
    from config import settings as cfg
    from ib_insync import FuturesOption

    if symbol in cfg.FUTURES_CONFIG:
        fc = cfg.FUTURES_CONFIG[symbol]
        exchange   = fc["exchange"]
        multiplier = str(fc["multiplier"])
        root       = fc.get("root_symbol", symbol)
        contract   = FuturesOption(root, expiration, strike, right, exchange, multiplier, "USD")
    else:
        contract = Option(symbol, expiration, strike, right, "SMART", "100", "USD")

    try:
        qualified = ib.qualifyContracts(contract)
        return qualified[0] if qualified else None
    except Exception as e:
        log.error(f"Qualify fehlgeschlagen {symbol} {strike}{right}: {e}")
        return None
```

### manager.py – Funktion `_fetch_spread_data`

**IST:**
```python
short_opt = Option(
    position.symbol, position.expiration,
    position.short_strike, "P", "SMART", "100", "USD",
)
long_opt = Option(
    position.symbol, position.expiration,
    position.long_strike, "P", "SMART", "100", "USD",
)
```

**SOLL:**
```python
from ib_insync import FuturesOption
from config import settings as cfg

def _make_option_contract(symbol: str, expiration: str, strike: float, right: str = "P") -> Contract:
    """Erstellt den korrekten Kontrakt-Typ: FOP für Futures, OPT für Aktien/ETFs."""
    if symbol in cfg.FUTURES_CONFIG:
        fc = cfg.FUTURES_CONFIG[symbol]
        root = fc.get("root_symbol", symbol)
        return FuturesOption(root, expiration, strike, right, fc["exchange"], str(fc["multiplier"]), "USD")
    return Option(symbol, expiration, strike, right, "SMART", "100", "USD")

# Dann in _fetch_spread_data:
short_opt = _make_option_contract(position.symbol, position.expiration, position.short_strike)
long_opt  = _make_option_contract(position.symbol, position.expiration, position.long_strike)
```

**Test nach Änderung:**
```bash
cd ~/optionsbot && source venv/bin/activate && python -m pytest tests/ -v
```

**Commit:**
```bash
git add bot/executor.py bot/manager.py
git commit -m "fix: FuturesOption statt Option fuer Futures-Symbole (FOP vs OPT)"
git push
```

---

## 🔴 TASK 2 – Combo Exchange für Futures-Optionen (KRITISCH)

**Problem:** `_build_combo` in `executor.py` setzt `combo.exchange = "SMART"`. SMART-Routing existiert bei IB nicht für Futures-Optionen (FOP). Die Exchange muss explizit gesetzt werden (CME, NYMEX, COMEX, CBOT).

**Datei:** `bot/executor.py`

### Funktion `_build_combo`

**IST:**
```python
def _build_combo(symbol: str, short_con_id: int, long_con_id: int) -> Contract:
    ...
    combo = Contract()
    combo.symbol     = symbol
    combo.secType    = "BAG"
    combo.currency   = "USD"
    combo.exchange   = "SMART"
    combo.comboLegs  = [short_leg, long_leg]
    return combo
```

**SOLL:**
```python
def _build_combo(symbol: str, short_con_id: int, long_con_id: int) -> Contract:
    """Baut einen BAG-Kontrakt (Combo) für den Spread.
    Exchange wird per Symbol aus FUTURES_CONFIG geholt; Fallback: SMART für OPT.
    """
    from config import settings as cfg

    short_leg = ComboLeg()
    short_leg.conId    = short_con_id
    short_leg.ratio    = 1
    short_leg.action   = "SELL"
    short_leg.exchange = "SMART"

    long_leg = ComboLeg()
    long_leg.conId    = long_con_id
    long_leg.ratio    = 1
    long_leg.action   = "BUY"
    long_leg.exchange = "SMART"

    # Exchange für Futures-Optionen explizit setzen
    if symbol in cfg.FUTURES_CONFIG:
        exchange = cfg.FUTURES_CONFIG[symbol]["exchange"]
    else:
        exchange = "SMART"

    combo = Contract()
    combo.symbol     = symbol
    combo.secType    = "BAG"
    combo.currency   = "USD"
    combo.exchange   = exchange
    combo.comboLegs  = [short_leg, long_leg]
    return combo
```

**Commit:**
```bash
git add bot/executor.py
git commit -m "fix: Combo Exchange per Symbol aus FUTURES_CONFIG (nicht SMART fuer FOP)"
git push
```

---

## 🔴 TASK 3 – requirements.txt vervollständigen (KRITISCH)

**Problem:** `requirements.txt` ist unvollständig. Flask, firebase-admin und weitere Dependencies fehlen → Bot und Web-Interface starten nicht.

**Datei:** `requirements.txt`

**SOLL (komplette Datei):**
```
# IB API
ib_insync>=0.9.86

# Scheduling
APScheduler>=3.10

# Market Calendars & Holidays
holidays>=0.45

# HTTP / Telegram
requests>=2.31

# Data & Math
scipy>=1.12
pandas>=2.0
numpy>=1.26

# Technical Indicators (RSI, MA200)
pandas-ta>=0.3.14b

# Web Interface
flask>=3.0
flask-session>=0.8

# Firebase / Firestore (optional – graceful fallback wenn credentials.json fehlt)
firebase-admin>=6.5

# Utilities
python-dotenv>=1.0
```

**Nach dem Ändern – Dependencies installieren und testen:**
```bash
cd ~/optionsbot
source venv/bin/activate
pip install -r requirements.txt
python -m pytest tests/ -v
python -c "import flask; import ib_insync; import pandas_ta; print('OK – alle Imports erfolgreich')"
```

**Commit:**
```bash
git add requirements.txt
git commit -m "fix: requirements.txt vervollstaendigt (flask, firebase-admin, pandas-ta, numpy)"
git push
```

---

## ✅ Abschluss-Check (nach Task 1–3)

Nach allen drei Tasks bitte folgenden Gesamt-Check ausführen:

```bash
cd ~/optionsbot
source venv/bin/activate

# 1. Alle Tests
python -m pytest tests/ -v

# 2. Import-Check aller Module
python -c "
from bot.scanner import run_scanner
from bot.executor import place_entry_order, _build_combo, _qualify_option
from bot.manager import run_manager
from bot.rules import check_entry, check_exit
from journal.trades import TradeJournal
from config import settings as cfg
print('✓ Alle Imports erfolgreich')
print(f'✓ UNIVERSE: {cfg.UNIVERSE}')
print(f'✓ IB Port: {cfg.IB_PORT}')
"

# 3. Web-App Syntax-Check
python -c "import web.app; print('✓ Flask App geladen')"
```

Ergebnisse im Commit-Message dokumentieren.

---

## 📋 Task-Status

| Task | Status | Commit |
|---|---|---|
| Task 1 – FuturesOption fix | [DONE] | f7b1887 |
| Task 2 – Combo Exchange fix | [DONE] | f7b1887 |
| Task 3 – requirements.txt | [DONE] | 3408a2e |
| Abschluss-Check | [DONE] | 3408a2e – 38/38 tests pass, alle Imports OK |

---

## 🔴 TASK 4 – Git-basierter Task-Watcher (Sepp-Bridge) [KRITISCH]

**Von:** Sepp (Projekt-Agent) | **Datum:** 2026-05-06

**Ziel:** Ich (Sepp auf claude.ai) kann künftig Tasks direkt ins GitHub-Repo schreiben.
Der Watcher auf dem VPS erkennt neue Tasks automatisch, führt sie via Claude Code aus
und schickt das Ergebnis per Telegram an Max. Max muss nie mehr manuell vermitteln.

---

### Architektur

```
Sepp (claude.ai) → git push → SEPP_QUEUE.md
                                    ↓
                         sepp-watcher.service (alle 30s git pull)
                                    ↓
                         claude --print (Task ausführen)
                                    ↓
                         Telegram → Max
                         git push → SEPP_QUEUE.md (Status: DONE)
```

---

### Schritt 1: SEPP_QUEUE.md anlegen

Erstelle `/home/optionsbot/SEPP_QUEUE.md` mit folgendem Inhalt:

```markdown
# SEPP_QUEUE – Task-Warteschlange
# Format: Jeder Task ist ein Block zwischen --- Trennern
# Status: QUEUED | RUNNING | DONE | FAILED

```

Die Datei wird leer erstellt – Sepp füllt sie mit Tasks.

---

### Schritt 2: sepp_watcher.py erstellen

Erstelle `/home/optionsbot/sepp_watcher.py`:

```python
"""
sepp_watcher.py – Git-basierter Task-Watcher für Sepp-Bridge.

Läuft als systemd-Daemon. Pollt alle 30 Sekunden SEPP_QUEUE.md im Git-Repo.
Neue Tasks (Status: QUEUED) werden via Claude Code CLI ausgeführt.
Ergebnis wird per Telegram gesendet und Status in SEPP_QUEUE.md aktualisiert.
"""

from __future__ import annotations

import json
import logging
import os
import re
import subprocess
import sys
import time
from datetime import datetime, timezone
from pathlib import Path

logging.basicConfig(
    level=logging.INFO,
    format="%(asctime)s  %(levelname)-8s  %(name)s  %(message)s",
    datefmt="%Y-%m-%d %H:%M:%S",
)
log = logging.getLogger("sepp_watcher")

REPO_DIR      = Path("/home/optionsbot")
QUEUE_FILE    = REPO_DIR / "SEPP_QUEUE.md"
POLL_INTERVAL = 30  # Sekunden
CLAUDE_TIMEOUT = 600  # 10 Minuten max pro Task

sys.path.insert(0, str(REPO_DIR))
from config import settings as cfg


# ---------------------------------------------------------------------------
# Telegram
# ---------------------------------------------------------------------------

def send_telegram(message: str) -> None:
    try:
        import requests
        url = f"https://api.telegram.org/bot{cfg.TELEGRAM_TOKEN}/sendMessage"
        requests.post(url, json={
            "chat_id":    cfg.TELEGRAM_CHAT_ID,
            "text":       message,
            "parse_mode": "HTML",
        }, timeout=10)
    except Exception as e:
        log.error(f"Telegram-Fehler: {e}")


# ---------------------------------------------------------------------------
# Git-Operationen
# ---------------------------------------------------------------------------

def git_pull() -> bool:
    """Pullt neuesten Stand vom Repo. Gibt True zurück wenn Änderungen da waren."""
    result = subprocess.run(
        ["git", "pull", "--rebase", "origin", "main"],
        capture_output=True, text=True, cwd=REPO_DIR, timeout=30,
    )
    if result.returncode != 0:
        log.error(f"git pull fehlgeschlagen: {result.stderr}")
        return False
    changed = "Already up to date." not in result.stdout
    if changed:
        log.info(f"git pull: Änderungen erkannt")
    return changed


def git_push(message: str) -> bool:
    """Committed und pusht SEPP_QUEUE.md."""
    try:
        subprocess.run(
            ["git", "add", "SEPP_QUEUE.md"],
            cwd=REPO_DIR, check=True, timeout=15,
        )
        subprocess.run(
            ["git", "commit", "-m", message, "--allow-empty"],
            cwd=REPO_DIR, check=True, timeout=15,
        )
        subprocess.run(
            ["git", "push", "origin", "main"],
            cwd=REPO_DIR, check=True, timeout=30,
        )
        return True
    except Exception as e:
        log.error(f"git push fehlgeschlagen: {e}")
        return False


# ---------------------------------------------------------------------------
# Queue parsen
# ---------------------------------------------------------------------------

TASK_PATTERN = re.compile(
    r"<!--TASK_START-->\s*"
    r"id:\s*(?P<id>\S+)\s*\n"
    r"status:\s*(?P<status>\S+)\s*\n"
    r"created:\s*(?P<created>[^\n]+)\s*\n"
    r"task:\s*(?P<task>.*?)\s*"
    r"<!--TASK_END-->",
    re.DOTALL,
)


def parse_queue() -> list[dict]:
    """Liest alle Tasks aus SEPP_QUEUE.md."""
    if not QUEUE_FILE.exists():
        return []
    content = QUEUE_FILE.read_text(encoding="utf-8")
    tasks = []
    for m in TASK_PATTERN.finditer(content):
        tasks.append({
            "id":      m.group("id"),
            "status":  m.group("status"),
            "created": m.group("created"),
            "task":    m.group("task").strip(),
        })
    return tasks


def update_task_status(task_id: str, status: str, result: str = "") -> None:
    """Aktualisiert Status und Ergebnis eines Tasks in SEPP_QUEUE.md."""
    content = QUEUE_FILE.read_text(encoding="utf-8")

    def replacer(m: re.Match) -> str:
        if m.group("id") != task_id:
            return m.group(0)
        result_block = f"\nresult: {result[:2000]}" if result else ""
        completed = f"\ncompleted: {datetime.now(timezone.utc).isoformat()}"
        return (
            f"<!--TASK_START-->\n"
            f"id: {m.group('id')}\n"
            f"status: {status}\n"
            f"created: {m.group('created')}\n"
            f"task: {m.group('task')}"
            f"{completed}"
            f"{result_block}\n"
            f"<!--TASK_END-->"
        )

    updated = TASK_PATTERN.sub(replacer, content)
    QUEUE_FILE.write_text(updated, encoding="utf-8")


# ---------------------------------------------------------------------------
# Task ausführen
# ---------------------------------------------------------------------------

def run_task(task: dict) -> str:
    """Führt Task via Claude Code CLI aus. Gibt Output zurück."""
    task_id   = task["id"]
    task_text = task["task"]

    log.info(f"[{task_id}] Starte: {task_text[:100]}")

    # Status auf RUNNING setzen
    update_task_status(task_id, "RUNNING")
    git_push(f"task {task_id}: RUNNING")

    send_telegram(
        f"🤖 <b>Sepp-Bridge</b>\n"
        f"Task <code>{task_id}</code> gestartet:\n"
        f"<i>{task_text[:300]}</i>"
    )

    # Claude Code CLI aufrufen
    result = subprocess.run(
        ["claude", "--print", "--dangerously-skip-permissions", task_text],
        capture_output=True,
        text=True,
        timeout=CLAUDE_TIMEOUT,
        cwd=str(REPO_DIR),
    )

    output = result.stdout.strip() or result.stderr.strip() or "(kein Output)"

    if result.returncode != 0:
        raise RuntimeError(output)

    return output


# ---------------------------------------------------------------------------
# Hauptschleife
# ---------------------------------------------------------------------------

def main() -> None:
    log.info("Sepp-Watcher gestartet")
    send_telegram("🚀 <b>Sepp-Bridge aktiv</b>\nGit-Watcher läuft. Bereit für Tasks von Sepp.")

    while True:
        try:
            git_pull()

            tasks = parse_queue()
            queued = [t for t in tasks if t["status"] == "QUEUED"]

            for task in queued:
                task_id = task["id"]
                try:
                    output = run_task(task)

                    update_task_status(task_id, "DONE", output)
                    git_push(f"task {task_id}: DONE")

                    short = output[:3000] + ("…" if len(output) > 3000 else "")
                    send_telegram(
                        f"✅ <b>Task {task_id} erledigt</b>\n\n<pre>{short}</pre>"
                    )
                    log.info(f"[{task_id}] Erfolgreich abgeschlossen")

                except Exception as e:
                    log.error(f"[{task_id}] Fehlgeschlagen: {e}")
                    update_task_status(task_id, "FAILED", str(e))
                    git_push(f"task {task_id}: FAILED")
                    send_telegram(
                        f"❌ <b>Task {task_id} fehlgeschlagen</b>\n\n"
                        f"<code>{str(e)[:500]}</code>"
                    )

        except Exception as e:
            log.error(f"Watcher-Fehler: {e}")

        time.sleep(POLL_INTERVAL)


if __name__ == "__main__":
    main()
```

---

### Schritt 3: systemd Service erstellen

Erstelle `/etc/systemd/system/sepp-watcher.service`:

```ini
[Unit]
Description=Sepp Bridge – Git Task Watcher
After=network.target
Wants=network-online.target

[Service]
Type=simple
User=root
WorkingDirectory=/home/optionsbot
ExecStart=/home/optionsbot/venv/bin/python /home/optionsbot/sepp_watcher.py
Restart=always
RestartSec=15
StandardOutput=journal
StandardError=journal
Environment=GIT_ASKPASS=/bin/true

[Install]
WantedBy=multi-user.target
```

Aktivieren und starten:
```bash
sudo systemctl daemon-reload
sudo systemctl enable sepp-watcher.service
sudo systemctl start sepp-watcher.service
sudo systemctl status sepp-watcher.service
```

---

### Schritt 4: Git-Credentials für automatischen Push einrichten

Der Watcher muss ohne manuelle Eingabe pushen können. Prüfe ob bereits ein Credential-Helper konfiguriert ist:

```bash
git -C /home/optionsbot config credential.helper
```

Falls leer – trage den GitHub-Token in die Git-Config ein:

```bash
git -C /home/optionsbot remote set-url origin \
  https://REMOVED_TOKEN@github.com/maxomantrader-de/optionsbot.git
```

Teste anschließend:
```bash
cd /home/optionsbot && git push origin main --dry-run
```

---

### Schritt 5: SEPP_QUEUE.md anlegen und committen

```bash
cat > /home/optionsbot/SEPP_QUEUE.md << 'EOF'
# SEPP_QUEUE – Task-Warteschlange
# Sepp (claude.ai) schreibt hier Tasks rein. Watcher führt sie aus.
# Format: Blöcke zwischen <!--TASK_START--> und <!--TASK_END-->

EOF

cd /home/optionsbot
git add SEPP_QUEUE.md
git commit -m "feat: SEPP_QUEUE.md initialisiert"
git push
```

---

### Schritt 6: Test-Task einfügen und verifizieren

Füge manuell einen Test-Task in SEPP_QUEUE.md ein um den Watcher zu testen:

```bash
cat >> /home/optionsbot/SEPP_QUEUE.md << 'EOF'

<!--TASK_START-->
id: test-001
status: QUEUED
created: 2026-05-06T17:30:00Z
task: Führe folgende Shell-Befehle aus und berichte das Ergebnis:
echo "=== Sepp-Bridge Test ===" && date && echo "VPS:" && hostname && echo "Services:" && systemctl is-active optionsbot.service optionsbot-web.service sepp-watcher.service && echo "Git:" && cd /home/optionsbot && git log --oneline -3
<!--TASK_END-->
EOF

cd /home/optionsbot
git add SEPP_QUEUE.md
git commit -m "test: Sepp-Bridge Test-Task (test-001)"
git push
```

Warte 60 Sekunden – dann sollte eine Telegram-Nachricht mit dem Ergebnis ankommen.

---

### Abschluss-Meldung an Sepp

Nach erfolgreichem Test, schreibe folgende Bestätigung als Antwort in claude.ai:

```
Sepp-Bridge (Git-Watcher) implementiert:
- sepp_watcher.py: [OK/FEHLER]
- sepp-watcher.service: [AKTIV/FEHLER]
- SEPP_QUEUE.md: [ERSTELLT/FEHLER]
- Git Push/Pull ohne Passwort: [OK/FEHLER]
- Test-Task test-001: [DONE/FAILED]
- Telegram-Nachricht empfangen: [JA/NEIN]
```

---

## 📋 Task-Status

| Task | Status | Commit |
|---|---|---|
| Task 1 – FuturesOption fix | [DONE] | f7b1887 |
| Task 2 – Combo Exchange fix | [DONE] | f7b1887 |
| Task 3 – requirements.txt | [DONE] | 3408a2e |
| Abschluss-Check | [DONE] | 3408a2e |
| Task 4 – Sepp-Bridge Git-Watcher | [DONE] | 5d085fa |
