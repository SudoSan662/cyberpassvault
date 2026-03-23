# 🔐 SecureVault — Dev Branch

**Aktiver Entwicklungszweig. Hier entstehen neue Features, 
werden Architekturen getestet und Team-Funktionalität gebaut.**

> ⚠️ Dieser Branch ist instabil. Für die stabile Portfolio-Version:
> → [github.com/SudoSan662/Portfolio_SecureVault](https://github.com/SudoSan662/Portfolio_SecureVault)

---

## Was ist dieser Branch?

SecureVault ist ein Zero-Knowledge Passwort-Manager — 
browserbasiert, ohne Backend, ohne Dependencies.

Dieser Entwicklungszweig geht über die Basis-Version hinaus. 
Hier werden getestet:

- Team-Funktionalität (Invite-System, geteilte Einträge)
- Admin-Panel mit Benutzerverwaltung
- Audit-Log und Zugriffsprotokollierung
- Architektonische Entscheidungen vor dem Merge in main

---

## Aktueller Entwicklungsstand

| Modul | Status |
|---|---|
| AES-256-GCM Verschlüsselung | ✅ Stabil |
| PBKDF2 Key Derivation | ✅ Stabil |
| GitHub API Storage | ✅ Stabil |
| Conflict Handling / 409 Resolver | ✅ Stabil |
| Brute-Force Schutz | ✅ Stabil |
| Passwort-Generator | ✅ Stabil |
| Team Invite-System | 🔧 In Entwicklung |
| Admin-Panel | 🔧 In Entwicklung |
| Audit-Log | 🔧 In Entwicklung |
| Granulare Eintrags-Berechtigungen | 📋 Geplant |

---

## Sicherheitsarchitektur
```
Master-Passwort (nur im RAM)
        │
        ▼
PBKDF2 — 200.000 Iterationen · SHA-256 · Salt
        │
        ├── Auth-Hash     →  Login-Verifikation
        └── AES-256 Key   →  nur im RAM, wird nie gespeichert
                │
                ▼
        AES-256-GCM Verschlüsselung · pro Eintrag · 12-Byte IV
                │
                ▼
        GitHub API  →  vault.db.enc (verschlüsselter Blob)
```

**Zero-Knowledge Prinzip:**  
GitHub sieht ausschließlich einen verschlüsselten Base64-String. 
Master-Passwort, Encryption Key und Klartextdaten verlassen 
den Browser nie.

---

## Lokales Setup

### Voraussetzungen
- GitHub Account
- Personal Access Token (Fine-grained)
- Einen modernen Browser (Web Crypto API erforderlich)

### Token erstellen
```
github.com → Settings → Developer Settings
→ Personal access tokens → Fine-grained tokens → Generate new token

Token name:        SecureVault-Dev
Expiration:        nach Wunsch
Repository access: Only select repositories → dieses Repo

Permissions:
  → Contents:  Read and Write
  → Metadata:  Read (automatisch)
```

### Repository klonen & deployen
```bash
# Repository klonen
git clone https://github.com/SudoSan662/Portfolio_SecureVault.git
cd Portfolio_SecureVault

# Änderungen pushen
git add .
git commit -m "beschreibung der änderung"
git push origin main
```

### GitHub Pages aktivieren
```
Repo → Settings → Pages
→ Source: Deploy from a branch
→ Branch: main / (root)
→ Save

Live unter: https://DEIN-USERNAME.github.io/securevault/
```

---

## Conflict Handling

Verhindert Datenverlust bei gleichzeitiger Nutzung:
```
Vor jedem Speichern:
  → SHA des aktuellen GitHub-Files prüfen
  → Abweichung erkannt → Konflikt-Dialog öffnet sich

Konflikt-Dialog Optionen:
  → GitHub-Version laden    (empfohlen — neueste Version)
  → Eigene Version pushen   (force — überschreibt Remote)
  → Abbrechen               (keine Änderung)
```

---

## Technologie-Stack

| Komponente | Technologie |
|---|---|
| Verschlüsselung | Web Crypto API (nativ im Browser) |
| Key Derivation | PBKDF2 · 200k Iterationen · SHA-256 |
| Symmetric Enc | AES-256-GCM · 12-Byte IV |
| Storage | GitHub Contents API |
| Hosting | GitHub Pages |
| Dependencies | Keine — pure Vanilla JS |

---

## Bekannte Limitierungen (bewusste Tradeoffs)

- **Metadaten-Exposition** — Sync-Zeitpunkte in der Git-History 
  sichtbar
- **Rate Limits** — GitHub API limitiert bei vielen gleichzeitigen 
  Nutzern
- **Kein Granular-Access** — Token gilt für das gesamte Repository, 
  nicht einzelne Einträge
- **Kein Offline-Modus** — GitHub API erfordert Internetverbindung 
  beim Sync

---

## Roadmap

### v1.1 — Team Features (in Entwicklung)
- [ ] Invite-System per verschlüsseltem Link
- [ ] Geteilte Einträge mit Berechtigungsstufen
- [ ] Admin-Panel für Benutzerverwaltung

### v1.2 — Audit & Compliance
- [ ] Audit-Log aller Zugriffe und Änderungen
- [ ] Export-Funktion (verschlüsselt)
- [ ] Session-Timeout konfigurierbar

### v1.3 — UX & Performance
- [ ] Offline-Cache mit Service Worker
- [ ] Import aus anderen Passwort-Managern
- [ ] Dark / Light Mode

---

## Branch-Strategie
```
main          →  Stabile Portfolio-Version (kein direkter Push)
dev           →  Dieser Branch — aktive Entwicklung
feature/*     →  Einzelne Features (merge into dev)
```

> Direkte Pushes auf `main` vermeiden.  
> Features über `dev` entwickeln und testen, dann PR auf `main`.

---

## Lizenz & Kontext

Dieses Projekt ist Teil eines persönlichen Portfolios 
im Bereich IT-Security & Webentwicklung.

📬 [github.com/SudoSan662](https://github.com/SudoSan662)