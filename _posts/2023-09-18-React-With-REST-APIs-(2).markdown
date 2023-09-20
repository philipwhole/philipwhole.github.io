---
layout: post
title:  "React with REST APIs (2)"
date:   2023-09-19 00:00:00 -0000
categories: [react, api, tutorial]
---
# React with REST APIs (MySQL)

    https://nodeserver.cyclic.cloud/getAllProducts

    https://nodeserver.cyclic.cloud/getProduct/:id

    https://nodeserver.cyclic.cloud/createProduct

    https://nodeserver.cyclic.cloud/updateProduct/:id
        
    https://nodeserver.cyclic.cloud/deleteProduct/:id

In react project, we use useFetch:

        import { useState, useEffect } from "react";

        const useFetch = (url, accessToken) => {
        const [data, setData] = useState(null);

        useEffect(() => {
            fetch(url, {
            headers: {
                Authorization: `Bearer ${accessToken}`,
            },
            })
            .then((res) => res.json())
            .then((data) => setData(data));
        }, [url, accessToken]);

        return [data];
        };

        export default useFetch;

In the Read component, we first declair url and accessToken, then call useFetch(url, accessToken):

        import React  from 'react'; 
        import useFetch from "./useFetch";

        export default function Read() { 
            const url="https://nodeserver.cyclic.cloud/getAllProducts"; 
            const accessToken = "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";
            const [data] = useFetch(url, accessToken);   
            
            return ( 
                <div> 
                    {data &&
                        data.map((item) => {
                        return <p key={item.id}>{item.id} {item.description} {item.prix} {item.image}</p>;
                    })}
                </div>
            );
        }
 

 
