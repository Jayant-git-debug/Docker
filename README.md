# Docker
** We need to first create the ec2 instance and install the Docker on that EC2 machine once install we need to provide the user in the user group were we have freely use the docker without using of the docker command, below are the command:**

sudo usermod -a -G docker ec2-user

## Check user was added or not:
sudo cat /etc/group
docker:x:992:ec2-user


**Some are commands important for the Docker:**
docker images ls –a >> Docker images View

docker container >>View all container

docker system –help >>to check the Systeam information

docker image pull hello-world  >> pulling image

docker container run hello-world >> create the container and run that

-a, --attach list                      Attach to STDIN, STDOUT or STDERR

-d, --detach                     Run container in background and print container ID

-e, --env list                         Set environment variables 

-i, --interactive                      Keep STDIN open even if not attached

-m, --name string                      Assign a name to the container

-p, --publish list                     Publish a container's port(s) to the host

-t, --tty                              Allocate a pseudo-TTY


### Command for generating the container:
docker image pull httpd

docker container run --name websrever -d -i -t -p 7000:80 httpd

docker container stop webserver  >> Stop the container 

docker container start webserver >> Start the container

docker container inspect webserver >> for detail for the image and container

docker container exec -it websrever /bin/bash >> execute the container

Path for changes >>> cd htdocs/


## How to create the images and run as containerization?
-First you need to login the EC2 instance where you have install Docker:

-Now Creating the directory in the Docker server 

-Under the docker we can create the file name image, where we clone the git resposrity of the python pogram

-While creating the image we require the one text file which is help us to execute the command

-so creating the text file inside same path “Dockerfile”

![image](https://github.com/user-attachments/assets/7e27f5f6-b213-4dec-ab09-e532e612c91d)

