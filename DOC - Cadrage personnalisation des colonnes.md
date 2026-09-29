# DOC — Mémorisation de la largeur et de l’ordre des colonnes

## Contextes

Dans Doc, les documentalistes utilisent différents tableaux pour rechercher et consulter les données produit, notamment les références équipementiers (IAM) et constructeur (OE).

Il est actuellement possible de modifier la largeur et l’ordre des colonnes pour adapter leur présentation. Cependant, ces modifications ne sont pas conservées à la réouverture de l’application.

Chaque utilisateur dispose d’un compte personnel et peut retrouver son environnement de travail lorsqu’il se connecte depuis un autre ordinateur de l’entreprise.

## Problématiques

Les utilisateurs doivent régulièrement refaire les mêmes ajustements pour retrouver une présentation adaptée à leur travail.

Nous souhaiterions que ces réglages soient conservés individuellement et retrouvés lors des prochaines utilisations de Doc, y compris depuis un autre ordinateur.

Certaines vues, comme les recherches standard et avancée d’un même type, peuvent coexister et doivent avoir la même présentation.

Enfin, plusieurs fenêtres peuvent s’empiler dans Doc. Il faudra veiller à ce que la fermeture d’une fenêtre conservant une ancienne présentation n’écrase pas les modifications effectuées dans une autre fenêtre.

## Besoins

Nous souhaitons permettre à chaque utilisateur de conserver automatiquement :

- la largeur des colonnes ;
- l’ordre des colonnes.

Ce fonctionnement doit être disponible dans tous les tableaux de Doc, y compris ceux présents dans les onglets, les fenêtres secondaires et les fenêtres superposées.

Les réglages doivent être rattachés au compte utilisateur et retrouvés après fermeture de Doc ou connexion sur un autre poste.

Nous souhaitons également disposer d’une action permettant de rétablir la présentation par défaut du tableau concerné.

Cette évolution ne concerne pas la mémorisation des tris, des filtres ou de la visibilité des colonnes. Elle ne concerne pas non plus Ref.

## Spécifications détaillées

### 1. Personnalisation et enregistrement

Lorsqu’un utilisateur termine le déplacement ou le redimensionnement d’une colonne, la nouvelle présentation doit être enregistrée automatiquement, sans bouton de validation.

Une modification de largeur ne doit pas modifier l’ordre des colonnes. De la même manière, un déplacement ne doit pas modifier leurs largeurs.

Si plusieurs ajustements sont réalisés successivement, nous devons conserver la dernière présentation définie par l’utilisateur.

Les réglages sont propres à chaque utilisateur. Une modification ne doit donc pas affecter la présentation des autres utilisateurs.

L’ouverture, la consultation ou la fermeture d’une fenêtre ne doivent pas déclencher à elles seules une sauvegarde de sa présentation.

### 2. Restitution des réglages

À l’ouverture d’un tableau, Doc doit appliquer les derniers réglages enregistrés pour l’utilisateur.

Ils doivent être conservés après :

- fermeture puis réouverture du tableau ;
- fermeture puis réouverture de Doc ;
- déconnexion puis reconnexion ;
- connexion depuis un autre ordinateur de l’entreprise.

Lorsqu’aucune personnalisation n’existe, la présentation par défaut de Doc doit être utilisée.

Une nouvelle recherche, une actualisation, un changement de page de résultats ou un résultat vide ne doivent pas réinitialiser les réglages. Il en va de même lors de la consultation d’une fiche puis du retour aux résultats.

### 3. Présentation commune entre certaines vues

Les recherches standard et avancée d’un même type doivent partager leurs réglages.

Par exemple, une modification effectuée dans le tableau de recherche standard IAM doit être retrouvée à la prochaine ouverture de la recherche avancée IAM, et réciproquement.

En revanche, les tableaux IAM et OE conservent des configurations distinctes.

Les réglages doivent être associés au tableau, et non à la recherche effectuée ou à la référence produit consultée.

Il faudra recenser les tableaux de Doc et identifier les différentes vues devant partager une même présentation.

### 4. Gestion des fenêtres superposées

Doc est utilisé dans une seule session, avec une fenêtre principale et plusieurs fenêtres pouvant s’empiler.

Lorsqu’un utilisateur personnalise un tableau dans une fenêtre, les modifications sont enregistrées automatiquement. Les autres fenêtres déjà ouvertes conservent leur présentation à l’écran ; elles retrouveront les nouveaux réglages à leur prochaine ouverture.

