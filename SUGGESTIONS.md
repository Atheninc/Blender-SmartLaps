# 💡 Suggestions d'amélioration — Smart Lapse Recorder

Ce document liste les pistes d'amélioration identifiées pour l'addon **Smart Lapse Recorder**. Chaque suggestion peut faire l'objet d'une issue GitHub pour le suivi et la discussion.

---

## 🗂️ Liste des suggestions

### 1. 🎞️ Compilation vidéo intégrée (FFmpeg)

Ajouter un bouton dans le panneau pour assembler automatiquement les frames d'un dossier rush en vidéo (MP4, GIF) via FFmpeg, sans quitter Blender.

---

### 2. 📊 Compteur de frames et durée estimée dans l'UI

Afficher en temps réel dans le panneau :
- Le nombre de frames capturées
- La durée estimée de la vidéo finale (selon un FPS cible configurable)
- L'espace disque consommé

---

### 3. ▶️ Reprise d'un enregistrement existant

Permettre de reprendre un enregistrement dans un dossier rush existant (après un crash ou un arrêt volontaire), au lieu de toujours créer un nouveau dossier.

---

### 4. 🖼️ Aperçu des dernières frames capturées

Afficher une miniature de la dernière frame capturée directement dans le panneau, pour vérifier visuellement que les captures se déroulent correctement.

---

### 5. ⏰ Démarrage programmé

Ajouter la possibilité de planifier le démarrage de l'enregistrement à une heure précise (ex : lancer une session de modélisation qui doit être capturée dès l'ouverture de Blender).

---

### 6. 📝 Convention de nommage personnalisée

Permettre à l'utilisateur de définir son propre préfixe/suffixe pour les noms des dossiers rush et des fichiers de frames.

---

### 7. 🔔 Alerte espace disque faible

Émettre un avertissement (notification Blender) lorsque l'espace disque disponible sur le dossier de sortie passe en dessous d'un seuil configurable.

---

### 8. 📐 Résolution de capture configurable

Permettre de définir une résolution personnalisée pour les captures (indépendante des paramètres de rendu de la scène), pour réduire la taille des fichiers lors d'enregistrements longs.

---

### 9. 🎥 Chemin de caméra animé

Prendre en charge les caméras animées (avec keyframes de position/rotation) pour produire des timelapses avec des mouvements de caméra fluides.

---

### 10. 🗑️ Suppression automatique des anciens dossiers rush

Ajouter une option pour supprimer automatiquement les anciens dossiers rush au-delà d'un nombre maximum configurable, afin de gérer l'espace disque automatiquement.

---

### 11. 📦 Export en GIF animé

Proposer un export direct en GIF animé depuis le panneau (en plus de la compilation MP4), utile pour partager rapidement des aperçus sur le web.

---

### 12. 🌐 Support multi-viewports

Capturer plusieurs viewports simultanément (ex : vue de face, côté, perspective) depuis un layout de fenêtres divisées, en produisant des images composites ou séparées.

---

### 13. 🛑 Arrêt automatique sur durée ou nombre de frames

Permettre de définir une durée maximale d'enregistrement ou un nombre maximum de frames, après lesquels l'enregistrement s'arrête automatiquement.

---

### 14. 🖱️ Icônes et amélioration visuelle du panneau

Améliorer l'interface utilisateur du panneau avec des icônes Blender sur les boutons, une mise en page plus claire et des sections pliables (collapsible sections).

---

### 15. 🔄 Compatibilité Blender 4.x et API Context Override

Blender 4.x a déprécié les context overrides en dictionnaire au profit de `bpy.context.temp_override()`. Mettre à jour `capture.py` pour assurer la compatibilité avec Blender 4.0+.
