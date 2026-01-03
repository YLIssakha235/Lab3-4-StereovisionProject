# Stereo Vision - 3D Reconstruction by Laser Scanning

## Objectif du projet

Ce projet a pour objectif de **reconstruire un objet en 3D** à partir de deux caméras fixes (stéréo vision), d'un plan laser balayant l'objet, et d'une séquence d'images synchronisées (Left / Right).

La reconstruction est réalisée par calibration stéréo, calcul de la géométrie épipolaire, extraction de la ligne laser, et triangulation 3D point par point.

---

## Données fournies

- **`data/chessboards/`** - Images du damier dans différentes positions (calibration)
- **`data/scanLeft/`** - Images de l'objet avec laser (caméra gauche)
- **`data/scanRight/`** - Images de l'objet avec laser (caméra droite)

---

## Installation

### Créer un environnement virtuel

```bash
python -m venv .venv
```

### Activer l'environnement

**Windows**
```bash
.venv\Scripts\activate
```

**Linux / macOS**
```bash
source .venv/bin/activate
```

### Installer les dépendances

```bash
pip install opencv-python numpy matplotlib jupyter
```

---

## Pipeline de reconstruction

Le projet suit une série d'étapes successives :

### 1. Détection des coins du damier
- Détection des coins internes du damier
- Raffinement sub-pixel
- Visualisation pour validation

### 2. Calibration mono
- Calibration indépendante de chaque caméra
- Estimation de la matrice intrinsèque K et des coefficients de distorsion
- Analyse de l'erreur RMS de reprojection

### 3. Géométrie épipolaire
- Estimation robuste de la matrice fondamentale F par RANSAC
- Validation par erreur épipolaire point–ligne
- Visualisation des lignes épipolaires

### 4. Rectification épipolaire
- Calcul des homographies H₁, H₂
- Rectification des images de référence et de scan
- Validation par alignement horizontal

### 5. Extraction et matching du laser
- Segmentation du laser rouge en espace HSV
- Extraction des pixels laser avec nettoyage du masque
- Appariement gauche/droite par coordonnée y
- Calcul de la disparité

### 6. Reconstruction 3D
- Calibration stéréo avec intrinsèques fixées
- Estimation de la pose relative R, T
- Triangulation 3D du laser
- Filtrage des points non physiques

---

## Résultat final

- Reconstruction 3D cohérente avec nuage de points stable et structuré
- Forme globale de l'objet clairement identifiable
- Géométrie valide à échelle relative

---

## Auteur

**YL Issakha (21252)**  
Cours: Image Processing