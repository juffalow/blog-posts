---
title: 'Introduction to Sequelize ORM'
media_order: sequelize-large.png
taxonomy:
    tag:
        - JavaScript
        - Node.js
header_image_file: sequelize-large.png
header_image_width: 128
header_image_height: 128
content:
    items: '@self.children'
    limit: '5'
    order:
        by: date
        dir: desc
    pagination: '1'
    url_taxonomy_filters: '1'
---

This is about the basic usage of Sequelize ORM – how to create migrations, models, seeders and how to get the data from the database. Everything is explained on an example project, which you can find on GitHub.

===

## Example project

Let’s create a simple application that connects to a database and get some informations from it. To make it really simple we create a table with two columns. The first is numeric primary key ( auto increment ) and the second is some string – a quote for example.

<table class="table table-bordered">
	<thead>
		<tr>
			<td>id</td>
			<td>quote</td>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td>1</td>
			<td>First, solve the problem. Then, write the code.</td>
    	</tr>
		<tr>
			<td>2</td>
			<td>Any fool can write code that a computer can understand. Good programmers write code that humans can understand.</td>
		</tr>
		<tr>
			<td>3</td>
			<td>If you stop learning, then the projects you work on are stuck in whatever time period you decided to settle.</td>
		</tr>
	</tbody>
</table>

#### Create new project

Let’s create a directory for our project :

```
$ mkdir sequelize-example
```

Now we have to initialize the project :

```
# jump into project directory
$ cd sequelize-example

# initialize project ( you may use npm init )
$ yarn init
```

#### Add dependencies

We are going to use `sequelize` and `sequelize-cli` packages along with `mysql` package.

```
$ yarn add sequelize sequelize-cli mysql
```

#### Directory structure

```
sequelize-example/
    index.js
    package.json
```

#### Database creation

You can design your database on paper or in some tool and generate sql file ( or write it yourself ), run it and create tables this way. In this case :

```
DROP TABLE IF EXISTS quotes;

CREATE TABLE quotes (
    id INT(11) NOT NULL AUTO_INCREMENT,
    quote TEXT NOT NULL,
    PRIMARY KEY ( id )
);

INSERT INTO quotes(quote) VALUES
('First, solve the problem. Then, write the code.'),
('Any fool can write code that a computer can understand. Good programmers write code that humans can understand.'),
('If you stop learning, then the projects you work on are stuck in whatever time period you decided to settle.');
```

This is ok, but think about the following scenario : you are working on a project with couple of other programmers. They will add some additional quotes and later, you will decide to add another column author. In this case, you have to drop the original table and create new table with the new column. Where is the problem? You will loose all your data. Maybe this is not a problem in devevelopment environment but what you do in production? What if the new feature cause errors and you have to revert it?

#### Migrations

You can find the solution in migrations. Migrations are a way of changing your database from one version to another. If we take the above example – add new author column – we would create new migration script which contains two methods : `up` and `down`. The `up` will alter the table and add the new column. The `down` will do the reverse action, i.e. alter the table and drop the column.

So when you create the new feature and other developers pulls the new version of code, run migrations, they end up with the same database as you have with their original data.

#### Initialize Sequelize

To initialize sequelize run the code below. It will create default directories *config*, *migrations*, *models* and *seeders* with *config/config.json* and *models/index.js* files.

```
$ ./node_modules/sequelize-cli/bin/sequelize init
```

#### Create first migration

We have to create the first migration, let’s name it *initial* :

```
$ ./node_modules/sequelize-cli/bin/sequelize migration:create --name initial
```

This will generate new file in *migrations* directory with those two methods, `up` and `down` :

```
'use strict';

module.exports = {
  up: function (queryInterface, Sequelize) {
    /*
      Add altering commands here.
      Return a promise to correctly handle asynchronicity.

      Example:
      return queryInterface.createTable('users', { id: Sequelize.INTEGER });
    */
  },

  down: function (queryInterface, Sequelize) {
    /*
      Add reverting commands here.
      Return a promise to correctly handle asynchronicity.

      Example:
      return queryInterface.dropTable('users');
    */
  }
};
```

We have to modify this file, so that in `up` method it will create our table and in `down` method it will drop this table :

```
'use strict';

module.exports = {
    up: function (queryInterface, Sequelize) {
        return queryInterface.createTable('quotes', {
            id: {
                type: Sequelize.INTEGER,
                primaryKey: true,
                autoIncrement: true
            },
            quote: {
                type: Sequelize.STRING,
                allowNull: false
            },
        });
    },

    down: function (queryInterface, Sequelize) {
        return queryInterface.dropTable('quotes');
    }
};
```

#### Config

