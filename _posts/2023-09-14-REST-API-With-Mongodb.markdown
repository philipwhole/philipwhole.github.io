---
layout: post
title:  "REST APIs with Mongodb"
date:   2023-09-14 00:00:00 -0000
categories: [api, tutorial]
---

# Build REST APIs with Mongodb

I built REST APIs with SQLDB connection, now I decide to challenge myself with Atlas mongodb.

The final APIs were deployed on cyclic
 
* https://node-serveur2.cyclic.cloud/getAllActors

* https://node-serveur2.cyclic.cloud/getActor/:actor_id

* https://node-serveur2.cyclic.cloud/getActor/addActor

* https://node-serveur2.cyclic.cloud/updateActor/:actor_id
    
* https://node-serveur2.cyclic.cloud/deleteActor/:actor_id

Access token: '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'

// Connect to your MongoDB database
mongoose.connect('mongodb+srv://root:JoB0QY9yqosg2EIa@cluster0.na3ur4v.mongodb.net/cluster0')
mongoose.connection.once('open', () => {
    console.log('conneted to database');
});

The complete code can be found at github: *https://github.com/houzhihuil/node_Serveur2*