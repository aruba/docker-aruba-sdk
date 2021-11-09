# docker-aruba-sdk
Docker Files for arubahpe/aruba-sdk docker image

To build an image, follow the below mentioned steps:

1. Run the ansible script in `/playbooks` dir. The outcome of the script should create a `samples` dir inside `/docker_files/aruba_docker` path. The `/docker_files/aruba_docker/samples` dir should contain sample scripts for *pycentral* and *ansible_aos_role*. 
2. Copy the `/docker_files/aruba_docker/samples` into `/docker_files/aruba_jenkins_docker/samples`
3. Execute *docker build* command to build images.
  ```bash
        cd /docker_files/aruba_docker
        sudo docker build -t arubahpe/aruba-sdk .
        
        cd /docker_files/aruba_jenkins_docker
        sudo docker build -t arubahpe/aruba-sdk:jenkins-agent .
  ```
4. `docker images` command should show two images with name `arubahpe/aruba-sdk` with *latest* tab and *jenkins-agent*

### Usage
**arubahpe/aruba-sdk:latest**

Test/Validate the image by running a [sample script](https://github.com/aruba/pycentral/blob/master/sample_scripts/pycentral_base_sample.py). 
```bash
sudo docker run -it --rm -v <local-path-to-script>:/aruba/pycentral_base_sample.py arubahpe/aruba-sdk:latest python3 pycentral_base_sample.py
```

**arubahpe/aruba-sdk:jenkins-agent**

Following is an example of [docker-compose.yaml](/docker_files/aruba_jenkins_docker/docker-compose.yaml) to deploy aruba sdk container as a Jenkins node. Provide **Jenkins Host URL, Agent name and secret** obtained by creating a node in Jenkins host `Manage Jenkins -> Manage Nodes and Cloud`. 

Optional: 
- Configure the Network for this node in-order to reach the Jenkins Host. In local test environment, Jenkins Host and this node could be part of same docker network.
- Volume Mount configuration is optional as well. If it's not provided Jenkins would create a default local volume mount. 
