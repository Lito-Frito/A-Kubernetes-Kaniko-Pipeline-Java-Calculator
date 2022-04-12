# Kubernetes-Kaniko Pipeline: Java Calulator
<div style="width:60px ; height:60px">
![image](https://user-images.githubusercontent.com/56422761/163040920-4abc1c4f-8d99-4feb-b15b-33e4c1863710.png)
<div>

This is a sample multi-branch pipeline job using Jenkins, Docker, Kubernetes, and Kaniko. The multi-branch pipeline will run various steps and use logic to determine what steps to run depending the branch of the repository it's reading.

Jenkins will first build the calculator program only when one of the branches on GitHub has a pull request. Next, if you're in the Main branch, it runs code coverage tests. Using Jenkins plugins, it publishes those reports on Jenkins. If you're on the Main or Feature branch, it then runs the static code analysis tests. These reports are also published on Jenkins.

If no errors are detecting during the QA phase, the next stage is to build an image of the calculator. This simulates when an app has passed all tests and is ready to be shipped as an updated image. This is done in the Kaniko container. The image name will reflect the branch name of each build (e.g. feature:0.1)  

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
