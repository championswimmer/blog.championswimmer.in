---
layout: post
published: true
mathjax: false
featured: false
comments: false
title: 'Very Strict Sequelize: Into the land of Typescript Overkill'
description: Using Typescript in an Express NodeJS server using Sequelize for ORM
headline: Using Typescript in an Express NodeJS server using Sequelize for ORM
categories:
  - webdevelopment
tags: 'nodejs,sequelize,typescript,javascript'
---

> *Disclaimer: If you believe that Typescript is a conspiracy by Microsoft to turn Javascript into C# and you believe in the religion of monkey-patching objects, then please do not proceed further. This article will burn your soul if you read it*

## Typescript: Enforce 'correct' code
Ever since I have started using Typescript, I cannot imagine writing large projects in plain JS anymore. 
My relationship to Typescript is best explained by this very nicely [articulated piece by Tom Dale](https://medium.com/@tomdale/glimmer-js-whats-the-deal-with-typescript-f666d1a3aad0) 

While Typescript can make working with frontend frameworks like Angular and Vue a whole new revelation, using it on backend projects can have equally pleasing results. 
What I like about Typescript is the strictness can be turned a few notches up or down based on your requirements. And as you keep turning the strictness up by notches, you start seeing Typescripting starting to block off all those cracks and crevices in your code that were sitting there, waiting for you to slip into them at runtime, and cause hard-to-debug errors.

Since Typescript brings in the concept of **compilation** before **running** the code, the obvious error-prone code blocks will be straightaway flagged by Typescript, and you'll have to fix them before you can even get to the **running** stage

## Sequelize: A good target
Here is an example of a run of the mill Sequelize code - 

```javascript
const Sequelize = require('sequelize')
const db = new Sequelize('db', 'user', 'pass', {
	dialect: 'postgres'
})

const User = db.define('users', {
	// line A
	name: Sequelize.STRING,
	age: Sequelize.INTEGER,
	city: Sequelize.STRING,
	isAdmin: {type: Sequelize.BOOLEAN, defaultValue: false}
})

db.sync().then(() => {
	User.create({
		// line B
		name: 'Arnav', age: 23, city: 'Delhi', isAdmin: true
	}).then(user => {
		// line C
		console.log(user.age) // 23
	})
})
```

Usual points of failure of this code are at: 
 - Line A: What if I miss any keys while defining, or jumble up the properties like `type` and `defaultValue`
 - Line B: What if I miss any keys, or mis-type the key name
 - Line C: What if I do not use correct key names when fetching data from `user`

Most IDEs like _Webstorm_ or _VS Code_ will only give rough suggestions, and not very indicative autocompletes either. 

When dealing with 15-20 models in your project (usual mid-size node/express projects get there pretty fast), you'd want a little more strictness around your models' keys and better autocomplete would always be welcome. 

## Typescript + Sequelize: We're pretty much in Java/C# land here
To start using Typescript, you'll need `typescript` module, and to make better use of types from **sequelize** you should also get the `@types/sequelize` module

```shell
npm install -D typescript @types/sequelize
```

You can check out what the entire project structure looks like [here](https://github.com/championswimmer/dukaan-backend/tree/fcdc211e51f6c864063d1dca513fbb7c579a6a34), but here is the lowdown - 

Here is what my _**`/db/models/Client.ts`**_ file looks like - 
```typescript
import {DefineAttributes, Instance, Model} from 'sequelize'
import * as Sequelize from 'sequelize'

// This is signature of my 'Client' objects
// Whenever I create 'Client' rows, the objects will be of this type
export interface ClientAttributes {
    id: number,
    secret: string,
    whitelist_domains: string[],
    whitelist_ips: string[],
    redirect_url: string[]
}

// To define Client in db, the columns must be one of the 
// keys of 'ClientAttributes', hence (( in keyof ClientAttributes )) 
type ClientDefineAttributes = {
    [x in keyof ClientAttributes]: string | DataTypeAbstract | DefineAttributeColumnOptions

}

// When writing this, I cannot stray away from **having to**
// use keys as in 'ClientAttributes'
// In Webstorm/VSCode I get autocomplete too
export const clientAttrs: ClientDefineAttributes = {
    id: {
        type: Sequelize.INTEGER,
        primaryKey: true,
        autoIncrement: true
    },
    secret: Sequelize.STRING(32),
    whitelist_domains: Sequelize.ARRAY(Sequelize.STRING),
    whitelist_ips: Sequelize.ARRAY(Sequelize.STRING),
    redirect_url: Sequelize.STRING
}

export interface ClientInstance extends Instance<ClientAttributes> {
    get(): ClientAttributes
}

export interface Client extends Model<ClientInstance, ClientAttributes> {

}

```

And then once many such models are defined, my _**`/db/index.ts`**_ will look like this - 

```typescript
import * as Sequelize from 'sequelize'
import {DataTypes} from 'sequelize'
import * as config from '../../config'
import {ClientAttributes, clientAttrs, ClientInstance} from './models/Client'
import {ProductCategoryAttributes, productCategoryAttrs, ProductCategoryInstance} from './models/ProductCategory'
import {TaxAttributes, taxAttrs, TaxInstance} from './models/Tax'

const db = new Sequelize(
    config.DB.NAME,
    config.DB.USER,
    config.DB.PASSWORD,
    {
        dialect: 'postgres'
    }
)
export const Clients =
    db.define<ClientInstance, ClientAttributes>('clients', clientAttrs)

export const ProductCategories =
    db.define<ProductCategoryInstance, ProductCategoryAttributes>(
        'product_categories',
        productCategoryAttrs
    )
export const Tax =
    db.define<TaxInstance, TaxAttributes>(
        'tax',
        taxAttrs
    )

```

## Leverage Live Templates: Do not write boilerplate
As long as you're using a good IDE (or even Vim with the appropriate plugins), you do not need to write those cumbersome boilerplates all over again. 
We can create _Apache Velocity_ templates for the define statement and the model file.

Here is what my Model file template looks like - 
NOTE: `${NAME}` is file name
```typescript
#set($fileName = $NAME.substring(0,1).toLowerCase() + $NAME.substring(1))
import {DataTypeAbstract, DefineAttributeColumnOptions, DefineAttributes, Instance, Model} from 'sequelize'
import * as Sequelize from 'sequelize'

export interface ${NAME}Attributes {
    id: number
}

type ${NAME}DefineAttributes = {
    [x in keyof ${NAME}Attributes]: string | DataTypeAbstract | DefineAttributeColumnOptions
}

export const ${fileName}Attrs: ${NAME}DefineAttributes = {
    id: {
        type: Sequelize.INTEGER,
        primaryKey: true,
        autoIncrement: true
    }
}

export interface ${NAME}Instance extends Instance<${NAME}Attributes> {
    get(): ${NAME}Attributes
}

export interface ${NAME} extends Model<${NAME}Instance, ${NAME}Attributes> {

}

```

So as soon as I generate a file of name **MyModel.ts** all the scaffolding is already done for me. 

Similarly I have created a live template to generate the `db.define` statements - 

```typescript
export const $NAME$ =
    db.define<$NAME$Instance, $NAME$Attributes>(
        '$SNAKENAME$',
        $SMALLNAME$Attrs
    )
```

## Autocomplete Bliss  + No Typo Mayhem in run time

Whenever I want to create objects, I can see the keys and their types in autocomplete
![Create Autocomplete](https://imgur.com/JopsxMx.jpg)

Unless the object I save is exactly right, I get a compile error 
![Error](https://imgur.com/uVdwNL5.jpg)

And I get similar fluent autocomplete in the query result item as well
![Result](https://imgur.com/Rg0NPvg.jpg)

And finally I get the same luxury on `where` queries too 
![Where](https://imgur.com/q9gwSr4.jpg)

## Typescript: Invest in boilerpate for long term safety
A little bit of investment into boilerplate; whether you type manually everytime - which IMHO is a bit cumbersome or you take advantage of your IDE/Editor and create templates; is required when using Typescript, but the advantages we get in terms of code completion and more importantly, compile-time error checking, even before we go into run-time mode, is well worth the time invested. 


> Could not have written without [StackEdit](https://stackedit.io/). 

----------

_**Obligatory shameless self-plug :**_  
I am one of the co-founders of [Coding Blocks](https://codingblocks.com) - A Software Programming bootcamp, based out of New Delhi, India. Among other things, we teach Full Stack Web Development using NodeJS, via both classroom programmes, as well as online classes. You can follow our Medium to find more articles on Android and Web development. 


