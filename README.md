Among the many challenges in the modern software development is to ensure that the application performs consistently across all environment.



This is where Docker come to the rescue! It provides a uniform way of building and running containers for any required services. The platform makes sure that your application performs the same regardless of the target environment.

To illustrate the process of Dockerize PHP Application, I will dockerize a blogging website, originally developed in Laravel 5.4.

You might also like: Getting Started With Laravel 5.4

For this, make sure that you have Docker setup on the machine. I will work with docker-compose, that makes it easy to deploy multi-container application by defining them in a single file and executing a simple command.

In the root of the project, create a file and name it docker-compose.yml. The code in the file would look like

![](https://www.cloudways.com/blog/wp-content/uploads/docker-compose.png)

#Docker-compose.yml Explained
In this code, two services with the names php and db are defined these two will connect to run the final application.

‘build’ defines the location of the Dockerfile of the respected service.

‘volume’ mounts the project directory as a volume on the container at /var/www/html. A log directory is also mounted in the respective directory.

The php container exposes the port 8000 of the machine and binds to the port 80 of the container. This will allow access to the application

‘environment’ is used to inject environment variables.

Remember that I could force Docker to build a container before another by specifying the depends on option.  However, in this case, the db container would be built first as the php container depends on it.

Once the docker-compose file has been created, I will next create the respective dockerfiles that   contain all the instructions for building the container images.

Go to the Docker directory and create two folders for php and db containers.

The two dockerfiles would look like:  

#php Dockerfile

![](https://www.cloudways.com/blog/wp-content/uploads/Docker-folders.png)

The file begins with a definition of the base image (in this case, a PHP 5.6 base image). You could easily set up your desired base image depending upon the needs of your application. Next, I copied several files to the container.

You might also like: How To Upgrade From PHP 5.X To PHP 7

#db Dockerfile

![](https://www.cloudways.com/blog/wp-content/uploads/docker-mysql.png)

In this dockerfile, I have used MySQL 5.6 as the base image. When a container is initiated, a new database with the specified name is created. I have also included the database SQL dump file that will be executed as soon as the server is created (because of its location in /docker-entrypoint-initdb.d).

Once all the files have been created, go to the root of the application and run the following command:

`docker-compose up -d`

This command will start the containers in the background. The launch could be slow the first time because all the base images have to be downloaded. However, all subsequent launches are much quicker.

Note that I have also copied the hosts file (with the following text) in the container.

`127.0.0.1 blogsite.com`

Now, you can access the application by visiting http://localhost:8000. As I also copied my hosts file in the container, I can also access the application by visiting the URL, http://blogsite.com.

![](https://www.cloudways.com/blog/wp-content/uploads/docker-welcome-768x381.png)

`dockerized php blog`

You can view your active containers through this command:

`docker ps`

Here is a sample output:

![](https://www.cloudways.com/blog/wp-content/uploads/docker-ps.png)

You can also access the container through

docker exec -it <container_name> bash

See! How simple is to Dockerize PHP application. Due to its light weight, it can be easily destroyed and created again in just a couple of moments.
