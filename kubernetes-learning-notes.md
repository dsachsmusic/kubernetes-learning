From within app folder, in repo where app lives (if different) app folder, create a docker file (i.e. a file named "Dockerfile" ...no extension(?)). 

Docker file includes:
- "FROM" to specify base image to use. Syntax is FROM <image>:<tag>
- "WORKDIR" sets the working directory for any subsequent COPY, RUN, and CMD commands. Syntax: WORKDIR <path>
- "COPY" files/folders from local machine to path on Docker image
- "RUN" executes a command on the docker image, during build
- "EXPOSE" - specify's port container should listen on 
- "CMD" - command to run when container starts (such as a command to run the app)


Run the following command to build the image:
- docker build -t helloec2rdsipaddress-flask-backend .
- Notes: In this command, the -t is a flag specifying a tag should be added to the image.  The parameter following it is the name of the image.  Regarding the -t flag,t he name can include a colon, and then some string, to specify a specific tag that should be added to the image...if no colon/string is added, then a default tag of "latest is added".  The "." specifies to build the image from the current directory (using the dockerfile there, and, using it as a point of reference for relative paths). 

Add an additional tag, and then push to Dockerhub
- docker tag helloec2rdsipaddress-flask-backend dsachsmusic/helloec2rdsipaddress-flask-backend
- docker push dsachsmusic/my-flask-backend:latest
- Notes: With, the docker build command, earlier, we chose to specify a name. We did not need to.  A docker image is given an image ID when created, and we can name it (and (?) tag it) later, with the "docker tag" command. In order to prepare the image to be pushed to a container registry, we should add a repository name (username), for which, the syntax required is, pre-pending the name with the repository name (username) and a forward slash. Side note: There is additional syntax...prepending the repository name (username) with a registry name, and a forward slash...this can be used if we wish for Docker to push the image to a repository other than the default, of DockerHub. We can add many names/tags, if we wish.  We can also, actually push the image to another respository as well, by adding a name with that respository named in the syntax (though, we need to be able to authenticate to that repository). And, by the way, a registry manages docker images in layers, meaning, that if the image already exists, with the same layers (docker image, working dir, files, run, expose, and cmd parameter values, then, it won't recreate it: the image will be referenced by image ID, and available/tagged to show in, each location.

Create a Kubernetes Deployment and a Kubernetes Service, with YAML
- YAML: Could have two separate YAML files...one for the Deployment and one for the Service
- Deployment: A resource object used to manage the lifecycle of pods and applications running on them. 
  - i.e. - getting the pods running and stood up in the desired state/rolling back if needed, scalaing if needed, etc?
  - Lorem ipsum....need to keep working on understanding the parts of the deployment (
- Aside about Resources: Kubernetes "resources" are entities managed by (via?) the Kubernetes API...including Pods, Services, Deployments, Configmaps, etc.


Running Kubectl
- Sends YAML file to Kubernetes API server (control plane) to "make it so" with regard to desired state in the YAML
- Scheduler determines which node, within the cluster, to run the Pod(s)(i.e. containers)? on, binds the pod(s) to the node, and updates the Kubernetes API server with the assignment
  - Assingment is based on resources available, some rules that can be customized, etc. (?)
  - Term "scheduler" in distributed computing often refers to resource allocation and management...perhaps an extension of the concept of scheduling "jobs" in the case of older task runners that ran scripts sequentially(?)

Kubernetes cluster basics/context
- Nodes are physical or virtual machines where containers are deployed to.
- Pods are smallest kubernetes "object" and they represent a single instance of an application
  - Pods can have one or more containers - these containers will share resoures like storage and network, ip address and port space, lifcycle ...
  - Any containers in pod should contain tightly coupled application components...that must share resources(?)/have no reason not to share resources(?)
- Cluster is a (set of) node(s) and a control plane (its possible to have a single node cluster, as in the case of minikube, or other simple cases.
- Control plane, container runtime, and other parts of a kubernetes cluster
  - Container runtime - runs the containers...provides interface between OS (kernel) and containerized applications (like a hypervisor(?)). Examples include Docker and containerd.
