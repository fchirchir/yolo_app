# Explanation File
This is an explanation on items appearing in this project and decisions make during its containerization.
## Image Choice
Choice of the base image on which to build each container.
1. Mongo DB
    - Initial choice was `mongo:latest` whose final image surpassed the `400 MB` max limit set for this exercise. I therefore opted for `mongo:4.2.10` which brought down the total image size to `387 MB`.
2. Both Client and Backend

## Dockerfile Directives
Dockerfile directives used in the creation and running of each container.
1. Mongo DB
    - No `dockerfile` was required since the DB container was started directly from Dockerhub's mongo image.
2. Client and Backend containers
    - **FROM node:current-buster-slim** -> Set base image to be modified into desired custom image.
    - **COPY backend/ /path/** -> Copy `backend` folder into the specified path in the image being modified.
    - **WORKDIR /path/** -> Set the working directory for all subsequent commands.
    - **RUN npm install --dd** -> Install node packages required to run the service.
    - **EXPOSE port_number** -> Expose the port number to be used by the service within the container
    - **CMD /bin/bash -c "npm start"** -> Set the entry command when a container is started.

## Networking
All the three containers were attached to the network named `yoloNetwork`. This was to facilitate resolving of containers names to respective container IPs allowing `http` calls to be made using container name instead of IP.
The ports used internally by the services were reused externally when mapping container ports to host ports to eliminate unnecessary confusion. However, container service port can be mapped to any host port as desired.

## Volume definition
MongoDB container is the only one which had a volume attached. This was to ensure that data in teh DB persists even when container is deleted and recreated. There was no good reason to add volumes to the client and backend containers.

## Git workflow used
Since I was a single contributor to the project at the time, I opted for `Centralized Workflow` where all changes were done on my local master branch and later pushed to central master.

## Running the applications
I was able to successfully run the applications using the steps outlined in the  `ReadME` file.

## Good practices
All images built were tagged over `semver conventions` before being pushed to *Dockerhub*. Custom and simple names were assigned to containers and images to ensure clarity and avoid confusion as shown below:
- **MongoDB** -> Container Name: `mongo-db` Image Tag: `mongo:4.2.10`
- **Client** -> Container Name: `client_cnt` Image Tag: `yolo_frontend:v1.0.0`
- **Backend** -> Container Name: `backend_cnt` Image Tag: `yolo_backend:v1.0.0`