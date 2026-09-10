# Rapport d'Investigation Numérique — US7 : Acquisition bit-à-bit et analyse du disque dur

- **Date d'exécution :** 08/09/2026
- **Analyste :** swago0o
- **Cible :** Machine virtuelle « kok » (Disque virtuel `kok.vdi`)
- **Rôle projet :** Partie 2 — Analyste Forensic

---

## 1. Objectif de la User Story
Réaliser une copie conforme bit-à-bit du support de stockage de la VM à l'état éteint afin de garantir la non-altération des preuves, calculer l'empreinte d'intégrité, et analyser les artéfacts d'exécution et de persistance à l'aide d'Autopsy.

---

## 2. Environnement et Outils Utilisés
- **Hyperviseur :** Oracle VM VirtualBox (v7.x)
- **Outil de clonage RAW :** `VBoxManage.exe clonemedium` (PowerShell Windows)
- **Outil d'analyse forensique :** Autopsy Forensic Browser (v4.23.1)
- **Fonction de hachage :** SHA-256 (`Get-FileHash`)

---

## 3. Méthodologie et Commandes d'Exécution

### Étape 1 : Clonage bit-à-bit du disque dur
Après extinction propre de la machine virtuelle :
```powershell
cd "C:\Program Files\Oracle\VirtualBox"
.\VBoxManage.exe clonemedium disk "C:\Users\swago0o\VirtualBox VMs\kok\kok.vdi" "C:\Forensic\disque.raw" --format RAW
```
- **Statut :** Conversion effectuée à 100%.
- **Format cible :** Image brute non compressée (RAW).

### Étape 2 : Calcul de l'empreinte cryptographique
Vérification immédiate de l'intégrité du fichier image généré :
```powershell
Get-FileHash -Algorithm SHA256 C:\Forensic\disque.raw
```

### Étape 3 : Ingestion et indexation dans Autopsy
1. Création d'un cas d'investigation : `Enquete_Malware_kok`.
2. Ajout de la source de données : fichier image `C:\Forensic\disque.raw`.
3. Modules d'ingestion exécutés : *Recent Activity*, *File Type Identification*, *Extension Mismatch Detector*.

---

## 4. Résultats et Observations (Artéfacts Disque)

### Programmes exécutés récents (`Data Artifacts > Run Programs`)
L'analyse des journaux d'exécution et du Prefetch a révélé 32 programmes exécutés récemment :
| Source | Nom du programme / Chemin complet | Utilisateur | Horodatage (CEST) | Commentaire artéfact |
| :--- | :--- | :--- | :--- | :--- |
| **SYSTEM** | `\Windows\System32\winlogon.exe` | SYSTEM | 2026-09-08 10:22:10 | Trace Prefetch |
| **SYSTEM** | `\Windows\System32\conhost.exe` | SYSTEM | 2026-09-08 10:33:04 | Hôte de console invite de commande |
| **SYSTEM** | `\Program Files\Windows Defender\MpCmdRun.exe` | SYSTEM | 2026-09-08 10:33:04 | Exécution commande Defender |
| **SYSTEM** | `\Windows\System32\reg.exe` | SYSTEM | 2026-09-07 15:45:03 | Manipulation Registre Windows |
| **SYSTEM** | `\Windows\System32\taskkill.exe` | SYSTEM | 2026-09-07 15:38:22 | Commande d'arrêt forcé de processus |

---

## 5. Livrables et Preuves
1. Capture PowerShell : clonage complet (100%) et commande `Get-FileHash`.
2. Image disque : `C:\Forensic\disque.raw`.
3. Projet Autopsy indexé : `Enquete_Malware_kok`.
4. Capture d'écran du tableau `Run Programs` d'Autopsy.