Now create a database on your localhost ( or wherever else ) and edit *config/config.json* file :

```
{
    "development": {
        "username": "root",
        "password": null,
        "database": "sequelize-example",
        "host": "127.0.0.1",
        "dialect": "mysql"
    },
    "text": {
        ...
    },
    "production": {
        ...
    }
}
```

#### Run the migrations

When you have ready your migration file and config file you can run it :

```
$ ./node_modules/sequelize-cli/bin/sequelize db:migrate
```

As description says, this command runs every not yet executed migration. If you run it for the first time, it creates new table *SequelizeMeta* where sequelize stores informations about already executed migrations. The migrations which you run are then immutable. If you want to change something, you have to write new migration script.

#### Writing the model

Now when you look on the database, it should have the *quotes* table ( and the *SequelizeMeta* table ). If so, you can go on and create new file *quotes.js* in models direcory.

```
module.exports = function(sequelize, DataTypes) {
    var quotes = sequelize.define('quotes', {
        id: {
            type: DataTypes.INTEGER,
            autoIncrement: true,
            primaryKey: true,
            field: 'id'
        },
        quote: {
            type: DataTypes.STRING,
            allowNull: false,
            validate: {
                notEmpty: true
            }
        }
    }, {
        // don't add the timestamp attributes (updatedAt, createdAt)
        timestamps: false,
        // disable the modification of table names; By default, sequelize will automatically
        // transform all passed model names (first parameter of define) into plural.
        // if you don't want that, set the following
        freezeTableName: true // here it is useless but once I had troubles because of this so I add it here
    });

    return quotes;
}
```

Sequelize creates two columns `updatedAt` and `createdAt` by default. If you don’t want them, you have to pass an object as second parameter which contains `timestamps` key set to `false`. FYI, if you name your models ( tables ) in singular, sequelize will automatically transform them into plural. To prevent this, add `freezeTableName` key and set it to `true`.

#### Index.js

Almost everything is ready. In the main file, *index.js*, we are going to initialize the sequelize and then write a function that load everything from the table and print it in form “id | quote”.

```
var models = require('./models/index.js');

function startApp() {
    models.quotes.findAll().then(function(quotes) {
        for( var i = 0; i < quotes.length; i++ ) {
            console.log(quotes[i].id + " | " + quotes[i].quote);
        }
    });
}

models.sequelize.sync()
.then(function() {
    startApp();
})
.catch(function (e) {
    throw new Error(e);
});
```

You can try it by running the command <kbd>node index.js</kbd>. In this state, it only displays message like *Executing (default): SELECT `id`, `quote` FROM `quotes` AS `quotes`;* and nothing else, because the table is still empty. You can insert there a few rows manualy and try it again. Or you can use another ORM feature : seeders.

#### Seeders

When you want to add initial data to the database ( seed the database ) you use seeders. Create new seed using sequelize cli again :

```
$ ./node_modules/sequelize-cli/bin/sequelize seed:create --name firstthree
```

This will create new file in *seed* directory with some example code. Edit this file as follows :

```
'use strict';

module.exports = {
    up: function (queryInterface, Sequelize) {
        return queryInterface.bulkInsert('quotes', [
            { quote: 'First, solve the problem. Then, write the code.' },
            { quote: 'Any fool can write code that a computer can understand. Good programmers write code that humans can understand.' },
            { quote: 'If you stop learning, then the projects you work on are stuck in whatever time period you decided to settle.' }
        ]);
    },
    down: function (queryInterface, Sequelize) {

    }
};
```

You can run the seeders with the command below. By default it will not keep track of executed seeds. ( You can change this behavior in *config.json* file. Read more in Sequelize CLI docs. )

```
$ ./node_modules/sequelize-cli/bin/sequelize db:seed:all
```

## Conclusion

If you run the script now, it should print something like this :

```
Executing (default): SELECT id, quote FROM quotes AS quotes;
1 | First, solve the problem. Then, write the code.
2 | Any fool can write code that a computer can understand. Good programmers write code that humans can understand.
3 | If you stop learning, then the projects you work on are stuck in whatever time period you decided to settle.
```

And we are done! You can find the project on [GitHub](https://github.com/juffalow/sequelize-example). Next time I am going to write about how to register users with Sequelize and how to create associations. If you have any questions, leave me a comment or send me an email.

#### References

[Sequelize](http://docs.sequelizejs.com/)

[Sequelize CLI](https://github.com/sequelize/cli)

[Wikipedia - Schema migrations](https://en.wikipedia.org/wiki/Schema_migration)

[Michael Brunton-Spall - Database Migrations Done Right](http://www.brunton-spall.co.uk/post/2014/05/06/database-migrations-done-right/)
