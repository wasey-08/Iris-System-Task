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

Task-4 (launch two more containers of the rails application. All three containers should be able to connect to a single database container.)

      Steps;
      
      1) Create three Docker containers for the Rails application; ( Three Docker containers named employee-management-app-1,2,3 and by using the employee-management-app image and joining them to the employee-management-app-network network. They will also set the DATABASE_URL environment variable to connect to the Postgres database.)
      
      2) Build the Docker image for Nginx using the following command in the terminal; ( " docker build -t employee-management-app-nginx -f Dockerfile.nginx . " )
      
      3) Launch the Nginx container; (" docker run --name employee-management-app-nginx --network employee-management-app-network -p 8080:80 -v $(pwd)/nginx.conf:/etc/nginx/nginx.conf -d employee-management-app-nginx ")

[ screenshot of step-1,2 ](./images/3.png "screenshot-5")

[ screenshot of step-3 ](./images/4.png "screenshot-6")
