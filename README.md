A membership database for federated non profits
===============================================

The problem
-----------

For large, mainly volunteer run non profits, managing personal data can be difficult.

This project aims to allows a HQ to securely share the personal data of members with group leaders, while allowing members to understand who is contacting them and why, and to manage those relationships.

Setup
-----

You will need ruby and postgres with postgis installed locally. For ruby I recommend rvm or rbenv, and postgres and postgis using postgres.app on OSX

Clone, `bundle`, `rake db:setup`, `rails server` should get you going.

Alternativley you can use docker, see instructions at the end.


Concept outline
---------------

Admins are able to filter members to their liking. Once they have created a filter, they can create a 'group' for that filter, and assign role holders to that group. Role holders will only be able to see and message members who are within groups they hold a role.

Seeding fake data
-----------------

You can run `rake import_fake` to fill the database with 10,000 fake profiles to test the system out. To create the first admin user, sign up via the application, then run `rails console; u = User.first; u.role = 'admin'; u.save!`.

Nationbuilder Migration
-----------------------

This app can import data from a nationbuilder database dump.

If you follow [these steps](http://nationbuilder.com/ht_open_your_snapshot) precisely to load your dump into your local postgres installation, and change the table_name_prefix in `app/models/nationbuilder.rb` then you should be able to import data by running `rake import`

Testing
-------

I'm currently working on this quite quickly, and more tests could definitely be written.

For now, I am focussing on rspec feature tests, which can be found in spec/features and run by running `rake`

Docker
------

To run the site locally using Docker, you'll need to install both [docker](https://docs.docker.com/engine/installation/) and [docker compose](https://docs.docker.com/compose/install/)

Start by editing the database config file at config/database.yml and set both the development and test environments have the following values set:

    host: db
    username: postgres
    password:

From the git checkout directory run `docker-compose up` to create the containers and to start them.
Once they are running you'll need to run `docker-compose run web rake db:setup` to setup the database.
Now goto http://localhost:3000 to see the site runing, create the first account through the web interface.
You can seed the site with fake data using `docker-compose run web rake import_fake`
To make the first user an admin, run `docker-compose run web rails console`, then enter the following into the console. `u = User.first; u.role = 'admin'; u.save!`
