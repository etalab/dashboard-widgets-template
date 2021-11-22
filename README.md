# Créer des widgets avec VueJS

## Résumé

Ce répertoire contient un template pour construire et publier des widgets web avec VueJS.

C'est le coeur de la solution utilisée par Etalab pour réaliser le [dashboard de suivi de l'épidémie de Covid19](https://github.com/etalab/covid19-dashboard-widgets) publié sur [gouvernement.fr](https://www.gouvernement.fr/info-coronavirus/carte-et-donnees).

Le principe est de générer un fichier js et un fichier css permettant à n'importe quel site distant d'utiliser des éléments html customisés dans ses pages.

## Propriétés

- Orienté datavisualisation
- Construire des widgets html hyper customisés avec VueJS
- Store de données et de paramètres mutualisé entre les widgets
- Styles et polices du [système de design de l'Etat](https://www.systeme-de-design.gouv.fr/) pré-intégrés
- Widgets présents dans le DOM = styles surchargeables  par la page parent

## Comment ça marche ?

Le concept de ce projet est assez proche de celui des [web components](https://developer.mozilla.org/fr/docs/Web/Web_Components) mais utilise les facilités offertes par VueJS pour créer le code hyper-customisés des widgets.

Il repose principalement sur l'utilisation du package [vue-custom-elements](https://github.com/karol-f/vue-custom-element) pour transformer les composants Vue classiques en éléments html pouvant être directement intégrés dans le DOM du site client.

L'ensemble des composants sont compilés lors du _build_ en deux fichiers js et css qu'il suffit d'ajouter dans le head de n'importe quelle page web pour pouvoir utiliser les widgets, même si la page parente ne contient pas Vue.

## Comment créer mes propres widgets ?

### Créer un nouveau widget
- ajouter un nouveau fichier Vue dans /src/components
- un composant générique MyComponent.vue existe dans le répertoire pour servir de modèle
- importer ce nouveau composant dans /src/main.js
	```js
	import MyComponent from './components/MyComponent'
	```	
- instancier le nouveau composant dans /src/main.js
	```js
	Vue.customElement('my-component', MyComponent)
	```

### Mutualiser des imports de données
- des données stockées dans l'object state du fichier /src/store/index.js sont accessibles à tous les widgets
- des données peuvent stockées dans le store depuis les composants ou des fichiers indépendants comme /src/import.js
- dans ce cas, la fonction d'import de données doit être déclarée dans /src/main.js
	```js
	import { getData } from './import.js'
	```	

### Mutualiser des fonctions
- décrire les fonctions utilitaires dans un fichier indépendant comme /src/utils.js
- exporter ces fonctions sous forme de mixins
	```js
	export const mixin = {
	  methods: {
	    myCustomFunction,
	  }
	}
	```	
- importer les mixins dans les composants qui en ont l'utilité
	```js
	import { mixin } from '@/utils.js'
	```	


## Mise en place du projet
```
npm install
```

### Compilation et rechargement à la volée pour le dévelopement
```
npm run serve
```

### Compilation et miniaturisation du code pour la production
```
npm run build
```

### Lints et fixes des fichiers
```
npm run lint
```

### Notes

Webpack doit être installé comme suit :  
```
npm install webpack@^4.0.0 --save-dev
```
