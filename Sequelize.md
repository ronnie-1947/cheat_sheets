Sequelize
============

## Initialize in Node JS
```
const Sequelize = require('sequelize');

const sequelize = new Sequelize('<database_name>', '<username>', 'password', {
    dialect: 'mysql',
    host: 'localhost'
})
module.exports = sequelize;
```
_____________________________________

## Creating table & Inserting
Timestamp (created_At, updated_At) are added automaticaly by sequelize . It can be configured and can be set to none .
```
//---- Making required table, if not exist

const Product = sequelize.define('product', {
    id: {
        type: Sequelize.INTEGER,
        autoIncrement: true,
        allowNull: false,
        primaryKey:true
    },
    title:{
        type: Sequelize.STRING,
        allowNull: false 
    },
    price:{
        type: Sequelize.DOUBLE,
        allowNull: false
    },
    imageUrl: {
        type: Sequelize.STRING,
        allowNull: false
    },
    description:{
        type: Sequelize.TEXT,
        allowNull: false
    }
}, 
{
    timestamp: true/false
})


 //---- Insert values in table 'products'

Product.create({
        title: this.title, 
        imageUrl: this.imageUrl, 
        price: this.price, 
        description: this.description
    })
    .then((res)=>{console.log(res)});
```
_____________________________________

## Quering

SELECT DATA from TABLE
```
Product.findAll({
    attributes: [column_name-1, column_name-2],
    where: {
        id: x
    }
})
```
Find by its ID
```
Product.findByPk(id)
    .then(result=>{})
    .catch(err=> {})
```

_____________________________________

## Updating
2 Methods to UPDATE
```
Product.findByPk(prodId)
    .then(prod=>{
        prod.update({
            title: updatedtitle, 
            price: updatedPrice, 
            imageUrl: updatedImgUrl,
            description: updatedDesc
        })
        .then(result=>{console.log(`It worked !!`)})
        .catch(err=>{console.log(`updating not working`)})  
    })
    .catch(err=>{ throw err});
```
```
Product.findByPk(prodId)
    .then(prod=>{
        prod.title= updatedtitle; 
        prod.price= updatedPrice; 
        prod.imageUrl= updatedImgUrl;
        prod.description= updatedDesc;
        return prod.save();
    })
    .then(result=> {console.log(`Done updating`)})
    .catch(err=>{console.log(`updating not done`)});
```

__________________________________

## DELETING or DESTROY
1st method:
```
Product.destroy({where:{id: prod_id}})
.then(result=>{})
.catch(err=>{console.log(`Error in deleting`)})
res.redirect('/admin/products');
```
2nd method : By taking help from "findByPk"
```
Product.findByPk(id)
    .then(result=>{
        result.destroy();
        res.redirect('/admin/products');
    })
    .catch(err=>{console.log(`Deleting not worked`)})
```
__________________________________

## RELATIONS of Tables
| Associations | Description |
| ------- | ----------- |
| `belongsTo` |One to one|
| `belongsToMany` |Many to many Used for associating tables|
| `hasMany` |One to many |
| `hasOne` |One to one |

__________________________________

## CASE : When user_id is stored in Request
This case is achieved where we have to store User_id in Creating tables.
```
req.user.Create<table_name>({
    this.title: title,
    this.foo: foo
})
.then()
.catch()
```