# Bunq Chat
A chat application API implementation written for the Bunq interview process using Laravel.

## Deployment
This app has been deployed to **Heroku**. You can view the demo [here](http://rhaarhoff-bunq-chat.herokuapp.com/api).

## Requirements
There are a few things needed in order to setup this project.
- PHP
- Apache / Nginx
- SQLite
- **[Composer](https://getcomposer.org/)**
- **[Laravel](https://laravel.com/docs/5.6/installation)**

## Installation
First, clone this repository and navigate to the directory where it has been downloaded. Follow the instructions below:

- Run the Composer installation
```jshelllanguage
$ composer install
```
- Create a `.env` file
```jshelllanguage
$ cp .env.example .env 
```
- Update the `.env` file with all needed information.
- Generate a key to your `.env` file
```jshelllanguage
$ php artisan key:generate
```
- Create a file called `database.sqlite` in the `storage` folder.
- Migrate the database. You can add `--seed` if you would like to seed the database with dummy data.
```jshelllanguage
$ php artisan migrate --seed
```
- If you want to seed the database after you have already migrated, run the following
```jshelllanguage
$ php artisan db:seed
```

## Implementation

### Summary
**Laravel**, which is a MVC (Model View Controller) framework is used in this implementation, alongside a **Service** and **Repository** design pattern. 

Communication through the API happens over a simple JSON based protocol over HTTP.

The implementation is using a simple SQLite database.

### Models
Using the Eloquent ORM, models can be created that map directly to database tables. Thus, we have two Models called
**User** and **Message**. Models can have relationships specified that map to database relationships. In the **Message** model
you can see two relationships `from` and `to` which link to the **User** model. These relationships make is easier to make queries 
through Eloquent and load data from the related models.

### Controllers
Controllers are responsible for controlling the flow of the application over the HTTP requests. 
A request will link to a function within a specified controller, which will then be passed along to a **Service**. We have two Controllers called **UserController** and **MessageController**.

### Services
The logic is written within the Services. Incoming requests, validation and responses are all handled by a Service.
The services are written in such a way that the Eloquent ORM is never used, making it available for other resources to 
work with. We have two Services called **UserService** and **MessageService**.

### Repositories
A Repository is an abstraction of the data layer and a centralised way of handling our models. 
The idea with this pattern is to have a generic abstract way for the app to work with the data 
layer without being bothered with if the implementation is towards a local database or towards an online API. 
We have two Repositories called **UserRepository** and **MessageRepository**.

### Factories
Factories give us an easy way to create dummy data for testing purposes. A factory can create random data for your models and
store them to the database. To view the factories visit the `database/factories` folder. We have two factories called
**UserFactory** and **MessageFactory**.

## API
These are the current API routes that are available. You can view them in the `api.php` file in the `routes` folder.

To access these routes, simply use the `/api/` prefix on the url. Some examples:
```jshelllanguage
http://rhaarhoff-bunq-chat.herokuapp.com/api/users
http://rhaarhoff-bunq-chat.herokuapp.com/api/messages
``` 
```
 METHOD         URI                             Description
 --------------------------------------------------------------------------------------------------
 GET            users                           Get all users
 POST           users                           Create a user
 PUT            users                           Update a user
 GET            users/:id                       Get user by id
 DELETE         users/:id                       Delete user by id
 GET            users/:id/messages              Get all messages of user
 GET            users/:id/messages/inc          Get all incoming messages of user
 GET            users/:id/messages/inc/:from    Get all incoming messages of user from another user
 GET            users/:id/messages/out          Get all outgoing messages of user
 GET            users/:id/messages/out/:to      Get all outgoing messages of user to another user
 GET            users/:id/messages/all/:user    Get all messages between two users (in order)
 
 GET            messages                        Get all messages
 POST           messages                        Create a message
 PUT            messages                        Update a message
 GET            messages/:id                    Get message by id
 DELETE         messages/:id                    Delete message by id
```

## Testing
A range of test cases has been created. You can view them in the `tests/Feature` folder. If you have installed
the project correctly and ran `composer install` you should be able to run these tests. Simply run the following command to run the tests:
```jshelllanguage
./vendor/bin/phpunit
```

### Collection
A [Postman](https://www.getpostman.com/) collection has been created in order to test the API easily.

Simply import the following to your postman collections:
```jshelllanguage
https://www.getpostman.com/collections/7a7ed1622f99c3f6b122
``` 

Also, import the following environment:
```text
{
    "id": "c43550d0-900b-4466-9077-389a9f08fb60",
    "name": "BUNQ CHAT PRODUCTION",
    "values": [
        {
            "key": "url",
            "value": "https://rhaarhoff-bunq-chat.herokuapp.com/",
            "description": "",
            "enabled": true
        }
    ],
    "_postman_variable_scope": "environment",
    "_postman_exported_at": "2019-04-21T23:44:38.080Z",
    "_postman_exported_using": "Postman/7.0.7"
}
```

After these have been imported, select the correct environment and start testing.
