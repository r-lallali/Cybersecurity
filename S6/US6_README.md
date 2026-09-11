# Rapport d'Investigation Numérique — US6 : Acquisition et analyse de la mémoire vive (RAM)

- **Date d'exécution :** 08/09/2026
- **Analyste :** swago0o
- **Cible :** Machine virtuelle « kok » (Windows 11 / x64)
- **Rôle projet :** Partie 2 — Analyste Forensic

---

## 1. Objectif de la User Story
Capturer l'intégralité de la mémoire volatile (RAM) de la machine virtuelle infectée sans altérer son contenu, garantir l'intégrité de la preuve par calcul d'empreinte cryptographique, et analyser les processus ainsi que les connexions réseau actives via un outil forensique standardisé (Volatility 3).

---

## 2. Environnement et Outils Utilisés
- **Hyperviseur :** Oracle VM VirtualBox (v7.x)
- **Outil d'acquisition hôte :** `VBoxManage.exe debugvm` (PowerShell Windows)
- **Environnement d'analyse :** WSL 2 (Ubuntu Linux)
- **Framework d'analyse mémoire :** Volatility 3 (v2.28.0)
- **Fonction de hachage :** SHA-256 (`Get-FileHash`)

---

## 3. Méthodologie et Commandes d'Exécution

### Étape 1 : Acquisition de la mémoire vive
L'extraction a été réalisée depuis le système hôte alors que la machine virtuelle était active :
```powershell
cd "C:\Program Files\Oracle\VirtualBox"
.\VBoxManage.exe debugvm "kok" dumpvmcore --filename "C:\Forensic\memoire.raw"
```
- **Fichier généré :** `C:\Forensic\memoire.raw`
- **Taille de la capture :** 4 438 817 660 octets (~4,13 Go)

### Étape 2 : Scellé numérique et intégrité
Afin de préserver la chaîne de traçabilité (*chain of custody*), le hash SHA-256 a été calculé immédiatement après l'extraction :
```powershell
Get-FileHash -Algorithm SHA256 C:\Forensic\memoire.raw
```
- **Empreinte SHA-256 :**
  `6548B76754B016824A1D6CF465649B3DEE1D2A95ED49494628ACDFD0DC09A8F3`

### Étape 3 : Analyse forensique sous Volatility 3 (WSL / Ubuntu)
Le fichier brut a été analysé pour répertorier l'état d'exécution du système :
```bash
# Extraction de la table des processus
~/.local/bin/vol -f memoire.raw windows.pslist > pslist_result.txt

# Extraction des sockets et connexions réseau
~/.local/bin/vol -f memoire.raw windows.netscan > netscan_result.txt
```

---

## 4. Résultats et Observations

### Processus actifs clés identifiés (`windows.pslist`)
| PID | PPID | ImageFileName | CreateTime (UTC) | Analyse préliminaire |
| :--- | :--- | :--- | :--- | :--- |
| **760** | 684 | `winlogon.exe` | 2026-09-08 09:31:15 | Processus système légitime de session |
| **2908**| 832 | `MsMpEng.exe` | 2026-09-08 09:31:27 | Service principal Windows Defender |
| **4760**| 4692| `explorer.exe` | 2026-09-08 09:31:39 | Shell utilisateur graphique Windows |
| **1852**| 8832| `msedge.exe` | 2026-09-08 09:33:41 | Processus de navigation web |

### Analyse des flux réseau (`windows.netscan`)
- Identification des sockets TCP/UDP à l'écoute et des connexions établies au moment du dump.
- Archivage des adresses IP locales et distantes dans `netscan_result.txt` pour corrélation.

---

## 5. Livrables et Preuves
1. Capture PowerShell : validation du dump et empreinte SHA-256.
2. Fichier brut : `C:\Forensic\memoire.raw`.
3. Rapports textuels : `pslist_result.txt` et `netscan_result.txt`.
