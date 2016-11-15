# [P5 Server Configuration Project](https://classroom.udacity.com/nanodegrees/nd004/parts/00413454014/project)

This project takes a baseline installation of the Ubuntu Linux distribution on an Amazon EC2 instance and prepares it to host the Catalog Flask application from [project 3](https://github.com/thatkahunaguy/Sports-catalog). This includes installing updates, securing it from a number of attack vectors and installing/configuring web and database servers prior to installing the application.




## Table of contents

* [Quick start](#quick-start)
* [Server Setup](#server-setup)
* [Software Installed](#software-installed)
* [Bugs and feature requests](#bugs-and-feature-requests)
* [Web App Documentation](#documentation)
* [Contributing](#contributing)
* [Community](#community)
* [Versioning](#versioning)
* [Creators](#creators)
* [Copyright and license](#copyright-and-license)


## Quick start

To get started as the grader for this project:

* The server can be accessed at IP address 35.162.118.206 on non-standard ssh port 2200
* For key based authentication on the server, you will need to create the file ~/.ssh/udacity_grader.rsa with the key contents which were placed in the notes of the submission for this project
* Once those steps are complete, the following command will connect to the server: ssh -i ~/.ssh/udacity_grader.rsa grader@35.162.118.206 -p 2200
* The password for user grader was provided in the notes of the submission for this project
* The url for the hosted web application is: http://ec2-35-162-118-206.us-west-2.compute.amazonaws.com 

## Server setup
Note that I found [this guide](https://github.com/stueken/FSND-P5_Linux-Server-Configuration) put together by Norbert Stuken to be extremely helpful in terms of resources.  Unfortunately I didn't find it until I was troubleshooting some issues with my application deployment but I highly recommend this resource for other students.  **Source** for all items was either the online class or this guide unless otherwise noted.  Digital Ocean provided several excellent resources.

* Setup Key Based Authentication: install key provided by Udacity in ~/.ssh/udacity.rsa and make appropriate permission modifications to the file.
* **Additional source for next few steps:** [Digital Ocean](https://www.digitalocean.com/community/tutorials/initial-server-setup-with-ubuntu-12-04)
* Root login & create user grader: create user grader with sudo privleges so that we can disable the root account and use this account for administration. 
* Generate key for user grader & update ssh to nonstandard port 2200: store private key in ~/.ssh/udacity_grader.rsa and add the public key to the server.  Verify login for grader works while still logged into root in another terminal window.  Disable password login on the server.  Verify login for grader is still functional with only key based authentication.  Change ssh port to a non-standard port: in this case 2200.  Verify login for grader is still functional.  Disable root login functionality on the server.  Verify login for grader is still functional...otherwise you'll need to restart :(
* Update & upgrade all packages
* Configure firewall(ufw) to only allow incoming traffic for http(80), ntp(123), and ssh(2200)  **Source:**[Digital Ocean](https://www.digitalocean.com/community/tutorials/how-to-setup-a-firewall-with-ufw-on-an-ubuntu-and-debian-cloud-server)
* Configure local timezone to UTC
* Install & configure Apache2 webserver
* Install & configure PostgreSQL database: create a user on the server(catalog) and then create a matching role on the database with priveleges only to create a database.  Verify no remote connections are allowed. **Source**[Digital Ocean](https://www.digitalocean.com/community/tutorials/how-to-secure-postgresql-on-an-ubuntu-vps)
* Install git & clone & setup web app: Install git, clone the catalog app from project 3.  Install all needed python libraries(sqlalchemy etc, see below for more detail) in a virtual environment. Setup a virtualhost file & wsgi file on the webserver. Update the catalog app to function with PostgreSQL rather than sqlite.  Secure the webserver so that .git directory is not accessible.  Note that the include statements for Seasurf had to be updated as the prior sytax had been deprecated.  The application should be running and accessible except for OAuth2 logings **Sources:**[Digital Ocean](https://www.digitalocean.com/community/tutorials/how-to-deploy-a-flask-application-on-an-ubuntu-vps) and [Kill the Yak](http://killtheyak.com/use-postgresql-with-django-flask/)
* Get the OAuth logins working by updating the whitelists & allowed Oauth and Callback ips and urls 

## Software Installed:
* Apache2 webserver
* PostgreSQL database
* git
* libapache2-mod-wsgi & python-dev for serving python Flask apps
* virtualenv
* Flask
* httplib2
* requests
* flask-seasurf
* oauth2client
* sqlalchemy
* python-psycopg2
* NOTE: this repository should be pulled into the directory /var/www/Catalog on an Apache2 webserver (ie the directory Catalog here with the application files will be /var/www/Catalog/Catalog  Virtual environment files are in /var/www/Catalog/Catalog/catalog

***
## DOCUMENTATION BELOW IS PROVIDED IF NEEDED FOR THE CATALOG APP(P3) DEPLOYED IN THIS PROJECT
## Bugs and feature requests
* Currently player countries and country flags can only be pre-populated in the database as the program doesn't provide a CRUD interface for this.  Simple enough to add but time to move on.
* The program doesn't currently include token renewal after expiration for third party authentication with Google and Facebook.  Users must currently login again after token expiration.

## Documentation

##### Login/Authentication

Users can login with either Google or Facebook accounts.  Information in the database can be viewed without logging in but CRUD actions for categories(sports) and items(players) can only be performed after logging in.

##### Authorization
Only the creator of a category(sport) is authorized to create, edit, or delete items(players) within a category.

##### Categories(Sports)
Sports are the categories in the catalog as currently populated.  Each category can have a name and a photo/icon url associated with it.  In the case of the prepopulated sports these images are from the Rio 2016 Olympics.  On the main page which displays categories, the most recent 3 items(players) added to the database along with their sports are displayed.

##### Items(Players)
Players are the items in the catalog as currently populated.  Each player has fields for a name, category(sport), country, birthdate, photo url, and description/bio.  5 countries are prepopulated in the database(no CRUD interface currently provided) along with their flags and are selected when adding or entering a player.  The players age is displayed based on the birthdate provided.  If the birthdate is not provided, TBD is shown for the age.

##### JSON Endpoints
JSON endpoint are provided for:
* categories(sports): http://localhost:5000/category/JSON
* category items(players): http://localhost:5000/category/#/item/JSON
* individual items(players): http://localhost:5000/category/#/item/#/JSON

## Contributing

This site uses [Bootstrap](http://getbootstrap.com) although in hindsite some simple CSS would have sufficed :)

Images are for demonstration purposes only and are copyright of their respective owners [ATP](http://www.atpworldtour.com), [WTA](http://www.wtatennis.com), [Rio 2016](http://www.rio2016.com), [AVP](http://www.avp.com), & [ESPNFC](http://www.espnfc.com). 

## Community

* Udacity discussions are [here](https://discussions.udacity.com/c/nd004-p3-item-catalog).


## Versioning

Debut release

## Creators

**John Glancy**

* <https://github.com/thatkahunaguy>

**Udacity**
* <https://www.udacity.com/course/full-stack-web-developer-nanodegree--nd004>

## Copyright and license

Code released under [the MIT license](https://opensource.org/licenses/MIT). Docs released under [Creative Commons](http://creativecommons.org/licenses/by/4.0/).
