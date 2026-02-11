Description du concept – Application “SmartCook AI”
L’objectif est de développer une application mobile intelligente permettant aux utilisateurs de découvrir des recettes qu’ils peuvent préparer à partir des ingrédients qu’ils possèdent réellement.

L’application utilise l’IA pour analyser soit une photo du réfrigérateur, soit une liste écrite d’ingrédients fournie par l’utilisateur.
L’expérience doit être simple, fluide et entièrement assistée par IA.
🎯 Fonctionnalités principales
1. Analyse d’une photo du réfrigérateur
L'utilisateur peut ouvrir l'application, prendre une photo de son réfrigérateur (ou de ses ingrédients), puis l'application envoie cette photo à un modèle d’IA.
L’IA :
détecte automatiquement les ingrédients visibles dans l'image,
renvoie une liste d’ingrédients identifiés.
L’utilisateur peut alors :
valider la liste,
modifier ou compléter les ingrédients,
retirer les éléments mal reconnus.
Une fois validée, la liste est envoyée à l’IA pour générer :
une ou plusieurs recettes possibles,
adaptées aux ingrédients réellement disponibles.
2. Saisie manuelle des ingrédients
L’utilisateur peut choisir de ne pas utiliser la photo et :
créer manuellement une liste d’ingrédients,
l’envoyer directement à l’IA.
L’IA renvoie immédiatement :
les recettes réalisables avec cette liste,
éventuellement des conseils supplémentaires (suggestions d’ingrédients manquants, variantes, etc.).
3. Génération de recettes personnalisées par l’IA
Une fois les ingrédients validés (via photo ou texte), l’IA génère :
des recettes détaillées,
le nombre d’étapes,
les quantités nécessaires,
le temps de préparation,
des variantes possibles,
des conseils pour optimiser l’utilisation des ingrédients.
🧩 Flux utilisateur (User Flow)
L’utilisateur ouvre l’application et choisit :
Prendre une photo, ou
Entrer manuellement les ingrédients.
Cas Photo :
L’utilisateur prend une photo -> L'application l’envoie à l’IA.
L’IA retourne une liste d’ingrédients détectés.
L’utilisateur valide/modifie cette liste.
L’application envoie la liste validée à l’IA.
L’IA renvoie les recettes possibles.
Cas Liste manuelle :
L’utilisateur saisit les ingrédients.
Il confirme la liste.
L’application envoie la liste à l’IA.
L’IA renvoie les recettes.
L’utilisateur consulte les recettes proposées.
✔️ Critères d’acceptation
Détection & Validation des ingrédients
L’application doit permettre de prendre une photo directement depuis l'appareil photo du smartphone.
L’IA doit identifier les ingrédients présents sur la photo et renvoyer une liste exploitable.
L’utilisateur doit pouvoir :
valider la liste,
modifier les ingrédients,
en ajouter ou en supprimer.
Génération des recettes
Lorsque la liste est validée, l’IA doit proposer au moins une recette.
Chaque recette doit contenir :
un titre,
la liste des ingrédients utilisés,
les quantités estimées,
le temps de préparation et de cuisson,
les étapes détaillées de la recette.
Saisie manuelle
L’utilisateur doit pouvoir ajouter une liste de plusieurs ingrédients via du texte.
L'application doit envoyer cette liste directement à l’IA et afficher les recettes sans passer par l'étape photo.
Expérience utilisateur
Le processus complet (photo → validation → recettes) doit être fluide et sans étapes inutiles.
Les réponses de l’IA doivent être affichées dans un format clair et lisible.
🚀 Objectif global du projet
Créer une application simple et intuitive qui :
aide l’utilisateur à réduire le gaspillage alimentaire,
lui fait gagner du temps,
lui propose des recettes adaptées à ce qu’il possède réellement,
combine vision par ordinateur + intelligence conversationnelle.
 