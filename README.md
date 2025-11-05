# File_organizer_compact
Ce script a été développé dans le cadre d’une démarche d’optimisation des processus BIM et de gestion documentaire automatisée. Il vise à faciliter la manipulation, le classement et l’analyse des fichiers issus des projets de construction numériques, tout en garantissant la traçabilité et la cohérence des données.
# 📁 Organisateur de Fichiers Compact (Excel + IFC)

Script Python ultra-compact (< 200 lignes) pour organiser automatiquement vos fichiers selon des règles définies dans Excel, avec support avancé pour l'analyse de fichiers IFC.

## ✨ Fonctionnalités

- 📊 **Configuration Excel** : Définissez vos règles dans un fichier Excel simple
- 🔍 **Support wildcards** : Utilisez `*` pour créer des patterns flexibles
- 🏗️ **Analyse IFC** : Extraction automatique des données BIM (murs, dalles, poutres, etc.)
- 📈 **Export Excel** : Résultats d'analyse IFC exportés en Excel avec résumé
- 🔄 **Versioning automatique** : Évite les écrasements (`fichier_v2.ext`, `fichier_v3.ext`)
- 🚀 **Simple et rapide** : Seulement 179 lignes de code
- 🎯 **Organisation intelligente** : Trie par catégories et sous-dossiers

## 📋 Prérequis

### Dépendances obligatoires

```bash
pip install openpyxl
```

### Dépendances optionnelles (pour l'analyse IFC)

```bash
pip install ifcopenshell
```

> **Note** : Le script fonctionne sans `ifcopenshell`, mais l'analyse IFC sera désactivée.

## 🚀 Installation

1. **Téléchargez le script** :
   ```bash
   wget https://votre-url/file_organizer_compact.py
   # ou
   curl -O https://votre-url/file_organizer_compact.py
   ```

2. **Installez les dépendances** :
   ```bash
   pip install openpyxl
   pip install ifcopenshell  # Optionnel
   ```

3. **Rendez le script exécutable** (Linux/Mac) :
   ```bash
   chmod +x file_organizer_compact.py
   ```

## 📖 Utilisation

### Première exécution

Lancez simplement le script :

```bash
python3 file_organizer_compact.py
```

À la première exécution, deux fichiers seront automatiquement créés :
- **`config.ini`** : Configuration des chemins et paramètres
- **`file_mapping.xlsx`** : Règles de transfert avec exemples

### Configuration des dossiers

Le script crée automatiquement un fichier `config.ini` avec les paramètres par défaut :

```ini
[Paths]
# Dossier source où chercher les fichiers à organiser
source_folder = /home/user/Downloads

# Dossier de base pour la destination des fichiers organisés
destination_base = /home/user/Documents/Organised_Files

# Chemin vers le fichier Excel de configuration des règles
excel_config_file = file_mapping.xlsx

# Dossier où seront sauvegardés les fichiers d'analyse IFC
ifc_analysis_folder = /home/user/Documents/IFC_Analysis

[Settings]
# Activer l'analyse IFC (yes/no)
analyze_ifc = yes
```

**Pour personnaliser**, éditez simplement le fichier `config.ini` avec vos propres chemins.

## 📊 Configuration Excel

Le fichier `file_mapping.xlsx` définit les règles d'organisation.

### Structure du fichier Excel

Chaque **feuille** représente une **catégorie**, et contient deux colonnes :

| Nom du fichier | Sous-répertoire destination |
|----------------|----------------------------|
| Pattern        | Chemin relatif             |

### Exemple de configuration

#### Feuille "google"
| Nom du fichier | Sous-répertoire destination |
|----------------|----------------------------|
| google.design.*.aps | Design/Plans |
| google.*.pdf | Documents |
| google.meeting.*.docx | Meetings |

#### Feuille "ifc"
| Nom du fichier | Sous-répertoire destination |
|----------------|----------------------------|
| *.ifc | BIM/Models |
| building.*.ifc | BIM/Buildings |
| structure.*.ifc | BIM/Structures |

