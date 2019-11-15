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