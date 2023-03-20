# Week 1 — App Containerization

## Tasks 
1. Run Dockerfile CMD as an external script [DONE]
2. Push and tag a image to DockerHub 
3. Use multi-stage building for a Docker build
4. Implement a healthcheck in the V3 Docker compose file
5. Install Docker on the local machine and get the same containers running outside Gitpod / Codespaces [DONE]
6. Launch an EC2 instance that has Docker installed, and pull a container to run your own Docker processes 

## Task 1: Running Dockerfile CMD as an external script

In a Dockerfile, the 'CMD' instruction is used to specify the command that should be run when a container is started from the image.  
    
    ```
        CMD ["python3", "-m", "flask", "run", "--host=0.0.0.0", "--port=4567"]
    ```
 <br /> By default, the command will run the Python3 interpreter and execute flask inside the container's filesystem. <br />

 To run the 'CMD' as an external script, it is required to create a Bash/Py file and specify the script's path to the filename instead of the abovementioned command. <br />

   1. Create a Bash script, named ```'script.sh'``` for the backend-flask
        ```
            #!/bin/bash

            python3 -m flask run --host=0.0.0.0 --port=4567
        ```
   2. Modify the CMD command in the Dockerfile
        ```
                CMD ["./script.sh"]
        ```
   3. Build a Docker image with versioning control
        ```
        docker build -t backend-flask:1.0 .
        ```
   4. Run the Docker 
        ``` 
            docker run -p 4567:4567 backend-flask:1.0
        ```
## Task 3: Use multi-stage building for a Docker build

Multi-stage build to remove build dependencies for backend-flask application <br />

   Before:

    ``` 
        FROM python:3.10-slim-buster
        WORKDIR /backend-flask
        COPY requirements.txt requirements.txt
        RUN pip3 install -r requirements.txt
        COPY . .
        ENV FLASK_ENV=development
        CMD ["./script.sh"]
    ```

   After:

    ``` 
        # Multi-Stage Builds
        # Stage 1: Build
        FROM python:3.10-slim-buster AS build
        WORKDIR /backend-flask
        COPY requirements.txt requirements.txt
        RUN pip3 install -r requirements.txt
        #COPY ..

        # Stage 2: Run
        FROM python:3.10-slim-buster AS run
        WORKDIR /backend-flask
        COPY --from=build /backend-flask /backend-flask
        ENV FLASK_ENV=development
        EXPOSE ${PORT}
        CMD ["bash", "./script.sh"]
    ```
    
Build and Run the Dockerfile following the abovementioned commands.

