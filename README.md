# Cours Ensicaen 2A (WIP)
## Introduction à Unity


## Introduction 
Dans ce TP, nous allons voir l'essentiel d'Unity, le tout en essayant d'appliquer une architecure propre 
respectant ce que vous avez déjà vu en cours (SOLID, etc.).
Nous allons réaliser un jeu aux mécaniques simples:
- On se déplace avec les flèches
- On saute avec la barre Espace
- Et on tire avec C

Nous ajouterons aussi un système de plaque de pression ainsi que des ennemis.
Les visuels sont tirés du pack Pixel Adventure 1, trouvable gratuitement sur l'Asset Store. 
Vous êtes libres d'utiliser d'autres visuels si vous le désirez.

N'hésitez pas à me contacter à tout moment sur l'adresse <errikubiak@gmail.com> ou par discord 
si vous êtes bloqués ou que vous avez la moindre question (même non relative au TP). 

Notions acquises durant le tp:
- Développement de comportements en utilisant les MonoBehaviour (MB)
- Utilisation des ScriptableObject (SO) pour stocker les données
- Instantiation d'objets
- Ajouts d'ennemis
- Prise en compte de la vie (joueur et ennemi)
- Affichage simple à l'aide de l'UI
- Utilisation des ScriptableObject pour coder des comportements
- Application des principes SOLID

Vous trouverez tout à la fin mes critères de notation.

Dans certaines sections, vous trouverez des sections:
- Aide, qui vous donnerons des astuces/solutions sur comment réaliser l'exercice. 
Essayez de l'utiliser qu'en dernier recours.
- Aller plus loin, cette section peut vous apporter du bonus sur la note finale si c'est correctement réalisé. 
**Attention**, ce n'est qu'un petit apport sur la note, je vous conseille de finir le TP en priorité. 

## Intialisation du projet
- Téléchargez le projet via git ou bien Gitlab en cliquant sur l'icône de téléchargement à côté de Clone.
- Vous remarquerez qu'il y a déjà dans le dossier Assets un certain nombre de dossiers:
    - Materials
    - Prefabs
    - Scenes
    - ScriptableObject
    - Scripts
    - Settings
Il est important de garder une architecture clair, même si vous ne comptez pas l'exporter. Cela évitera de vous perdre.

## Mouvement du personnage

### Mouvements latéraux du personnage
- Tout d'abord, prenez un des assets se trouvant sous PixelAdventure 1 et faites un glissez-déposez dans la scène. Ce sera notre personnage.
- Ajoutez un component BoxCollider2D et RigidBody2D dessus.
- Sous scripts créez un nouveau script et ajoutez le au joueur.
- Dans la méthode Update vous ferez en sorte que la position du joueur change en fonction des entrées.  

> **Vous utiliserez:**
> - La classe Input (méthode GetAxis)
> - Vous pouvez accéder à la position du joueur à travers transform.position 
> - Utilisation de variables modifiables dans l'éditeur. 
> - Utilisez des variables privées avec l'attribut SerializeField.

### Mouvement de la caméra
- Créez un script et ajoutez le sur MainCamera
- Faites en sorte que la caméra suive le joueur (de façon lissée de préférence).

> **Vous utiliserez:**
> - Les méthodes de lerp disponibles (Vector3)
> - Vous référencerez le joueur

