# 🎬 Smart Lapse Recorder

**Smart Lapse Recorder** est un addon Blender conçu pour simplifier la production de timelapses stylisés directement depuis le viewport de Blender. Il capture automatiquement des images à intervalles réguliers, avec la possibilité de choisir le mode d'affichage, la caméra, et de filtrer les captures selon les modifications apportées à la scène.

> Compatible avec **Blender 3.3 et supérieur** — Version actuelle : **0.4.0**

---

## 📋 Table des matières

- [Fonctionnalités](#fonctionnalités)
- [Installation](#installation)
- [Utilisation](#utilisation)
- [Cas d'usage](#cas-dusage)
- [Détails techniques](#détails-techniques)
- [Structure du projet](#structure-du-projet)

---

## ✨ Fonctionnalités

- ⏱️ Capture automatique à intervalles de temps configurables
- 🖼️ Capture multi-modes simultanée (Wireframe, Solid, Material Preview, Rendered)
- 📷 Sélection d'une caméra spécifique pour le point de vue
- 🔄 Mode "Only Edit Frames" : ne capture que si la scène a été modifiée
- 🎛️ Contrôle des overlays (annotations, grille, etc.)
- 📁 Organisation automatique des frames dans des dossiers horodatés (*rush*)
- ▶️ Contrôles Start / Pause / Stop intuitifs depuis le panneau 3D View

---

## 🛠️ Installation

1. **Télécharger** le fichier `v2 smart_lapse_recorder (3.3+).zip` directement depuis ce dépôt (section **Code**, ou depuis la [page de releases](https://github.com/Atheninc/Blender-SmartLaps/releases) si disponible).

2. **Ouvrir Blender** (3.3 ou supérieur).

3. Aller dans **Edit > Preferences > Add-ons**.

4. Cliquer sur **Install...** en haut à droite.

5. **Sélectionner** le fichier `.zip` téléchargé.

6. **Activer** l'addon en cochant la case à côté de *"Smart Lapse Recorder"*.

7. Le panneau apparaît dans la **3D View**, onglet latéral **"Timelapse Recorder"** (touche `N` pour ouvrir le panneau latéral).

---

## 🎮 Utilisation

### Accéder au panneau

Ouvrez la **3D View** → appuyez sur `N` → onglet **"Timelapse Recorder"**.

### Paramètres disponibles

| Paramètre | Description | Valeur par défaut |
|---|---|---|
| **Output Folder** | Dossier de sortie pour les frames capturées | `//timelapse_frames` |
| **Interval (s)** | Durée en secondes entre chaque capture | `1.0 s` |
| **Camera** | Caméra à utiliser pour le point de vue | *(première caméra disponible)* |
| **Wireframe** | Activer la capture en mode Wireframe | ☐ |
| **Solid** | Activer la capture en mode Solid | ☐ |
| **Material** | Activer la capture en mode Material Preview | ☐ |
| **Render** | Activer la capture en mode Rendered | ☐ |
| **Display Overlay** | Afficher les overlays dans les captures | ☑ |
| **Only Capture Edit Frames** | Ne capturer que si la scène a été modifiée | ☐ |

### Contrôles

- **Start Recording** — Démarre l'enregistrement et crée un dossier `rush_YYYYMMDD_HHMMSS/`
- **Pause Recording** — Met en pause l'enregistrement (reprend depuis le même dossier rush)
- **Stop Recording** — Arrête et finalise l'enregistrement

### Workflow typique

```
1. Définir le dossier de sortie (ex : //timelapse_frames)
2. Régler l'intervalle (ex : 5s pour une session longue)
3. Choisir un ou plusieurs modes d'affichage
4. Sélectionner une caméra (facultatif)
5. Cliquer sur "Start Recording"
6. Travailler normalement sur votre scène
7. Cliquer sur "Stop Recording" une fois terminé
8. Assembler les frames en vidéo avec un logiciel externe (ex : FFmpeg, DaVinci Resolve)
```

---

## 📐 Cas d'usage

### 1. 🏗️ Timelapse de modélisation

Capturez l'évolution d'un mesh au fil du temps en mode **Solid** ou **Wireframe**. Idéal pour montrer le processus de création d'un personnage ou d'un environnement.

- Intervalle recommandé : **2–5 secondes**
- Modes conseillés : **Wireframe** + **Solid**
- Option **Only Capture Edit Frames** : **activée** (évite les frames vides)

### 2. 🎨 Timelapse de texturing / shading

Montrez l'application progressive des matériaux et textures sur votre modèle.

- Intervalle recommandé : **3–10 secondes**
- Modes conseillés : **Material Preview** + **Rendered**
- Overlays : **désactivés** pour un rendu plus propre

### 3. 💡 Timelapse de lighting

Documentez la mise en place de votre éclairage, de l'obscurité à la lumière finale.

- Intervalle recommandé : **5–15 secondes**
- Modes conseillés : **Rendered**
- Sélectionner une caméra fixe pour un cadrage cohérent

### 4. 🏙️ Timelapse de scène complexe

Pour de grandes scènes (architecture, environnements), visualisez la progression globale.

- Intervalle recommandé : **10–30 secondes**
- Modes conseillés : **Solid** ou **Rendered**
- Caméra externe conseillée pour un point de vue fixe et stable

### 5. 📚 Tutoriel / démonstration

Créez du contenu pédagogique montrant chaque étape de votre processus.

- Intervalle recommandé : **1–2 secondes**
- Option **Only Capture Edit Frames** : **activée**
- Overlays : **activés** pour afficher les annotations et la grille

---

## ⚙️ Détails techniques

### Architecture

L'addon est structuré en modules Python :

```
smart_lapse_recorder/
├── __init__.py            # Point d'entrée, bl_info, register/unregister
├── properties.py          # Définition des propriétés de scène (bpy.props)
├── operators/
│   ├── __init__.py        # Enregistrement des opérateurs
│   ├── start_timelapse.py # Opérateur modal de démarrage
│   ├── pause_timelapse.py # Opérateur de pause
│   └── stop_timelapse.py  # Opérateur d'arrêt
├── panels/
│   ├── __init__.py        # Enregistrement des panneaux UI
│   └── timelapse_panel.py # Panneau 3D View
└── utils/
    ├── __init__.py
    └── capture.py         # Logique de capture OpenGL
```

### Opérateur modal (`StartTimelapseRecording`)

L'opérateur principal est un **opérateur modal** Blender (`RUNNING_MODAL`) qui :

1. Crée un dossier de rush horodaté : `rush_YYYYMMDD_HHMMSS/`
2. Enregistre un timer via `wm.event_timer_add(time_step=interval)`
3. À chaque tick du timer, appelle `offscreen_capture()` pour chaque mode d'affichage activé
4. Incrémente `context.scene.frame_current` à chaque capture

### Détection des modifications (`Only Capture Edit Frames`)

Un handler `depsgraph_update_post` est enregistré globalement pour détecter toute modification du graphe de dépendances de la scène. La variable globale `scene_updated` est passée à `True` lors d'un changement, et réinitialisée après chaque capture.

### Capture viewport (`offscreen_capture`)

La capture utilise `bpy.ops.render.opengl()` avec un contexte surchargé (`override`) pointant vers une aire `VIEW_3D` existante. Le processus :

1. Trouve une aire `VIEW_3D` dans les écrans Blender actifs
2. Sauvegarde l'état du viewport (mode d'ombrage, overlays, perspective, caméra)
3. Applique les paramètres demandés (mode, overlay, caméra)
4. Déclenche `bpy.ops.render.opengl(write_still=True, view_context=True)`
5. Restaure l'état initial du viewport

### Nommage des fichiers

Les frames sont sauvegardées au format PNG avec la convention :

```
{rush_folder}/timelapse_frame_{NNNN}_{mode}.png
```

Exemples :
- `timelapse_frames/rush_20240101_120000/timelapse_frame_0001_wireframe.png`
- `timelapse_frames/rush_20240101_120000/timelapse_frame_0001_solid.png`

### Propriétés de scène enregistrées

| Propriété | Type | Description |
|---|---|---|
| `timelapse_interval` | `FloatProperty` | Intervalle entre captures (min: 0.1s) |
| `timelapse_output_path` | `StringProperty` | Chemin du dossier de sortie |
| `timelapse_current_rush_path` | `StringProperty` | Chemin du rush en cours |
| `timelapse_camera` | `EnumProperty` | Caméra sélectionnée |
| `timelapse_display_wireframe` | `BoolProperty` | Capture mode Wireframe |
| `timelapse_display_solid` | `BoolProperty` | Capture mode Solid |
| `timelapse_display_material` | `BoolProperty` | Capture mode Material |
| `timelapse_display_render` | `BoolProperty` | Capture mode Rendered |
| `timelapse_display_overlay` | `BoolProperty` | Affichage des overlays |
| `timelapse_only_capture_edit_frames` | `BoolProperty` | Filtre sur modification |

---

## 📁 Structure du projet

```
Blender-SmartLaps/
└── v2 smart_lapse_recorder (3.3+).zip   # Archive de l'addon à installer dans Blender
```

---

## 📄 Licence

Ce projet est développé par **Adrien de Tena**. Consultez les conditions d'utilisation directement dans le dépôt.
