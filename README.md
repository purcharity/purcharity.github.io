# PUR CHARITY — 4ème édition

Site vitrine de l'événement caritatif Twitch au profit de l'association
[Les P'tits Doudous](https://lesptitsdoudous.org/).

Site **100 % statique** : pas de serveur, pas de build, pas de `npm install`.
C'est du HTML/CSS/JS dans un seul fichier.

```
purcharity/
├── index.html              ← tout est là (page + styles + liste des streamers)
├── logo-purcharity.png
├── logo-ptits-doudous.png
├── titre-4eme-edition.png
├── fond.jpg
└── README.md
```

⚠️ Les 4 images doivent rester **à côté** de `index.html`, dans le même dossier.
Sinon : page noire sans logos.

---

## Lancer le site en local

Le plus simple : **double-clic sur `index.html`**. Ça suffit pour tout voir.

Si tu préfères un vrai serveur local (rendu identique à la mise en ligne) :

```bash
python -m http.server 8000
```

Puis ouvre http://localhost:8000 dans ton navigateur.
Pour arrêter le serveur : `Ctrl + C` dans le terminal.

---

## Ajouter un streamer

Ouvre `index.html` dans un éditeur de texte (Notepad++, VS Code, ou même le Bloc-notes).
Cherche le gros bloc `const SECTIONS` encadré de `▼▼▼` — c'est le **seul endroit à modifier**.

Ajoute une ligne dans la bonne section :

```js
{ n:"pseudo_twitch" },
```

Le pseudo doit être **exactement** celui de l'URL `twitch.tv/pseudo`.
Le lien vers la chaîne et la photo de profil se font tout seuls.

### Options

```js
{ n:"pseudo_twitch", label:"Nom Affiché", img:"https://une-image.png" },
```

| Clé | Obligatoire | À quoi ça sert |
|---|---|---|
| `n` | oui | le pseudo Twitch, tel quel dans l'URL |
| `label` | non | afficher un nom différent du pseudo |
| `img` | non | forcer une photo précise (voir plus bas) |

### Retirer / réordonner

- **Retirer** quelqu'un : supprime sa ligne entière (virgule comprise).
- **Changer l'ordre** : déplace la ligne. L'affichage suit l'ordre du tableau.
- Le compteur « X chaînes » sous chaque titre **se recalcule tout seul**, rien à toucher.

> Attention à la virgule : chaque ligne se termine par `,` **sauf la dernière** de la
> section. Si la page s'affiche vide, c'est presque toujours une virgule en trop ou
> un guillemet manquant.

---

## Ajouter une section entière

Copie un bloc complet dans `SECTIONS` :

```js
{ titre:"Invités surprise", liste:[
    { n:"pseudo1" },
    { n:"pseudo2" }
]},
```

Ajoute `classe:"featured"` pour avoir des grosses cartes, comme la mise en avant :

```js
{ titre:"Invités surprise", classe:"featured", liste:[
    { n:"pseudo1" }
]},
```

Les sections s'affichent dans l'ordre du tableau. Le titre et le compteur sont
générés automatiquement.

---

## Comment marchent les photos de profil

Aucune clé API Twitch n'est nécessaire. Pour chaque streamer, le site essaie
**quatre choses dans l'ordre**, et s'arrête à la première qui marche :

1. **`unavatar.io`** — service public qui renvoie l'avatar Twitch ;
2. **la clé `img`** de la fiche, si tu en as mis une ;
3. **`decapi.me`** — renvoie en texte brut l'URL réelle du CDN Twitch ;
4. **l'initiale du pseudo** en gros dans le rond violet.

Comme ça la page reste propre même si un service tombe : au pire on voit une lettre,
jamais une image cassée.

En pratique, une partie des avatars passe par l'étape 3 (`unavatar` ne connaît pas
tout le monde) — c'est normal et invisible pour le visiteur.

**Un avatar affiche une lettre au lieu de la photo ?** Vérifie d'abord l'orthographe
du pseudo : c'est la cause dans 9 cas sur 10. Sinon, tu peux forcer l'image à la main :
va sur la chaîne Twitch, clic droit sur la photo de profil → « Copier l'adresse de
l'image », et colle-la dans `img:"..."`.

---

## Mise en ligne

Le site est déposable tel quel sur n'importe quel hébergeur statique
(GitHub Pages, Netlify, Cloudflare Pages, un simple FTP) : tous les chemins sont
relatifs, tout est à plat dans le dossier.

Il suffit d'envoyer le contenu du dossier `purcharity/` à la racine de l'hébergement.

> Petite note pour plus tard : les balises Open Graph (`og:image`) utilisent un chemin
> relatif. Pour que la miniature s'affiche correctement quand on partage le lien sur
> Discord ou Twitter, il faudra la remplacer par l'URL complète une fois le domaine
> connu (ex. `https://mon-site.fr/logo-purcharity.png`).

---

## Détails techniques

- Aucune dépendance, aucun framework, aucun `localStorage`.
- Polices via Google Fonts (Passion One + Denk One) — nécessite une connexion ;
  sans internet, le navigateur retombe sur une police système, la mise en page tient.
- Responsive jusqu'à 390 px de large (3 cartes par ligne sur mobile).
- Les animations sont désactivées si le visiteur a activé
  « réduire les animations » dans son système (`prefers-reduced-motion`).
- Navigation au clavier possible : les cartes et le bloc de l'association ont un
  contour de focus visible.
