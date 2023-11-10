# Kubernetes-Kaniko Pipeline: Java Calculator
![image](https://user-images.githubusercontent.com/56422761/163040920-4abc1c4f-8d99-4feb-b15b-33e4c1863710.png)

In one of my grad classes, we learned about the concept of running Docker inside of Docker. I made this project in order to help the clients I was working with back then to understand different DevOps concepts. I'd often cover what CI/CD means, some of the core technologies around them (e.g. Docker, Kubernetes, Jenkins), and why so many business have adopted this approach to development. To that end, I made this project as a visual aid for some of these concepts.

This is a sample multi-branch pipeline job; it uses a Docker version of Jenkins, Docker, Kubernetes, and Kaniko. I've configured Jenkins to build the calculator program only when one of the branches on GitHub has a pull request. The multi-branch pipeline uses logic to run different steps, depending what branch is being used. It will run appropriate QA tests for any production-related branches. It will also run Docker (while inside of Docker) to build an image of the Java calculator program the pipeline yielded. Finally, it will tag the image depending on the branch used and push it to Docker Hub for public use. 

## What This Includes
`Jenkinsfile`: This includes all the instructions Jenkins will follow to create the calculator, test it, and then build an image from it.

`pod.yaml` : This is the Kaniko container that I used to build an image of the Java calculator at the end of my pipeline. It was how I ran Docker-in-Docker.

The rest of the files and folders are the necessary components to get the calculator program running and tested (or to get them running in Kubernetes).

# Getting Started

## Requirements
* Jenkins
* Kubernetes (I used Minikube)
* A Docker Hub Account (free)

## Quick Start
You can go to [Docker Hub](https://hub.docker.com/repository/docker/crc8109/calculator) to see where I'm hosting the image from the pipeline I built. Here's the link for the [feature](https://hub.docker.com/repository/docker/crc8109/calculator-feature) version. These are the direct result of the pipeline I created using the above files.

## Starting from Scratch
Clone the repo. Then on your Jenkins instance, setup a multi-branch pipeline project. Use your repo's own URL and Jenkinsfile. Configure your own credentials so GitHub can trigger builds in Jenkins. Don't forget to supply the Kaniko container with your own secret too so you can push the image you build to your own Docker Hub account.
