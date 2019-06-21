
# Ionic
- [Ionic 3](./version-3.md)
- [Ionic 4](./version-4.md)

Ionic 4 vs Ionic 3 — What you need to know about Ionic 4
https://ionicthemes.com/tutorials/about/ionic-4-vs-ionic-3


## CLI
`ionic g page tabs` to create new folder for logic tab

### Build
ionic build --prod 


## Tools
### Ionic Developer Hub
https://dashboard.ionicframework.com/hub

### Appflow
https://ionicframework.com/docs/appflow/

### Firebase

https://javebratt.com/ionic-firebase-tutorial-intro/

#### TIPS: JSON data structure
https://firebase.google.com/docs/database/rest/structure-data

## DATA

### NoSQL Data Modeling Techniques
https://highlyscalable.wordpress.com/2012/03/01/nosql-data-modeling-techniques/

### Lighthouse
npm install -g lighthouse
lighthouse URL-TO-TEST --view

#### Tets example
with demo app
```
ionic serve
lighthouse http://localhost:8100 --view
```
then compile and compare
```
ionic build --prod
http-server ./www -p 8888
lighthouse http://localhost:8888 --view
```

## Tutorials
https://ionicacademy.com/navigate-pages-ionic/
