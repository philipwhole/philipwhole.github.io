---
layout: post
title:  "REST APIs with MySQL"
date:   2023-09-15 00:00:00 -0000
categories: [api, tutorial]
--- 

REST APIs were deployed on cyclic
 
        https://nodeserver.cyclic.cloud/getAllProducts

        https://nodeserver.cyclic.cloud/getProduct/:id

        https://nodeserver.cyclic.cloud/getActor/createProduct

        https://nodeserver.cyclic.cloud/updateProduct/:id
            
        https://nodeserver.cyclic.cloud/deleteProduct/:id

Access token: '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'

// Connect to your MySQL database

    const con= mysql.createConnection({
        host:"mysql-philippehou.alwaysdata.net", 
        user: "289337_root",
        password: "Tm3x-****************Mf",
        database:"philippehou_gestionproduit" 
    });
 
con.connect(function(err){
    if(err) throw err; 
});

The complete code can be found at github: *https://github.com/philip-whole/nodeserver*

*Test:*

to get all products:
curl -X GET -H "Authorization: Bearer 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" https://nodeserver.cyclic.cloud/getAllProducts

to get a single product with id:
curl -X GET -H "Authorization: Bearer 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" https://nodeserver.cyclic.cloud/getProduct/1