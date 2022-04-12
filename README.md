# Kubernetes-Kaniko Pipeline: Sample Java Calulator
![image](https://user-images.githubusercontent.com/56422761/163040920-4abc1c4f-8d99-4feb-b15b-33e4c1863710.png)

This is a sample multibranch pipeline job using Jenkins, Docker, Kubernetes, and Kaniko. The multibranch pipeline will run various steps and use logic to determine what steps to run depending the branch of the repository it's reading. Regardless of branch though, first, it builds the calculator program. Jenkins was configured to build only when one of the branches on Github had a pull request.

Next, if you're in the Main branch, it runs code coverage tests. Using Jenkins plugins, it publishes those reports on Jenkins too. If you're on the Main or Feature branch, it then runs the static code analysis tests. These reports are also published on Jenkins too.

If no errors are detecting during the QA phase of the pipeline, then the next stage is to build an image of the calculator. This simulates when an app has passed all tests and is ready to be shipped as an updated image. This is done in the Kaniko container. Your image name will reflect the branch name you're in (e.g. feature:0.1)  

Jenkins was configured so that Github

## What This Includes
`main.py`: The example script to showcase XOR encryption. Run this to see the XOR encryption demo.
`encrypt.py` & `decrypt.py`: Supplementary files that feed into `main.py`. No need to touch these.

# Getting Started

## Requirements
* Python3
* a CLI (if you're on Windows, this is the Command Prompt; for Linux and Mac, this is your terminal)

## Quick Start
You can go to [repl.it](https://repl.it/@crc8109/XOR-Encryption#.replit) where I'm hosting the app in a personal repl. When you click the link, just hit the button up top that says `Run` with the forward arrow and the app will start up.

## Starting from Scratch
Download the file `main.py` and then open the CLI from the folder that has the `main.py` file. Run the file by typing `python3 main.py`.

If this doesn't work, feel free to reach out!
