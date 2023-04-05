# NodeJs_Api
Api Rest avec NodeJS  pour une gestion de stocks de télévision

# Installation de Node JS

https://nodejs.org/fr/


# Installation de Express Js 

npm install express

# Installation de Nodemon

(relance automatiquement votre serveur node à chaque fois que vous sauvegardez votre fichier)

npm install nodemon 

# Lancement du serveur

nodemon index.js

```javascript
const express = require('express')
const app = express()
const products = require('./products.json')

// Middleware
app.use(express.json())

app.get('/products', (req,res) => {
    res.status(200).json(products)
})

app.get('/products/:id', (req,res) => {
    const id = parseInt(req.params.id)
    const product = products.find(product => product.id === id)
    res.status(200).json(product)
})

app.post('/products', (req,res) => {
    products.push(req.body)
    res.status(200).json(products)
})

app.put('/products/:id', (req,res) => {
    const id = parseInt(req.params.id)
    let product = products.find(product => product.id === id)
    product.name =req.body.name,
    product.quantity =req.body.city,
    product.unitprice =req.body.type,
    res.status(200).json(product)
})


app.delete('/products/:id', (req,res) => {
    const id = parseInt(req.params.id)
    let product = products.find(product => product.id === id)
    products.splice(products.indexOf(product),1)
    res.status(200).json(products)
})

app.listen(8080, () => {
    console.log("Serveur à l'écoute")
})
```
