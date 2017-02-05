# admin-web-app


A react web app built and maintained using the docker eco system. We create 2 docker containers and manage them through docker compose.
One docker container is node.js and helps in building the source using webpack into bundle files ready for serving.
These bundle files are fed to the nginx container which takes care of the static serving

Original author and link for the react app is
Harigopal + Abdul Jaleel
https://github.com/godreams/admin-client


#Structure
Base folder has docker-compose.yml from where we run the compose build. This config file points to the dockerfiles present in the 2 folders.

##admin-web-client
Docker image is inherited from the official node.js boron docker image
	https://hub.docker.com/_/node/
THe dockerfile first creates a  working directory which is where the code will eventually live at /user/apps/admin-client
It also creates a temporary build location. First only the package.json will be copied here and all npm dependencies will be downloaded here first.This is to make use of docker layer caching to speed up build times incase dependencies have not changed.

http://bitjudo.com/blog/2014/03/13/building-efficient-dockerfiles-node-dot-js/
https://gist.github.com/dweinstein/9550188

 Once npm install is completed, then the source is moved to the main location.
 Now webpack is run and the bundle files are generated. The output directory is then shared to a mounted volume /static. This /static is used by the other container to read and serve the static files 	

##admin-nginx
Docker image is inherited from official nginx docker image. Simply copies over the nginx configuration to the container and starts the server at the specified http port. This image uses the mounted volume which has the build files from the other container


#References
http://bitjudo.com/blog/2014/03/13/building-efficient-dockerfiles-node-dot-js/

