SAAS
====
## Getting Started 
```
npm install node-sass --save-dev // command

"compile:sass": "node-sass sass/main.scss css/style.css -w" // change in package.json file
```

## Architecture
7 folder architecture available on - #

* Base > Animation + Base + Typography + Utilities
* Components > Btn
* Layout > Header + Sidebar
* Pages > Home + About
* Themes 
* Abstract > Abstracts + Mixins + Variables
* Vendors

## BEM

Block__Element--Modifier

## Variables

Variables can be used to store any values
```
$default-font-size: 1.6rem;
color-primary:	#0085ab;
$grid-width:114rem;
```

## Functions
```
@function divide($a, $b){ // Function generation
    @return $a / $b ;
}

margin: divide(60 , 2)*1px // Use above function in Sass
```



## Mixins
Variable to store codes.

```
@mixin clearfix {
    &::after{
        content: "";
        display: table;
        clear: both;
    }
}

@include clearfix; // use this to copy the mixin in code
```

## Extends
@extend is a feature of Sass that allows classes to share a set of properties with one another
```
%btn-placeholder{
    padding: 10px;
    display: inline-block;
}

.btn-main{   // Way to use extend 
    @extend btn-placeholder
}
.btn-secondary{
     @extend btn-placeholder
}
```

## Saas syntax

| Syntax| Description |
| ------- | ----------- |
| `&__` | Used for Nesting |
| `&>*` | Select the first child inside it |
| `[class^="col-"]{ }` | Select all the classes begin with 'col-' |
| `&:not(:last-child)` | Donot select the last child |