Il faudra notamment couvrir le cas suivant :

1. La fenêtre principale affiche une première présentation.
2. L’utilisateur ouvre une fenêtre superposée présentant le même tableau.
3. Il y modifie la largeur ou l’ordre des colonnes.
4. Il ferme cette fenêtre, puis la fenêtre principale sans effectuer d’autre modification.
5. À la prochaine ouverture, il doit retrouver la présentation définie dans la fenêtre superposée.

La fermeture de la fenêtre principale ne doit donc pas écraser les réglages plus récents.

Si l’utilisateur modifie volontairement les colonnes de la fenêtre principale après avoir personnalisé la fenêtre superposée, la présentation résultant de cette dernière action devient celle à conserver.

### 5. Réinitialisation des colonnes

Une action intitulée « Réinitialiser les colonnes » doit permettre de retrouver la largeur et l’ordre prévus par défaut.

Cette action concerne uniquement :

- l’utilisateur connecté ;
- le tableau concerné et les vues partageant sa présentation.

Elle ne doit pas modifier les réglages des autres tableaux ou ceux des autres utilisateurs. Elle ne doit pas non plus effacer les critères de recherche, les filtres ou le tri en cours.

La réinitialisation doit être mémorisée pour les prochaines ouvertures. Les vues équivalentes déjà ouvertes conservent leur présentation jusqu’à leur prochaine ouverture.

L’utilisateur pourra ensuite personnaliser à nouveau ses colonnes.

### 6. Évolution des tableaux

Les colonnes disponibles restent identiques pour tous les utilisateurs. Seules leur largeur et leur position sont personnalisées.

Lorsqu’une nouvelle colonne est ajoutée dans Doc, elle doit être disponible pour tous. Dans les tableaux personnalisés, elle sera placée à la fin avec sa largeur par défaut, sauf spécification particulière.

Si plusieurs colonnes sont ajoutées, elles seront placées à la suite dans l’ordre défini par l’application.

La suppression d’une colonne ne doit pas provoquer d’erreur ni réinitialiser les autres réglages. Un changement de libellé seul ne doit pas faire perdre sa largeur ou sa position.

Pour un utilisateur n’ayant pas personnalisé le tableau, la présentation par défaut de la nouvelle version doit s’appliquer.

### 7. Gestion des erreurs

Une erreur de sauvegarde ou de chargement des préférences ne doit pas empêcher l’utilisateur de continuer à travailler.

En cas d’échec de sauvegarde :

- les modifications restent visibles dans la fenêtre courante ;
- un message non bloquant indique que les derniers réglages n’ont pas pu être enregistrés et risquent de ne pas être conservés ;
- une nouvelle modification déclenche une nouvelle tentative de sauvegarde de la présentation courante.

En cas d’échec de chargement, le tableau doit s’ouvrir avec sa présentation par défaut et l’utilisateur doit être informé. Cet affichage temporaire ne doit pas écraser automatiquement les préférences enregistrées.

Si une configuration n’est plus entièrement compatible avec un tableau, les réglages encore utilisables doivent être conservés autant que possible. Les valeurs par défaut seront appliquées aux éléments qui ne peuvent plus être restitués.

### 8. Vérifications attendues

Il faudra notamment vérifier que :

- les réglages sont retrouvés après réouverture de Doc et changement de poste ;
- deux utilisateurs conservent chacun leur présentation ;
- les recherches standard et avancée partagent les réglages attendus ;
- la fermeture d’une ancienne fenêtre n’écrase pas une modification plus récente ;
- la réinitialisation concerne uniquement le tableau et l’utilisateur concernés ;
- l’ajout, la suppression ou le changement de libellé d’une colonne préservent les réglages compatibles ;
- les recherches, les consultations de fiches et les actions dans les tableaux continuent de fonctionner ;
- tous les tableaux de Doc sont couverts.

### 9. Tâches complémentaires proposées

Nous proposons d’ajouter les trois tâches suivantes afin de compléter la réalisation et la recette. Leur périmètre reste à valider avant développement.

#### T01 — Vérifier les exports et les copier-coller après personnalisation

Le déplacement des colonnes ne doit pas entraîner de décalage entre les intitulés et les données lorsque l’utilisateur exporte ou copie les résultats.

