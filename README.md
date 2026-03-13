# 🔐 SecureVault Light

Zero-Knowledge Passwort-Manager · AES-256-GCM · GitHub als verschlüsselter Cloud-Speicher.

[![Live Demo](https://img.shields.io/badge/Live-GitHub%20Pages-00e07a?style=flat-square&logo=github)](https://DEIN-USERNAME.github.io/securevault/)
[![Security](https://img.shields.io/badge/Crypto-AES--256--GCM-00e07a?style=flat-square)]()
[![Zero Knowledge](https://img.shields.io/badge/Architecture-Zero--Knowledge-00e07a?style=flat-square)]()

---

## Features

- 🔒 **AES-256-GCM** Verschlüsselung — jeder Eintrag einzeln
- 🔑 **PBKDF2** Key Derivation — 200.000 Iterationen, SHA-256
- ☁️ **GitHub API** als verschlüsselter Remote-Speicher
- 👁️ **Zero-Knowledge** — GitHub sieht nur verschlüsselte Blobs
- 🔄 **Conflict Handling** — SHA pre-check + 409 Resolver
- 🛡️ **Brute-Force Schutz** — 5 Versuche, dann 5min Lockout
- 📁 Projekte / Kategorien
- 🎲 Passwort-Generator (kryptografisch sicher)
- 🔍 Echtzeit-Suche

---

## Deployment auf GitHub Pages (5 Minuten)

### Schritt 1 — Repository erstellen

```bash
# Option A: GitHub CLI
gh repo create securevault --public --clone
cd securevault

# Option B: Manuell
# github.com → New repository → Name: "securevault" → Create
# Dann lokal klonen:
git clone https://github.com/DEIN-USERNAME/securevault.git
cd securevault
```

### Schritt 2 — Dateien hinzufügen

```bash
# index.html in den Ordner kopieren, dann:
git add index.html
git commit -m "🔐 SecureVault Light — initial deploy"
git push origin main
```

### Schritt 3 — GitHub Pages aktivieren

```
GitHub Repo → Settings → Pages
→ Source: Deploy from a branch
→ Branch: main / (root)
→ Save
```

Nach ~60 Sekunden live unter:
**`https://DEIN-USERNAME.github.io/securevault/`**

---

## Erste Benutzung

### GitHub Token erstellen (Fine-grained — empfohlen)

```
github.com/settings/personal-access-tokens/new

Token name:     SecureVault
Expiration:     nach Wunsch (365 Tage empfohlen)
Repository access: Only select repositories → securevault
Permissions:
  → Contents: Read and Write
  → Metadata: Read (automatisch)
```

### App einrichten

```
1. URL öffnen: https://DEIN-USERNAME.github.io/securevault/
2. Token eingeben → Weiter
3. Repository eingeben: DEIN-USERNAME/securevault → Verbinden
4. Account erstellen (erster Start)
5. Fertig — Vault ist bereit
```

---

## Sicherheitsarchitektur

```
Master-Passwort (nur im RAM)
        │
        ▼
PBKDF2 (200k Iterationen, SHA-256, Salt)
        │
        ├── Auth-Hash    → in vault.db.enc (Login-Verifikation)
        └── AES-256 Key  → nur im RAM, nie gespeichert
                │
                ▼
        Verschlüsselte Einträge (AES-256-GCM)
                │
                ▼
        GitHub API → vault.db.enc (nur verschlüsselter Blob)
```

**Was GitHub sieht:** Einen verschlüsselten Base64-Blob.
**Was GitHub NICHT sieht:** Passwörter, Benutzernamen, Master-Passwort, Encryption Key.

---

## Conflict Handling

Die App verhindert Datenverlust bei gleichzeitiger Nutzung:

```
Vor jedem Speichern:
  → SHA des aktuellen GitHub-Files prüfen
  → Abweichung erkannt → Konflikt-Dialog

Konflikt-Dialog:
  → GitHub-Version laden (empfohlen)
  → Eigene Version erzwingen (force-push)
  → Abbrechen
```

---

## Technologie

| Komponente | Technologie |
|------------|-------------|
| Verschlüsselung | Web Crypto API (nativ im Browser) |
| Key Derivation | PBKDF2 · 200k iter · SHA-256 |
| Symmetric Enc | AES-256-GCM · 12-Byte IV |
| Storage | GitHub Contents API |
| Hosting | GitHub Pages |
| Dependencies | **Keine** — pure Vanilla JS |

---

## Warum GitHub als Storage?

- **Zero Infrastrukturkosten** — 0€/Monat, 99.9% Uptime
- **Native Versionierung** — Git History als automatisches Backup
- **Zero-Knowledge by design** — GitHub kann nicht entschlüsseln
- **Fine-grained Access Control** — Token nur für dieses eine Repo

*Tradeoffs: Metadaten-Exposition (Sync-Zeitpunkte sichtbar), kein Granular-Access auf Eintrags-Ebene, API Rate Limits bei vielen gleichzeitigen Nutzern.*

---

*SecureVault Light ist die Basis-Version. Eine Team-Version mit Invite-System, Admin-Panel, geteilten Einträgen und Audit-Log ist in Entwicklung.*