### Syntaxe des patterns

- **`*`** : Remplace n'importe quelle séquence de caractères
- **Exemples** :
  - `*.pdf` → Tous les fichiers PDF
  - `google.*.aps` → Tous les fichiers commençant par "google." et finissant par ".aps"
  - `rapport_2024_*.docx` → `rapport_2024_janvier.docx`, `rapport_2024_final.docx`, etc.

## 🏗️ Analyse IFC

Lorsqu'un fichier IFC est détecté, le script :

1. ✅ Extrait les éléments BIM (murs, dalles, poutres, colonnes, fenêtres, portes)
2. ✅ Récupère les propriétés (largeur, hauteur, longueur)
3. ✅ Identifie les GlobalId et noms
4. ✅ Exporte tout dans un fichier Excel avec :
   - **Feuille "IFC Analysis"** : Données détaillées de chaque élément
   - **Feuille "Résumé"** : Statistiques globales

### Exemple de sortie IFC

Fichier généré : `building_model_analysis_20250105_143022.xlsx`

#### Feuille "IFC Analysis"
| Type | GlobalId | Nom | Largeur | Hauteur | Longueur |
|------|----------|-----|---------|---------|----------|
| IfcWall | 2O2Fr... | Mur extérieur | 0.200 | 3.000 | 5.500 |
| IfcSlab | 3K5Gx... | Dalle RDC | 5.000 | 0.200 | 8.000 |

#### Feuille "Résumé"
- **Fichier analysé** : building_model.ifc
- **Date d'analyse** : 2025-01-05 14:30:22
- **Nombre total d'éléments** : 156

## 📝 Exemples d'utilisation

### Exemple 1 : Organisation de fichiers Google

**Fichiers dans Downloads** :
```
google.design.plan_v1.aps
google.architecture.schema.pdf
google.meeting.notes_jan.docx
rapport.pdf  # Pas de règle correspondante
```

**Configuration Excel (feuille "google")** :
| Nom du fichier | Sous-répertoire destination |
|----------------|----------------------------|
| google.design.*.aps | Design/Plans |
| google.*.pdf | Documents |
| google.meeting.*.docx | Meetings |

**Résultat** :
```
Documents/Organised_Files/
├── google/
│   ├── Design/Plans/
│   │   └── google.design.plan_v1.aps
│   ├── Documents/
│   │   └── google.architecture.schema.pdf
│   └── Meetings/
│       └── google.meeting.notes_jan.docx
```

Le fichier `rapport.pdf` reste dans Downloads (pas de règle).

### Exemple 2 : Organisation de fichiers IFC

**Fichiers dans Downloads** :
```
building_structure.ifc
facade_design.ifc
```

**Configuration Excel (feuille "ifc")** :
| Nom du fichier | Sous-répertoire destination |
|----------------|----------------------------|
| *.ifc | BIM/Models |

**Résultat** :
```
Documents/Organised_Files/
└── ifc/
    └── BIM/Models/
        ├── building_structure.ifc
        └── facade_design.ifc

Documents/IFC_Analysis/
├── building_structure_analysis_20250105_143022.xlsx
└── facade_design_analysis_20250105_143045.xlsx
```

### Exemple 3 : Versioning automatique

Si `fichier.pdf` existe déjà dans la destination :
- Nouvelle version → `fichier_v2.pdf`
- Encore une nouvelle → `fichier_v3.pdf`
- Et ainsi de suite...

## 🔧 Personnalisation avancée

### Modifier la configuration

Tous les paramètres sont dans le fichier `config.ini` :

```ini
[Paths]
source_folder = /mon/dossier/source
destination_base = /mon/dossier/destination
excel_config_file = mes_regles.xlsx
ifc_analysis_folder = /mon/dossier/analyses_ifc

[Settings]
analyze_ifc = yes  # ou no pour désactiver
```

