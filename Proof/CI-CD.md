# CI/CD
This file containes all the proof for the CI/CD learing outcome.

> You **design and implement**  a (semi)automated software release process that matches the needs of the project context.

> **Design and implement:** You design a release process and implement a continuous integration and deployment solution (using e.g. Gitlab CI and Docker).


## Docker

### why docker?
For our CI/CD we decided to use Docker. First we build our project as a Docker container and then we push the container to Docker Hub. 
We decided to use docker because we didn't want to host our application online (With a service like Azure or AWS), we had some trouble with those services in the past and more importantly they often cost money.


### Github workflows/actions
We used Github workflow files to build and push our projects to Docker Hub. [Here](https://github.com/IPS3-DB04-Teun-Mos-Lukas-Jansen/Dashboard-Front-End/tree/main/.github/workflows) Is an example of how our workflow files look like. As you can see we have one file to build the apllication as a docker container (Which runs on pull requests to test if the application doesn't get build errors), and one file which builds the Docker container and then publishes it to Docker Hub (runs when pushed to main).

![image](https://user-images.githubusercontent.com/81776357/199513242-1f23d4c7-52c0-41ae-9792-b7e91313fb29.png)

We have made a shared Docker account where you can see all our published containers. [*Click here*](https://hub.docker.com/u/teunlukas) to view our account.
