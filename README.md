# Iris-System-Tasks

Task-1 ( Pack the rails application in a docker container image. )

      Steps;

      1) Create a Dockerfile

      2) Edit the Dockerfile and add the lines shown in screenshot; 
      
      3) Build the Docker image using the following command in the terminal; ( " docker build -t employee-management-app . " )

[ screenshot of Dockerfile ](./images/Dockerfile.png "Dockerfile")

[ screenshot of above command executation ](./images/1.png "screenshot-1")

Task-2 (Launch the application in a docker container. Launch a separate container for the database and ensure that the two containers are able to connect.)

      Steps;

      1) Create a Docker network; ( employee-management-app-network )
      
      2) Create a Docker container for the database; ( create a new container named employee-management-app-db using the postgres:13-alpine image and join it to the employee-management-app-network network.)
      
      3) Create a Docker container for the application; ( create a new container named employee-management-app using the employee-management-app image and join it to the employee-management-app-network network.)
      
      4) expose port 3000 in the container as port 8080 on the host machine and set the DATABASE_URL environment variable to connect to the Postgres database.
      
[ screenshot of step-1 & step-2 ](./images/2.png "screenshot-2")

[ screenshot of step-3 ](./images/3.png "screenshot-3")

Task-3 ( Launch an Nginx container, and configure it as a reverse proxy to the rails application.)

      Steps;
      
      1) Create a Dockerfile for Nginx; (Dockerfile.nginx)
            
            #add :: FROM nginx:latest
                    COPY nginx.conf /etc/nginx/nginx.conf
                    
      2) Create an Nginx configuration file; (nginx.conf)
            
            #add :: worker_processes 1;

                    events { worker_connections 1024; }

                     http {
                         upstream employee-management-app {
                         server employee-management-app:3000;
                         }

                         server {
                             listen 80;
                              server_name localhost;

                             location / {
                                 proxy_pass http://employee-management-app;
                                 proxy_set_header Host $host;
                                 proxy_set_header X-Real-IP $remote_addr;
                                 proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
                                 proxy_set_header X-Forwarded-Proto $scheme;
                              }
                         }
                    }
                    
    NOTE : This configuration file sets up an upstream server pointing to the Rails application container and configures Nginx to act as a reverse proxy.

[ screenshot of step-1,2 ](./images/3.png "screenshot-4")   

Task-4 ( Launch two more containers of the rails application. All three containers should be able to connect to a single database container.)

      Steps;
      
      1) Create three Docker containers for the Rails application; ( Three Docker containers named employee-management-app-1,2,3 and by using the employee-management-app image and joining them to the employee-management-app-network network. They will also set the DATABASE_URL environment variable to connect to the Postgres database.)
      
      2) Build the Docker image for Nginx using the following command in the terminal; ( " docker build -t employee-management-app-nginx -f Dockerfile.nginx . " )
      
      3) Launch the Nginx container; (" docker run --name employee-management-app-nginx --network employee-management-app-network -p 8080:80 -v $(pwd)/nginx.conf:/etc/nginx/nginx.conf -d employee-management-app-nginx ")

[ screenshot of step-1,2 ](./images/3.png "screenshot-5")

[ screenshot of step-3 ](./images/4.png "screenshot-6")

Task-5 ( Enable persistence for the DB data and Nginx config files so that they are available even when the containers go down. )

      Steps;
      
      1) Create a volume for the DB data; ( create a Docker volume named employee-management-app-db-data that will be used to store the DB data. )
      
      2) Modify the DB container to use the volume & launch a new DB container using the volume
      
      NOTE : A new DB container named employee-management-app-db using the postgres image and join it to the employee-management-app-network network. It will also mount the employee-management-app-db-data volume to the container's /var/lib/postgresql/data directory, so that the DB data is persisted even if the container is deleted.
      
      3) Create a volume for the Nginx config files; ( create a Docker volume named employee-management-app-nginx-config that will be used to store the Nginx config files. )
      
      4) Modify the Nginx container to use the volume & launch a new Nginx container using the volume
      
      NOTE : launch a new Nginx container named employee-management-app-nginx using the employee-management-app-nginx image, join it to the employee-management-app-network network, and expose port 8080 on the host machine. It will also mount the employee-management-app-nginx-config volume to the container's /etc/nginx directory, so that the Nginx config files are persisted even if the container is deleted.

[ screenshot of step-1,2 ](./images/4.png "screenshot-7")

[ screenshot of step-3,4 ](./images/5.png "screenshot-8")

Task-6 ( Use docker-compose to easily bring these containers up together with a single command. )

      Steps;
      
      1) Create a docker-compose.yml file which include the below script;
      
      version: '2.3'

      services:
      db:
    image: postgres
    restart: always
    environment:
      POSTGRES_PASSWORD: root
    volumes:
      - employee-management-app-db-data:/var/lib/postgresql/data
    networks:
      employee-management-app-network:

      app:
    build: .
    restart: always
    environment:
      DATABASE_URL: postgres://postgres:root@db:5432/employee_management_app
    depends_on:
      - db
    networks:
      employee-management-app-network:

      nginx:
    image: employee-management-app-nginx
    restart: always
    volumes:
      - employee-management-app-nginx-config:/etc/nginx
    ports:
      - '8080:80'
    depends_on:
      - app
    networks:
      employee-management-app-network:

      volumes:
      employee-management-app-db-data:
      employee-management-app-nginx-config:

      networks:
      employee-management-app-network:
      
    NOTE : This docker-compose.yml file defines three services: db, app, and nginx. The db service uses the postgres image and mounts the employee-management-app-db-data volume to persist the DB data. The app service builds the application image and sets the DATABASE_URL environment variable to connect to the PostgreSQL database. The nginx service uses the employee-management-app-nginx image and mounts the employee-management-app-nginx-config volume to persist the Nginx config files. It also exposes port 8080 on the host machine to access the application.
    
    2) start the containers
    
[ screenshot of step-1,2 ](./images/6.png "screenshot-9")
[ screenshot of build ](./images/8.png "screenshot-10")


      
      
