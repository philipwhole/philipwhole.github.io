---
layout: post
title:  "Add access token for REST API"
date:   2023-09-14 00:00:00 -0000
categories: [blog, tutorial]
---

# Add access token for REST API

I have created a node.js project to generate Rest APIs and it works well. Now I want to add access token to secure the APIs. Here are my steps:

// Middleware to check API token

    app.use((req, res, next) => {
        const apiToken = req.headers.authorization;

        if (!apiToken || !apiToken.startsWith('Bearer ')) {
            // Authorization header is missing or doesn't start with 'Bearer '
            return res.status(401).json({ error: 'Invalid authorization header' });
        }

        const token = apiToken.substring(7); // Remove 'Bearer ' prefix
        const presetApiToken = 'ghp_*****************************'; // Replace with your own preset API token

        if (token === presetApiToken) {
            // API token is valid, proceed to the route
            next();
        } else {
            // API token is invalid, return an error response
            res.status(401).json({ error: 'Invalid API token' });
        }
    }); 

Now, let's test it. I used to test REST APIs in postman, which always worked very well. But today I want try another tool: <a href="https://testfully.io/">Testfully.</a>

1. Signup if you do not have an account, then login.
2. In the Requests interface, fill in the token credential required in Headers.
    name: Authorization, value: ghp_*****************************// Replace with your own preset API token, without quotation.
3. Continue the requests and send. 

## Now you can interact with your APIs data. Congratulations!