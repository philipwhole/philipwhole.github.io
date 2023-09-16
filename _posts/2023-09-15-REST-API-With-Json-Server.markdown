---
layout: post
title:  "REST APIs with Json-server"
date:   2023-09-15 00:00:00 -0000
categories: [api, tutorial]
---

# Build REST APIs with Json-server

* Create a project json-server1: npm init
* Install json-server package: npm install json-server
* Add the following script to your package.json file:

        "scripts": {
            "start": "json-server --watch db.json",
        ...
    },

* include db.json in this repository
* deploy to cylic.cloud

# This it! 

Now you have your REST APIs over *https://json-server1.cyclic.cloud/* 

The complete code can be found at github: https://github.com/philipwhole/json-server