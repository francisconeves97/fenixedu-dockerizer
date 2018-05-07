# fenixedu-dockerizer

Create a simple webapp based on [http://start.fenixedu.org](start.fenixedu.org)
for your framework projects.

Example docker-compose that creates a linked db container and allows for
a [http://github.com/jwilder/nginx-proxy](jwilder/nginx-proxy):
```
version: '2'
services:
    fenixedu:
        image: fenixedu
        container_name: fenixedu-webapp
        environment:
            - PROJECT_NAME=fenixedu
            - PROJECT_VERSION=1.0.0-SNAPSHOT
            - BENNU_VERSION=5.0.2
            - DB_HOST=fenixedu-db
            - DB_PORT=3306
            - DB_DATABASE=fenixedu-db
            - DB_USER=root
            - DB_PASS=pass
            - CONTEXT_PATH=/
            - VIRTUAL_HOST=localhost
        volumes:
            - ./configs:/cfg
        ports:
            - 8080:8080
        expose: 
            - 8080
        depends_on:
            - fenixedu-db
    fenixedu-db:
        image: mysql:5.7.22
        container_name: fenixedu-db
        environment:
            - MYSQL_ROOT_PASSWORD=pass
            - MYSQL_DATABASE=fenixedu-db
```
Note that you can also use your own mysql server elsewhere

The volume ```./configs:/cfg`` holds the following files
 - settings.xml 
 - configuration.properties
 - dependencies.xml
 - repositories.xml

The settings.xml file will be placed in the ~/.m2 folder
