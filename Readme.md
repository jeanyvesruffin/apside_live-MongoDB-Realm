# MONGO REALM

Nous allons creer une base de donnee MongoDB dans le cloud qui sera destinees a une application de chat.


## Installation

Connectez vous a mongodb cloud atlas qui vas initialiser notre base de donnee et creer un cluster AWS Region Irland.

[MONGODB REALM](https://cloud.mongodb.com/)

Plusieurs bases de donnees peuvent etre initialiser pour un seul cluster.




## Creation base de donnee (Atlas)

Cliquer sur "Collections"/ "Add My Own Data", indiquer le nom de la base donnee (ici: livecoding) et un nom de collections (ici: chat).

## Deployement application (Realm)

Partie serverless, c'est a dire la configuration de deployement de l'application.

Cliquez sur Realm et initialiser une nouvelle application (ici: ChatApp).

On peux a partir de l'interface Realm Mongodb:

* Creer des utilisateurs
* Creer des regles pour les utilisateurs, pour une application de chat sera necessaire d'etre parametre pour lecture et ecriture seulement par le createur. (Field name : userId).
* Configurer le push notification en parametrant au prealable firebase pour recuperer le sender Id. Mongodb utilise GCM Google Cloud Messagery.
* Creer des fontions, avantage principal de MongoDbRealm.


## Creation fonction dans Atlas

Apres avoir donnee un nom a la fonction, nous avons la possibilite de donnee un acces a celle-ci soit par l'userId ou autres (ici system, car les message de chat transistera une autorisation du system).

Nous la mettrons la fonction prive a MongoDb.

Exemple de fonction qui gerera les pop up de notification:

```js
exports = function(arg){
  const notif = context.services.get("gcm").send({
    to:"/topics/mongodb",
    "priority":"normal",
    "notification":{
      "title":"Vous avez un nouveau message"
    }
  });
  return {arg: arg};
};
```

## Utilisation des trigger de base de donnee

On peux ajouter des triggers, dans notre exemple de chat nous pourrions ajouter un trigger lors d'un echange de message dans notre chat, afin que l'utilisateur recoive une notification d'un nouveau message.

Pour cela cliquer sur trigger, completer le formulaire puis selectionner le type de declencheur ici insert (lors d'un insert en base de donnee on envoi une notification).


## Utilisation de l'hosting.

Permet de deployer les applications.

**IMPORTANT**

Dans **Tous** les cas une authentification sera necessaire, pour acceder a Mongodb. Soit logging mot de passe, soit coder une maniere d'authentification *anonyme*.


## Rappel transactionnel ACID

En informatique, les proprietes ACID (atomicite, coherence, isolation et durabilite) sont un ensemble de proprietes qui garantissent qu'une transaction informatique est executee de façon fiable.

**Atomicite**

La propriete d'atomicite assure qu'une transaction se fait au complet ou pas du tout : si une partie d'une transaction ne peut être faite, il faut effacer toute trace de la transaction et remettre les donnees dans l'etat où elles etaient avant la transaction. L'atomicite doit être respectee dans toutes situations, comme une panne d'electricite, une defaillance de l'ordinateur, ou une panne d'un disque magnetique.

**Coherence**

La propriete de coherence assure que chaque transaction amenera le systeme d'un etat valide a un autre etat valide. Tout changement a la base de donnees doit être valide selon toutes les regles definies, incluant mais non limitees aux contraintes d'integrite, aux rollbacks en cascade, aux declencheurs de base de donnees, et a toutes combinaisons d'evenements.


**Isolation**

Toute transaction doit s'executer comme si elle etait la seule sur le systeme. Aucune dependance possible entre les transactions. La propriete d'isolation assure que l'execution simultanee de transactions produit le même etat que celui qui serait obtenu par l'execution en serie des transactions. Chaque transaction doit s'executer en isolation totale : si T1 et T2 s'executent simultanement, alors chacune doit demeurer independante de l'autre.

**Durabilite**

La propriete de durabilite assure que lorsqu'une transaction a ete confirmee, elle demeure enregistree même a la suite d'une panne d'electricite, d'une panne de l'ordinateur ou d'un autre probleme. Par exemple, dans une base de donnees relationnelle, lorsqu'un groupe d'enonces SQL a ete execute, les resultats doivent être enregistres de façon permanente, même dans le cas d'une panne immediatement apres l'execution des enonces.
