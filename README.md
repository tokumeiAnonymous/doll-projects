### To run the jenkins container

Replace the directory with your own desired directory

Note: Make sure that your docker service is running
```
docker run --name doll-jenkins --detach -p 8080:8080 -p 50000:50000 --restart=on-failure -v jenkins_home:/var/jenkins_home jenkins/jenkins 
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

### Questions
- What are the different use cases for ssh key, gpg key, and deploy key as credentials?
- Is there a way to access the volumes content in your machine without using a container?
  - For example without mounting the volume to a container and then exec inside the container to see the content.
  - I vaguely remember that I could save it to a folder last time? or Am I remembering 
it wrong?

### References
- [Installing Jenkins on Docker](https://www.jenkins.io/doc/book/installing/docker/)
  - You can also refer to this. However it was overwhelming to me so I didn't follow it.