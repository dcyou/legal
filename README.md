# dcyou — pages légales publiques

Site statique servi par GitHub Pages sur <https://dcyou.github.io/legal/>.
Une page par app : politique de confidentialité + conditions d'utilisation, en
français et en anglais. Le contenu est **généré** par Cockpit (panneau « Pages
légales ») à partir de `src/data/legal.js` de chaque app — la page publiée dit
donc exactement la même chose que l'écran légal dans l'app.

## Règle de publication : uniquement les apps sorties

Ce site est public et référencé depuis la page d'accueil dcyou. **Une app qui
n'est pas encore en store ne doit y apparaître d'aucune façon** — ni dans
l'index, ni comme dossier : un nom de dossier (`/legal/<app>/`) trahit un projet
autant qu'un lien, et le dépôt `dcyou/legal` est public (arborescence et
historique compris).

La page d'une app non sortie se génère **au moment de la soumission** au store,
pas avant. Rien n'est perdu : Cockpit la recrée à l'identique, même URL, en un
clic, à partir du texte légal versionné dans le dépôt de l'app.

### Piège : Cockpit republie tout ce qu'il trouve

`legal_generate` écrit une page pour **chaque** app découverte dans le
portfolio, puis réécrit `index.html` avec la liste complète. Générer puis
publier sans regarder **remet donc en ligne les apps non sorties**.

Avant de publier :

1. générer, puis vérifier `index.html` et la liste des dossiers ;
2. supprimer les apps non sorties (dossier **et** entrée d'index) ;
3. ou, définitivement, ajouter l'app à la liste `scan.skip` de Cockpit tant
   qu'elle n'est pas en store.

## `/legal/letterscatch/` — URL déclarée, à ne pas casser

Cette adresse est déclarée à Apple **et** à Google pour une app en cours de
validation. Elle doit répondre **200**, sans redirection, à tout moment : ne pas
la renommer, ne pas la déplacer, ne pas la supprimer. Toute intervention sur ce
dépôt se termine par une vérification de cette URL.

## Vérifier après un push (GitHub Pages met ~1 minute)

```bash
curl -s -o /dev/null -w '%{http_code}\n' https://dcyou.github.io/legal/letterscatch/   # attendu : 200
curl -s https://dcyou.github.io/legal/ | grep -o '<a href="\./[a-z]*/"'                # la liste publiée
```

## Contenu du dépôt

- `index.html` — l'index public (liste des apps sorties).
- `<slug>/index.html` — la page d'une app, autonome (aucun CSS/JS externe).
- `.nojekyll` — **indispensable** : sans lui, GitHub Pages passe le site dans
  Jekyll, qui ignore silencieusement certains chemins.