### Utiliser un fichier de config personnalisé

Vous pouvez créer votre propre script avec un autre fichier de configuration :

```python
from file_organizer_compact import FileOrganizer

# Configuration personnalisée
organizer = FileOrganizer(config_file='ma_config.ini')
organizer.run()
```

### Désactiver l'analyse IFC

Dans le fichier `config.ini`, changez :

```ini
[Settings]
analyze_ifc = no
```

## 🐛 Dépannage

### Le script ne trouve pas mes fichiers

Vérifiez le fichier `config.ini` :
```ini
[Paths]
source_folder = /votre/dossier/correct
```

### Changer le dossier de destination

Éditez le fichier `config.ini` :
```ini
[Paths]
destination_base = /nouveau/dossier/destination
```

### Mes fichiers ne sont pas déplacés

1. **Vérifiez votre fichier Excel** : Les patterns doivent correspondre exactement
2. **Testez les patterns** : `google.*.pdf` ne match PAS `rapport_google.pdf`
3. **Vérifiez les logs** : Le script affiche "⚠ Pas de règle" si aucun pattern ne correspond

### L'analyse IFC ne fonctionne pas

```bash
# Installez ifcopenshell
pip install ifcopenshell

# Vérifiez l'installation
python3 -c "import ifcopenshell; print('OK')"

# Vérifiez la configuration
# Dans config.ini :
[Settings]
analyze_ifc = yes
```

### Erreur de permission

Sur Linux/Mac, assurez-vous d'avoir les droits :
```bash
chmod +x file_organizer_compact.py
```

## 📊 Sortie du script

### Exemple de sortie normale

```
╔============================================================╗
║  ORGANISATEUR DE FICHIERS COMPACT (Excel + IFC)         ║
╚============================================================╝

📂 Source: /home/user/Downloads
📁 Destination: /home/user/Documents/Organised_Files
📊 Excel: file_mapping.xlsx
🏗️  Analyse IFC: Activée → /home/user/Documents/IFC_Analysis

✓ Config chargée: 2 catégorie(s)
📂 5 fichier(s) trouvé(s)

📄 google.design.plan.aps
  ✓ → google.design.plan.aps
📄 building.ifc
  📊 Analyse IFC...
  ✓ Analyse exportée: 156 éléments
  ✓ → building.ifc
📄 rapport.pdf
  ⚠ Pas de règle

✓ Terminé: 2/5 fichiers traités
```

## 🔄 Automatisation

### Linux/Mac - Cron

Exécutez le script toutes les heures :

```bash
# Éditez crontab
crontab -e

# Ajoutez cette ligne
0 * * * * /usr/bin/python3 /chemin/vers/file_organizer_compact.py
```

### Windows - Planificateur de tâches

1. Ouvrez le **Planificateur de tâches**
2. Créez une nouvelle tâche
3. Déclencheur : Quotidien ou à l'ouverture de session
4. Action : `python.exe C:\chemin\vers\file_organizer_compact.py`

## 📄 Licence

Script libre d'utilisation et de modification.

## 🤝 Contribution

N'hésitez pas à adapter ce script à vos besoins !

## 📞 Support

Pour toute question ou problème :
1. Vérifiez la section **Dépannage**
2. Consultez les **Exemples d'utilisation**
3. Testez avec des fichiers simples d'abord

## 🎯 Résumé rapide

```bash
# 1. Installer
pip install openpyxl ifcopenshell

# 2. Lancer (crée config.ini et file_mapping.xlsx)
python3 file_organizer_compact.py

# 3. Personnaliser config.ini
# Modifiez les chemins source et destination

# 4. Personnaliser file_mapping.xlsx
# Ajoutez vos règles dans Excel

# 5. Relancer
python3 file_organizer_compact.py

# 6. Profiter ! 🎉
```

---

**Version** : 1.0  
**Lignes de code** : 200  
**Compatibilité** : Python 3.6+  
**Testé sur** : Linux, macOS, Windows
