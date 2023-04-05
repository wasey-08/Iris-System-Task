# Iris-System-Tasks

Task-1 ( Pack the rails application in a docker container image. )

steps;

      1) Create a Dockerfile

      2) Edit the Dockerfile and add the lines shown in screenshot; 
      
      3) Build the Docker image using the following command in the terminal; ( " docker build -t employee-management-app . " )

[ screenshot of Dockerfile ](./images/Dockerfile.png "Dockerfile")

[ screenshot of above command executation ](./images/1.png "screenshot-1")

Task-2 (Launch the application in a docker container. Launch a separate container for the database and ensure that the two containers are able to connect.)

steps;

      1) Create a Docker network; ( employee-management-app-network )
      
      2) Create a Docker container for the database; ( create a new container named employee-management-app-db using the postgres:13-alpine image and join it to the employee-management-app-network network.)
      
      3) Create a Docker container for the application; ( create a new container named employee-management-app using the employee-management-app image and join it to the employee-management-app-network network.)
      
      4) expose port 3000 in the container as port 8080 on the host machine and set the DATABASE_URL environment variable to connect to the Postgres database.
      
[ screenshot of step-1 & step-2 ](./images/2.png "screenshot-2")

[ screenshot of step-3 ](./images/3.png "screenshot-3")
      
      

