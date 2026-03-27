# Cimetières Célèbres — Paris
> Application iOS Capacitor · Père-Lachaise & Montparnasse

---

## Ajouts 1.1
- Ajout Lionel Jospin
- Ajout Nos autres App
- Suppression du double clic

## Contenu de l'appli

**48 tombes célèbres** avec pour chacune :
- Photo (Wikipedia Commons)
- Dates de naissance/mort
- Courte biographie
- Coordonnées GPS précises
- Division / section dans le cimetière
- Bouton "Ouvrir dans Plans" (Apple Maps)

### Père-Lachaise (12)
Jim Morrison · Édith Piaf · Oscar Wilde · Frédéric Chopin · Marcel Proust ·
Molière · Honoré de Balzac · Sarah Bernhardt · Yves Montand · Simone Signoret ·
Guillaume Apollinaire · Eugène Delacroix

### Montparnasse (36)
Jean-Paul Sartre · Simone de Beauvoir · Serge Gainsbourg · Samuel Beckett ·
Charles Baudelaire · Guy de Maupassant · Man Ray · Marguerite Duras ·
Henri Poincaré · Susan Sontag

---

## Vues de l'appli

| Vue | Description |
|-----|-------------|
| **Liste** | Cartes scrollables, filtre par cimetière, barre de recherche |
| **Carte** | Plan Leaflet dark (Carto), marqueurs colorés par cimetière |
| **Favoris** | Personnages mis en favori (persistés localStorage) |
| **Fiche détail** | Sheet slide-up avec photo, biographie, GPS, Plans |

---

## Installation Capacitor

### Prérequis
```bash
# Vérifier que Capacitor CLI est disponible
npx cap --version   # >= 6.x
```

### Setup du projet
```bash
# 1. Installer les dépendances npm
npm install

# 2. Ajouter la plateforme iOS
npx cap add ios

# 3. Synchroniser les assets web
npx cap sync ios

# 4. Ouvrir dans Xcode
npx cap open ios
```

### Dans Xcode

1. **Bundle ID** → `com.migapple.cimetieres` (ou ton propre ID)
2. **Signing** → ton Apple Developer account
3. **Deployment target** → iOS 14.0+
4. **Info.plist** → coller le contenu de `ios-info-plist-additions.xml`
5. Connecter l'iPhone → Build & Run ▶

---

## Notes techniques

### Safe areas (notch/Dynamic Island)
Le CSS utilise `env(safe-area-inset-top)` et `env(safe-area-inset-bottom)`.
`viewport-fit=cover` est déjà dans le `<meta viewport>`.

### Réseau
- **Leaflet** + tuiles Carto : chargées depuis CDN (HTTPS) ✓
- **Photos** : Wikipedia Commons HTTPS ✓
- **Google Fonts** : Cormorant Garamond + Jost ✓
- `CapacitorHttp: enabled` dans `capacitor.config.json` pour les requêtes XHR

### Prévenir le zoom iOS sur focus input
`font-size: 16px` sur `.search-input` (déjà en place)

### Persistence des favoris
`localStorage` — persisté entre les sessions

### Carte offline
Les tuiles Carto nécessitent une connexion. Pour une carte offline,
envisage [leaflet-offline](https://github.com/allartk/leaflet.offline).

---

## Ajouter des personnages

Dans `www/index.html`, ajouter un objet au tableau `GRAVES` :

```js
{
  id: 23,                          // ID unique
  cemetery: 'pere-lachaise',       // 'pere-lachaise' | 'montparnasse'
  name: 'Prénom Nom',
  born: 1900, died: 1980,
  profession: 'Métier',
  section: 'Division XX',
  lat: 48.86100,                   // coordonnées GPS précises
  lng: 2.39500,
  desc: 'Biographie courte...',
  photo: 'https://...',            // URL image (HTTPS)
  init: 'PN'                       // initiales (fallback si photo absente)
}
```

---

## App Store

- **Catégorie suggérée** : Travel ou Education
- **Permissions** : aucune (pas de caméra, pas de micro, pas de contacts)
- **Âge minimum** : 4+
- **Localisation** : français (description + UI)

---

*Projet généré avec Capacitor 6 · HTML/CSS/JS vanilla · Leaflet 1.9.4*