<details>
 <summary> Aide </summary>
 Vous pouvez vous aider de ce [tuto](https://youtu.be/MFQhpwc6cKE). D'ailleurs je vous conseille cette chaîne!
</details>

### Saut du personnage
Vous allez maintenant faire en sorte que votre personnage puis sauter quand le joueur appuye sur la barre espace.
Vous ferez bien attention à ce que le joueur ait un nombre de sauts limités.

> **Vous utiliserez:**
> - La classe Input (GetKeyDown)
> - Vous pouvez vous servir de la physique afin de savoir si le joueur a retouché le sol (OnCollisionEnter2D, etc.)
> - RigidBody2D pour ajouter une force vers le haut
> - N'oubliez pas, GetComponent est gourmand !

<details>
<summary> Aller plus loin </summary>
Selon votre implémentation vous pouvez essayer de sauter en dessous d'une plateforme. 
Il se pourrait que le fait de la toucher avec la tête, permettent au joueur de sauter à nouveau.
Corrigez ce bug.
</details>

### Utilisation des ScriptableObject pour stocker les paramètres de notre joueur
**Qu'est ce qu'un ScriptableObject?**
Un SO est un peu comme un MonoBehaviour que vous pouvez enregistrer dans la hiérarchie de projet.
Vous codez une classe qui va représenter les données et les méthodes, quand un MB.
Ensuite, vous pourrez créer des instances très facilement, qui seront modifables dans l'éditeur d'Unity.
Cela a donc plusieurs avantages:
- Vous pouvez réduire le couplage dans votre code. 
En effet, plus besoin d'avoir un lien entre deux systèmes pour accéder à un seul paramètre/état.
Pour cela vous pouvez stocker des consantes, ou bien encore l'état d'un élément qui va changer dans le temps.
- Tout ce qui est modifié dans le SO quand on est en Play est enregistré.
- Les SO ne sont pas dépendant d'une scène à une autre
- Les SO ne possèdent pas les méthodes comme Update des MB, ils sont donc plus performants.

Vous pouvez regarder ce [tutoriel](https://www.youtube.com/watch?v=PVOVIxNxxeQ) si vous le souhaitez. 
Il résume bien ce que j'ai évoqué au-dessus et comment les créer/utiliser.

Utilisez maintenant un ScriptableObject afin de stocker les différentes données que vous avez pu utiliser pour le joueur.

### Application de principe d'inversion de dépendance (SOLID)
**Problème actuel dans le script qui contrôle notre joueur:**

Il ya un fort couplage entre le script de joueur et les entrées utilisateurs. 
Notre code est ainsi:
- Très peu modulable, par exemple on pourrait vouloir des entrées clavier pour le joueur 1 et utiliser une manette pour le second. 
Il nous faudrait donc recoder un script de joueur à chaque fois.
- Très difficilement testable, le fort couplage rend quasi impossible les tests unitaires.
- Difficilement débugguable, car plus on a de dépendances et de code plus c'est dur de s'y retrouver. 

> **Inversion de dépendance (Wikipédia):**
>
> En suivant ce principe, la relation de dépendance conventionnelle que les modules de haut niveau ont, par rapport aux modules de bas niveau, est inversée dans le but de rendre les premiers indépendants des seconds.

Pour résumer, il faut utiliser les interfaces le plus possible.
Malheuresement, les interfaces ne sont pas supportés comme attribut Sérializable. Mais elles fonctionnent avec les GetComponent, nous verrons ça plus tard.
Afin de contourner le problème, nous pouvons tout simplement utiliser des MonoBehaviour ou des ScriptableObject avec des méthodes abstraites.

Nous allons donc appliquer ce principe aux entrées utilisateurs afin de reduire le couplage.
Cela nous permettrai notamment de remplacer les entrées utilisateurs par une IA s'il y a besoin.

Veillez à trouver toutes les dépendances et à les extraire.
Vous aurez donc une classe abstraite qui définit des méthodes pour savoir si le joueur saute, etc. référencée dans le script Joueur.
Vous aurez aussi une implémentation concrète que vous placerez dans le champ Serializable.

### Application du principe de Responsabilité Unique
> **Responsabilité unique**
> [...] every class in a computer program should have responsibility over a single part of that program's functionality, which it should encapsulate

En résumé une classe doit avoir une seule responsabilité, ce qui apporte plusieurs avantages:
- Les classes sont plus faciles à comprendre
- Donc plus facilement débugguable
- Elle n'a qu'une raison de ne pas fonctionner

On voit que l'on pourrait appliquer ce principe sur notre joueur: 
la mécanique de saut n'a absolument pas besoin de connaître le fonctionnement de la mécanique de mouvements latéraux.

Vous séparerez votre script de joueur en deux MB, vous séparerez aussi les SO de configuration. 

### Application du principe de Ségrégation d'interface
**Problème actuel dans les entrées utilisateurs:**
Imaginons que l'on souhaite implémenter les entrées du joueur de cette façon:
- Quand on appuie sur un bouton de l'UI on saute
- Quand on fait un swipe on va dans la direction souhaitée

Si l'on garde notre interface actuel, on va avoir une implémentation très complexe, or on a vu au dessus que c'était une très mauvaise idée.

> **Ségrégation d'interface (Wikipédia):**
>
> [...] aucun client ne devrait dépendre de méthodes qu'il n'utilise pas

Pour résumer, il faut que les interfaces ait une méthode ou une raison d'exister. Ce qui fait qu'une seule raison de casser.

Cassez donc maintenant votre interface pour les entrées en plusieurs interfaces.

## Système plaque de pression-action


## Tir de balles


## Ajout d'ennemis


## Prise en compte des dégats


## Affichage de la vie


## Utilisation des ScriptableObject pour coder des comportements


## Utilisation d'un Asset Extérieur: LeanPool


## Rendu du TP

Veuillez me rendre le TP avec le dossier Unity complet, pas que le dossier Asset.
Le dossier du projet unity ainsi que le fichier compressé devront être sous la forme: 2A_NOM_PRENOM
Si ces consignes ne sont pas respectées 2 points seront retirés à la note.

Vous pouvez rendre le TP en binôme et le nom du rendu devra être de la forme: 2A_NOM_PRENOM_NOM_PRENOM

Chaque jour de retard entraînera 1 point de pénalité.

Critères de notation:
- Fonctionnel
- Qualité du code
- Architecture de qualité
