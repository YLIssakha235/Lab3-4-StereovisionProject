# Stereo Vision - 3D Reconstruction by Laser Scanning

## 1. Objectif du projet

Ce projet a pour objectif de **reconstruire un objet en 3D** à partir de :
- deux caméras fixes (stéréo vision),
- un plan laser balayant l’objet,
- une séquence d’images synchronisées (Left / Right).

La reconstruction est réalisée par :
- calibration stéréo à l’aide d’un damier,
- calcul de la géométrie épipolaire,
- extraction de la ligne laser,
- triangulation 3D point par point.

---

## 2. Données fournies

- `data/chessboards/`  
  Images du **damier** dans différentes positions (calibration)

- `data/scanLeft/`  
  Images de l’objet avec **laser – caméra gauche**

- `data/scanRight/`  
  Images de l’objet avec **laser – caméra droite**

---

## 3. Installation

### 3.1 Créer un environnement virtuel

```bash
python -m venv .venv
```

### 3.2 Activer l’environnement

Windows

```bash
.venv\Scripts\activate
```

Linux / macOS
```bash
source .venv/bin/activate
```

### Installer les dépendances
```bash
pip install opencv-python numpy matplotlib jupyter
```

---

## 4. Organisation du projet
Le projet est structuré en blocs successifs, chacun correspondant à une étape théorique du cours.
---

## 5. Description des blocs

### BLOC 1: Calibration mono (caméras indépendantes)
- Détection des coins du damier
- Raffinement sub-pixel
- Calcul des paramètres intrinsèques :
    - matrice K
    - coefficients de distorsion
- Estimation des poses (rvec, tvec)

chaque caméra est calibrée individuellement.

---

### BLOC 2: Matrice fondamentale F
- Utilisation des correspondances du damier
- Calcul de F par :
    - méthode 8-point
    - méthode RANSAC (retenue)
- Vérification de la contrainte :
x′TFx≈0

La géométrie épipolaire entre les deux caméras est validée.

---

### BLOC 3: Rectification stéréo
- Calcul de la pose relative entre caméras
- Utilisation de cv2.stereoRectify
- Images rectifiées avec :
    - épilignes horizontales
    - correspondances facilitées
Après rectification, un point gauche correspond à un point droit sur la même ligne horizontale.

--- 

### BLOC 4: Matrices de projection P1, P2
- Construction des matrices :
    P1​=KL​[I∣0],P2​=KR​[R∣t]
- La caméra gauche est prise comme référence.

Ces matrices sont utilisées pour la triangulation.

---


### BLOC 5: Triangulation du damier (validation)
- Triangulation des coins du damier
- Vérification de la planéité du nuage 3D

Le damier reconstruit est quasi-plan → validation de la géométrie.

---

### BLOC 6: Reconstruction 3D par laser
C’est le cœur du projet.

Étapes réalisées pour chaque scan :
- Rectification des images gauche et droite
- Extraction de la ligne laser (canal rouge)
- Échantillonnage : 1 point par ligne horizontale
- Appariement gauche/droite par coordonnée y
- Triangulation 3D
- Accumulation des points sur tous les scan


---

## 6. Résultat final
- Reconstruction 3D cohérente
- Nuage de points aligné et stable
- Forme globale de l’objet clairement identifiable

---

## 7. Conclusion

Ce projet démontre une implémentation complète et cohérente
d’un système de reconstruction 3D par stéréo vision et laser,
en accord avec les concepts théoriques du cours.

L’approche est robuste, progressive et validée à chaque étape.

---

## 8. Auteur
Projet réalisé par : YL Issakha (21252)
Cours: Image Processing