# MONGO REALM

Nous allons creer une base de donnee MongoDB dans le cloud qui sera destinees a une application de chat.


## Installation

Connectez vous a mongodb cloud atlas qui vas initialiser notre bbase de donnee et creer un cluster AWS Region Irland.

[MONGODB REALM](https://cloud.mongodb.com/)

Plusieurs bases de donnees peuvent etre initialiser pour un seul cluster.




## Creation base de donnee (Atlas)

Cliquer sur "Collections"/ "Add My Own Data", indiquer le nom de la base donnee (ici: livecoding) et un nom de collections (ici: chat).

## Deployement application (Realm)

Partie serverless, c'est a dire la configuration de deployement de l'application.

Cliquez sur Realm et initialiser une nouvelle application (ici: ChatApp).

On peux a partir de l'interface Realm Mongodb:

* Creer des utilisateurs
* Creer des regles pour les utilisateurs, pour une application de chat sera necessaire d'etre parametre pour lecture et ecriture seulement par le createur. (Field name : userId).
* Configurer le push notification en parametrant au prealable firebase pour recuperer le sender Id. Mongodb utilise GCM Google Cloud Messagery.
* Creer des fontions, avantage principal de MongoDbRealm.


## Creation fonction dans Atlas

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

