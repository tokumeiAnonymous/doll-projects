### To run the jenkins container

Note: Make sure that your docker service is running \
This is the initial command I was running 
```
docker run --name doll-jenkins --detach -p 8080:8080 -p 50000:50000 --restart=on-failure -v jenkins_home:/var/jenkins_home jenkins/jenkins 
```
This is the commands I've used to be able to use docker on jenkins
This is also available from the jenkins official docs
```
docker run --name jenkins-docker --rm --detach   --privileged --network jenkins --network-alias docker   --env DOCKER_TLS_CERTDIR=/certs   --volume jenkins-docker-certs:/certs/client   --volume jenkins_home:/var/jenkins_home   --publish 2376:2376   docker:dind
docker run --name jenkins-myjenkins --restart=on-failure --detach   --network jenkins --env DOCKER_HOST=tcp://docker:2376   --env DOCKER_CERT_PATH=/certs/client --env DOCKER_TLS_VERIFY=1   --publish 8080:8080 --publish 50000:50000   --volume jenkins_home:/var/jenkins_home   --volume jenkins-docker-certs:/certs/client:ro   myjenkins:2.401.2-1
```

### To stop the jenkins container

```
docker ps # Copy the name or id for the jenkins container
docker stop container_name|container_id
```


### To delete the docker container
```
docker container rm container_name|container_id
```

### To replicate the set up
- Go to localhost:8080 
- Login using the password indicated in the shell output
- It can also be accessed by initiating a shell to the jenkins container. It will be saved in the file `/var/jenkins_home/secrets/initialAdminPassword`
- Install the recommended plugins
- Set up an account
- Add credentials to be able to access your github repo. You can choose from ssh key, gpg key, and deploy key to let jenkins access your repo.
- If you are encountering an issue with your github credentials go to 
  - Dashboard > Manage Jenkins > Security > Git Host Key Verification Configuration > No verification
  - You can also see this instructions in the failed build


### Creating the pipeline build for your application
- Fork the repo [simple-java-maven-app](https://github.com/jenkins-docs/simple-java-maven-app)
- Create a Pipeline
- Proceed with the configuration
- Select `Pipeline script from SCM` and paste the url of the forked repo
- Used your created github credentials. If you haven't created yet you can create here as well
- Make sure to point the Script Path to your `Jenkinsfile`
- Then save
- Test the build
- If you encounter any error it is worth checking 
out the logs or console output to debug

### Questions
- What are the different use cases for ssh key, gpg key, and deploy key as credentials?
- Is there a way to access the volumes content in your machine without using a container?
  - For example without mounting the volume to a container and then exec inside the container to see the content.
  - I vaguely remember that I could save it to a folder last time? or Am I remembering 
it wrong?
- What is the usual setup for docker in jenkins? Using the docker on host or Installing docker on the jenkins image


### Future expansion
- Create your own pipeline using build tools you are already familiar
- Experiment with building a nodejs app or python app

### References
- [Installing Jenkins on Docker](https://www.jenkins.io/doc/book/installing/docker/)
  - You can also refer to this. However it was overwhelming to me so I didn't follow it.
  - As I move forward I've picked up some of the things from this reference. The one where you enable running docker command in the jenkins image
  - I've also ended up following the params regarding certs. Since from my build it indicated https error. Even though I didn't understand it quite fully
- [docker on docker](http://jpetazzo.github.io/2015/09/03/do-not-use-docker-in-docker-for-ci/)