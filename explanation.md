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





     //**************************************************************************************************
     
     
 #IP3 EXPLANATION

The provided playbook configures an e-commerce website on a target VM (ecommerceserver) using Ansible. 

##Order of Execution:

1.The playbook begins with the name "Configuring ecommerce website on vm" and targets the ecommerceserver host.
2.The playbook runs with elevated privileges (become: true) to handle tasks that require root/sudo access.
Variables (repository_url and vm_clone_path) are defined for use throughout the playbook.
The tasks outside the roles section are executed first, followed by the roles.
Tasks Outside Roles:

3.Create directory: This task uses the file module to create a directory at the specified vm_clone_path. It ensures that the directory exists with the given permissions (mode: 0755) to clone the repository.

4.Clone the GitHub repository: The git module is used to clone the e-commerce website's repository from the repository_url to the vm_clone_path. This sets up the project's codebase for further processing.

5.Update apt package index: The apt module updates the package index on the target VM to ensure the package information is up to date.

6.Install required packages for Docker: This task uses the apt module to install Docker (docker.io) on the target VM.

7.Install Docker Compose: The apt module is used again to install Docker Compose on the VM. Docker Compose is required to manage multi-container Docker applications.

8.Start and enable Docker service: The service module ensures that the Docker service is started and enabled, ensuring Docker is ready to run containers.

9.Roles:

The roles are defined under the roles section and include client_container and server_container.

9.1 Role client_container:

name: The name of the task. In this case, it is "Build frontend container image" for clarity.

docker_image: This is an Ansible module used to build Docker images. It takes various parameters to define the image's name, source directory (where the Dockerfile is located), and the desired state of the image.

name: The name of the Docker image to build. In this case, it is specified as "client_image". This name is used as the tag for the image when it is built.

path: The path to the source directory containing the Dockerfile used to build the image.The Dockerfile for the frontend client container is located in the /roles/client_container/files directory. This is where Ansible will look for the Dockerfile to build the image.

state: The desired state of the Docker image. In this case, it is specified as "present", which means Ansible will ensure that the Docker image exists and is up to date. If the image doesn't exist or is outdated, Ansible will build it based on the Dockerfile.

become: The become: yes parameter allows the task to be executed with elevated privileges (usually sudo or root access). 

Ansible will look for the Dockerfile in the specified path (/roles/client_container/files), and if it finds it or if it needs to be updated, it will build the Docker image with the name "client_image".

9.2 Role server_container:

name: The name of the task. In this case, it is "Build backend container image" for clarity.

docker_image: This is a module in Ansible used to build Docker images. It takes various parameters to define the image's name, source directory (where the Dockerfile is located), and the desired state of the image.

name: The name of the Docker image to build. In this case, it's specified as "server_image". This name is used as the tag for the image when it is built.

path: The path to the source directory containing the Dockerfile used to build the image.The Dockerfile for the server container is located in the /roles/server_container/files directory. This is where Ansible will look for the Dockerfile to build the image.

state: The desired state of the Docker image. In this case, it is specified as "present", which means Ansible will ensure that the Docker image exists and is up to date. If the image doesn't exist or is outdated, Ansible will build it based on the Dockerfile.

become: The become: yes parameter allows the task to be executed with elevated privileges (usually sudo or root access). This might be necessary for certain operations, such as managing Docker on the target machine.

With this task, Ansible will look for the Dockerfile in the specified path (/roles/server_container/files), and if it finds it or if it needs to be updated, it will build the Docker image with the name "server_image".

10. Start Docker Compose services: Finally, the playbook runs the docker-compose up command to start the configured Docker containers using the docker-compose.yml file located at the specified vm_clone_path. This is the step where the frontend and backend containers work together to run the e-commerce website as a whole.

 
  //*******************************************************
    IP 4 Orchestration

i) Activate the gcloud shell 
ii)git clone the the project 
      git clone https://github.com/victorkimanthi/yolo.git

iii)Build the images for creating containers
  -  Run the command below to build the client image
    docker build -t gcr.io/ip4gke/yoloclient_image:v1.0.0 -f Dockerfile.client .

  - Run the command below to build the server image
    docker build -t gcr.io/ip4gke/yoloserver_image:v1.0.0 -f Dockerfile.server .

  - To build mongodb image
    docker build -t gcr.io/ip4gke/mongodb_image:v1.0.0 -f Dockerfile.mongodb .   

   iv)Pushing the docker images to google container registry 

     - pushing server image
      docker push gcr.io/ip4gke/yoloserver_image:v1.0.0

      - pushing client image
      docker push gcr.io/ip4gke/yoloclient_image:v1.0.0

      - pushing mongodb image
      docker push gcr.io/ip4gke/yoloserver_image:v1.0.0

  v)Created a manifests folder and added kubernetes manifest files for the images in the container registry.

vi)Create a kubernetes cluster  in GKE.

vii)Connect to the cluster
    
     gcloud container clusters get-credentials autopilot-cluster-1 --zone us-central1

 viii) Use the kubectl apply -f command to apply each manifest file to your Kubernetes cluster  
      
      kubectl apply -f client-deployment.yaml
   




