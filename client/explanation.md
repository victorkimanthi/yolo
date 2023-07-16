a). Choice of the base image on which to build each container.

  I installed  that base image to setup the right environment and tools to create the image.It is  the official node image with version 16 and based on alpine.Alphine is lightweight and hence will have result to a smaller Docker image size.

b). Dockerfile directives used in the creation and running of each container.
  In Dockerfile.client 
  

1. `FROM node:16-alpine`

   This directive sets the base image for the Docker container. It specifies that the container will be built using the official Node.js 16 Alpine image. The Alpine variant is chosen for its lightweight nature, as Alpine Linux is a minimal Linux distribution, resulting in a smaller Docker image size.

2. `WORKDIR /app`

   The `WORKDIR` directive sets the working directory inside the container. It creates the `/app` directory if it does not already exist and sets it as the working directory. All subsequent commands will be executed relative to this directory, making it easier to manage the file paths within the container.

3. `COPY ./client/package.json .`

   This `COPY` directive copies the `package.json` file from the host machine's `./client` directory to the current working directory inside the container (which is `/app` due to the previous `WORKDIR` directive). The `.` at the end of the command refers to the current directory inside the container.

4. `COPY ./client/package-lock.json .`

   Similar to the previous `COPY` directive, this command copies the `package-lock.json` file from the host machine's `./client` directory to the current working directory inside the container.

5. `RUN npm install`

   The `RUN` directive is used to execute commands inside the container during the build process. In this case, it runs `npm install`, which installs the Node.js dependencies listed in the `package.json` file (and recorded in `package-lock.json`). This step is essential to ensure that the required dependencies are installed before copying the rest of the application code.

6. `COPY ./client .`

   This `COPY` directive copies the entire content of the `./client` directory from the host machine to the current working directory inside the container (again, `/app`). This includes all the application code, static assets, and any other files or directories present in the `./client` directory.

7. `CMD ["npm", "start"]`

   The `CMD` directive specifies the default command to be executed when the container starts. In this case, it runs the `npm start` script, which is typically defined in the `package.json` file of the Node.js application. The `npm start` command is commonly used to start the application server or any other required service.

To summarize, this Dockerfile sets up a containerized Node.js environment using the Node.js 16 Alpine image. It copies the `package.json` and `package-lock.json` files to install the Node.js dependencies and then copies the entire application code into the container. Finally, it specifies that the container should execute `npm start` as the default command to start the Node.js application when the container is launched.

 Dockerfile.server

 1. `FROM node:16-alpine`

   This directive sets the base image for the Docker container. It specifies that the container will be built using the official Node.js 16 Alpine image. The Alpine variant is chosen for its lightweight nature, as Alpine Linux is a minimal Linux distribution, resulting in a smaller Docker image size.

2. `WORKDIR /app`

   The `WORKDIR` directive sets the working directory inside the container. It creates the `/app` directory if it does not already exist and sets it as the working directory. All subsequent commands will be executed relative to this directory, making it easier to manage the file paths within the container.

3. `COPY ./backend/package.json .`

   This `COPY` directive copies the `package.json` file from the host machine's `./client` directory to the current working directory inside the container (which is `/app` due to the previous `WORKDIR` directive). The `.` at the end of the command refers to the current directory inside the container.

4. `COPY ./backend/package-lock.json .`

   Similar to the previous `COPY` directive, this command copies the `package-lock.json` file from the host machine's `./client` directory to the current working directory inside the container.

5. `RUN npm install`

   The `RUN` directive is used to execute commands inside the container during the build process. In this case, it runs `npm install`, which installs the Node.js dependencies listed in the `package.json` file (and recorded in `package-lock.json`). This step is essential to ensure that the required dependencies are installed before copying the rest of the application code.

6. `COPY ./backend .`

   This `COPY` directive copies the entire content of the `./client` directory from the host machine to the current working directory inside the container (again, `/app`). This includes all the application code, static assets, and any other files or directories present in the `./client` directory.

7. `CMD ["npm", "start"]`

   The `CMD` directive specifies the default command to be executed when the container starts. In this case, it runs the `npm start` script, which is typically defined in the `package.json` file of the Node.js application. The `npm start` command is commonly used to start the application server or any other required service.

To summarize, this Dockerfile sets up a containerized Node.js environment using the Node.js 16 Alpine image. It copies the `package.json` and `package-lock.json` files to install the Node.js dependencies and then copies the entire application code into the container. Finally, it specifies that the container should execute `npm start` as the default command to start the Node.js application when the container is launched.


 c). Application port allocation

   1.Client service
    The client service is defined with the build section, which specifies how to build the Docker image for the client application. 
    It will use the Dockerfile.client located in the current directory (context: .).
    The ports section maps port 3000 of the host to port 3000 of the container.
    This allows the client application to be accessible from the host machine on port 3000.
    The depends_on section indicates that the client service depends on the server service. 
    This means that the server service will start before the client service.

    2.Server service
    The server service is defined with the build section, which specifies how to build the Docker image for the server application.
    It will use the Dockerfile.server located in the current directory (context: .).
    The ports section maps port 4000 of the host to port 4000 of the container.
    This allows the server application to be accessible from the host machine on port 4000.
    The depends_on section indicates that the server service depends on the database service. 
    This means that the database service will start before the server service.

    3.Database service
    The database service is defined with the image directive, which means it uses the official MongoDB image (mongo) from Docker Hub to create the container. No build is required for this service as it uses a pre-built image.
    The ports section maps port 27017 of the host to port 27017 of the container. 
    This allows external applications (e.g., MongoDB clients) to connect to the MongoDB database running inside the container on port 27017.

    To summarize, the Docker Compose file defines three services: client, server, and database. The client and server services are built from custom Dockerfiles, and they expose ports 3000 and 4000, respectively, to the host. The database service uses the official MongoDB image, and port 27017 is exposed to allow external access to the MongoDB database. The services are orchestrated using Docker Compose, ensuring proper dependency management and communication between the containers.

    d).Docker-compose volume 
      The volumes section creates a named volume called mongodb_data and mounts it to the container's /data/db directory. This allows the MongoDB data to be persisted even if the container is stopped or removed.

    e).Git workflow
    1.Git clone
       I started by cloning the repository into my machine.
       git clone https://github.com/victorkimanthi/yolo.git

    2.Implement Docker Compose Files:

   -Create the necessary Dockerfiles (Dockerfile.client and Dockerfile.server) and the Docker Compose file (docker-compose.yml) in the project root.

    3.Commit Changes:

     Once you've implemented the Docker Compose files, stage and commit the changes to your remote repository.

       git add Dockerfile.client Dockerfile.server docker-compose.yml
       git commit -m "Add Docker Compose files for client and server"

     4.Push to remote repo
     After commiting the changes, push the project to the remote repository.   

     git push 
