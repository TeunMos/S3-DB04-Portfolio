
# CI/CD
This file containes all the proof for the CI/CD learing outcome.

> You **design and implement**  a (semi)automated software release process that matches the needs of the project context.

> **Design and implement:** You design a release process and implement a continuous integration and deployment solution (using e.g. Gitlab CI and Docker).


## Docker

### why docker?
For our CI/CD we decided to use Docker. We decided Docker because it makes it really easy for users to deploy our application, and because we wanted to learn how it works. First we build our project as a Docker container and then we push the container to Docker Hub. 
We decided to use docker because we didn't want to host our application online (With a service like Azure or AWS), we had some trouble with those services in the past and more importantly they often cost money.


### Github workflows/actions
We used Github workflow files to build and push our projects to Docker Hub. [Here](https://github.com/IPS3-DB04-Teun-Mos-Lukas-Jansen/Dashboard-Front-End/tree/main/.github/workflows) Is an example of how our workflow files look like. As you can see we have one file to build the apllication as a docker container (Which runs on pull requests to test if the application doesn't get build errors), and one file which builds the Docker container and then publishes it to Docker Hub (runs when pushed to main).

![image](https://user-images.githubusercontent.com/81776357/199513242-1f23d4c7-52c0-41ae-9792-b7e91313fb29.png)

We have made a shared Docker account where you can see all our published containers. [*Click here*](https://hub.docker.com/u/teunlukas) to view our account.

### Docker compose

We've also made a Docker compose file to make it easy to run our project. the compose file pulls the Docker images from Docker Hub and sets up the containers as they should be set-up with the correct ports. 

This is how the Docker compose file looks like *(as of 7-12-2022)*:
``` yaml
name: dashboard
services:
 
  dashboard-frontend:
    container_name: frontend
    restart: unless-stopped
    image: teunlukas/dashboard-frontend:main
    ports:
      - 3000:3000
    env_file:
    - ./.env
    environment:
        REACT_APP_REDIRECT_URI: "http://localhost:3000"
        REACT_APP_USER_PREFRERENCES_URL: "http://localhost:8000"
        REACT_APP_INTEGRATIONS_URL: "https://localhost:8001"
        REACT_APP_INTEGRATIONS_URL_INSECURE: "http://localhost:81"
    
  user-preferences-db:
    container_name: user-preferences-db
    restart: unless-stopped
    image: mongo
    ports:
        - 27017:27017
        
  user-preferences-api:
    container_name: user-preferences-api
    restart: unless-stopped
    image: teunlukas/dashboard-user-preferences-api:main
    ports:
        - 8000:8000
    env_file:
    - ./.env
    links:
    - user-preferences-db
    environment:
        USER_PREF_DB_URL: "mongodb://user-preferences-db:27017/"

  integration-db:
    container_name: integration-db
    restart: unless-stopped
    image: mongo
    ports:
        - 27016:27017
        
  integrations-api:
    container_name: integrations-api
    restart: unless-stopped
    image: teunlukas/dashboard-integration-api:main
    ports:
        - 8001:443
        - 81:80
    links:
    - integration-db
    env_file:
    - ./.env
    environment:
        INTEGRATION_DB_URL: "mongodb://integration-db:27017"
```
> this info may be out of date, to ensure you have the latest information [_click here!_](https://github.com/IPS3-DB04-Teun-Mos-Lukas-Jansen#running-the-project)

the compose file also needs a .env file for the enviorment variables:
```
REACT_APP_CLIENT_ID = ...
REACT_APP_CLIENT_SECRET = ...
OPENWEATHERMAP_APIKEY = ...
```
> this info may be out of date, to ensure you have the latest information [_click here!_](https://github.com/IPS3-DB04-Teun-Mos-Lukas-Jansen#running-the-project)




This is how The containers from the compose file look in Docker Desktop:

![image](https://user-images.githubusercontent.com/81776357/199514315-eb309925-0de8-4055-a336-ac79280d5060.png)

## Deployment
We've made a research document about deploying our application which you can view [here!](https://docs.google.com/document/d/12H3scYrzKteGmO81OCrcXmpe4WQdXSHRZ8Nco2Ydc54/edit?usp=sharing)