Il faudra recenser les fonctions d’export, d’impression et de copier-coller disponibles dans les tableaux concernés, puis vérifier leur comportement après déplacement, redimensionnement et réinitialisation des colonnes.

Pour chaque fonction existante, nous devrons préciser si l’ordre de sortie suit la présentation personnalisée ou un format fixe. Les exports utilisés par d’autres outils devront conserver leur structure attendue, sauf demande d’évolution spécifique.

**Résultats attendus :**

- Le comportement attendu est documenté pour chaque fonction recensée.
- Chaque valeur reste associée au bon intitulé de colonne.
- Aucune donnée n’est tronquée dans un export du seul fait de la largeur choisie à l’écran.
- Les formats fixes utilisés par d’autres outils restent compatibles après personnalisation.

#### T02 — Mesurer les performances de la mémorisation

Nous souhaitons vérifier que la sauvegarde automatique et le chargement des préférences ne ralentissent pas l’utilisation des tableaux, notamment lorsque plusieurs ajustements sont effectués rapidement.

Il faudra comparer les temps d’ouverture avant et après l’évolution, sur des tableaux représentatifs et sur les postes de l’entreprise. Les essais devront également couvrir un tableau comportant de nombreuses colonnes, des modifications successives et un accès lent au stockage des préférences.

**Résultats attendus :**

- Les mesures avant et après évolution sont disponibles et réalisées dans des conditions comparables.
- Les seuils de temps de réponse acceptables sont définis avec le métier avant la recette.
- L’enregistrement ne bloque pas la manipulation du tableau.
- Après une succession rapide de modifications, la dernière présentation choisie est bien celle retrouvée à la réouverture.
- Une lenteur de chargement ou de sauvegarde ne provoque pas de perte silencieuse des derniers réglages.

#### T03 — Vérifier l’utilisation au clavier et la lisibilité des commandes

Nous proposons de vérifier que la personnalisation des colonnes et leur réinitialisation restent utilisables sans dépendre uniquement du glisser-déposer à la souris.

Il faudra examiner les possibilités offertes par les composants de tableaux, prévoir un accès au clavier aux actions de personnalisation et vérifier que les commandes sont compréhensibles, notamment lorsqu’elles sont représentées par des pictogrammes.

**Résultats attendus :**

- Un parcours au clavier permet de modifier l’ordre et la largeur des colonnes et d’accéder à la réinitialisation.
- L’élément sélectionné reste identifiable pendant les manipulations.
- Les commandes disposent d’un libellé explicite ou d’une description accessible.
- Après une modification, la navigation au clavier reste cohérente avec l’ordre visible des colonnes.
- Les éventuelles limites des composants sont documentées et une solution est arbitrée avant validation de cette tâche.

## Contraintes

### Couverture de l’application

Le besoin concerne tous les tableaux de Doc. Lorsqu’un tableau ne permet pas encore de déplacer ou de redimensionner ses colonnes, ces possibilités devront être ajoutées.

Les éventuelles limitations devront être identifiées et faire l’objet d’un arbitrage avant d’exclure un tableau ou certaines colonnes.

Les contraintes existantes, comme les colonnes fixes, les largeurs minimales ou maximales et les droits d’accès, devront être respectées.

### Conservation des préférences

Les préférences doivent suivre le compte utilisateur et être disponibles depuis un autre ordinateur.

Les tableaux et les colonnes devront être identifiés de manière stable afin qu’un changement de libellé ou de position ne fasse pas perdre les réglages.

L’enregistrement ne doit pas gêner les manipulations et une sauvegarde ancienne ne doit pas remplacer une modification plus récente.

### Affichage sur les postes de travail

Doc doit s’afficher en plein écran, aussi bien sur les ordinateurs portables que sur les grands écrans de l’entreprise.

Il reste à préciser le comportement lorsque les largeurs enregistrées dépassent l’espace disponible :

- conserver les largeurs avec un défilement horizontal ;
- ou adapter les largeurs pour afficher toutes les colonnes dans l’espace disponible.

Ce point devra être arbitré avant la réalisation du comportement d’adaptation à l’écran. Une adaptation automatique ne devra pas écraser les préférences choisies par l’utilisateur.

### Ordre de réalisation

Nous pouvons commencer par les tableaux de restitution de recherche IAM et OE afin de valider le fonctionnement, puis l’étendre aux autres tableaux.

Le périmètre final reste l’ensemble des tableaux de Doc.
