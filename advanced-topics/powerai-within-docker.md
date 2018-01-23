# PowerAI Through Docker Containers Inside IBM Spectrum LSF

The overall process is made of the following steps:
*	Install Docker.
*	Install NVIDIA Docker plugin.
*	Create base NVIDIA Docker images.
* Create PowerAI Docker image.

## Components Overview

| Component        | Version    |
|:----------------:|:----------:|
| CUDA Toolkit     | 8.0.61-1   |
| CuDNN            | 6.0.21-1   |
| Docker           | 17.06.1-ce |
| IBM Spectrum LSF | 10.1.0.1   |
| NVIDIA Docker    | 1.0.1      |
| NVIDIA Drivers   | 375.51-1   |
| PowerAI          | 4.0.0      |

> The NVIDIA Docker plugin requires a system featuring:
*	NVIDIA driver version 361.93.03 or higher.
*	CUDA version 8.0.44 or higher.

## Install & Configure Docker

The Docker installation is performed through the following procedure:

* Setup the Docker repository from Unicamp:
```
[unicamp]
baseurl=http://ftp.unicamp.br/pub/ppc64el/rhel/7/docker-ppc64el/
enabled=1
gpgcheck=0
name=Unicamp
```

*	Install the Docker package from the Docker repository:
```
$ yum install docker-ce
```

*	Create the default configuration file for the Docker daemon the following content.
```
/etc/docker/daemon.json
```
  * Declare the local Docker registry (to be created):
```
{
        "insecure-registries": ["registry:5000"]
}
```

*	Validate the Docker installation:

  *	Check that the Docker deamon is running by issuing the following command:
```
$ systemctl status docker
```

  *	If not already running, start the Docker daemon:
```
$ systemctl start docker
```

  *	Open a command-line terminal and run some basic Docker commands:

    *	Display various information related to the host system:
```
$ docker version
```

    *	Search for available ppc64le images on the official Docker public registry:
```
$ docker search ppc64le
```

    *	Try running a Ubuntu container on your RHEL host by running the following command:
```
$ docker run --rm -it ppc64le/ubuntu /bin/bash
root@6bfb0623bce7:/# cat /etc/lsb-release
DISTRIB_ID=Ubuntu
DISTRIB_RELEASE=16.04
DISTRIB_CODENAME=xenial
DISTRIB_DESCRIPTION="Ubuntu 16.04.1 LTS"
```

## Install & Configure NVIDIA Docker

In order to exploit the host GPUs from the docker image, the NVIDIA Docker plugin needs to be installed through the following additional steps:

*	Get latest NVIDIA Docker source package for ppc64le architecture from GitHub:
```
$ wget https://github.com/NVIDIA/nvidia-docker/archive/ppc64le.zip
```

*	Extract source package:
```
$ unzip ppc64le.zip
```

*	Build NVIDIA Docker:
```
$ cd nvidia-docker-ppc64le
$ make
$ make install
```

*	At the end of the build process, the following two binaries have been generated inside the `tools/bin` subdirectory:
  * `nvidia-docker`
  * `nvidia-docker-plugin`

> The build process is performed inside a Docker image that is retrieved from the Docker public registry. An access to this remote registry is therefore required.

*	Check the NVIDIA Docker version:
```
$ bin/nvidia-docker-plugin -v
NVIDIA Docker plugin: 1.0.1
```

*	Create the NVIDIA Docker service by adapting the following configuration file:
```
$ vi /usr/lib/systemd/system/nvidia-docker.service
```
{% gist id="geoffrey-pascal/a25903aee7642082de09edf6b40cee61" %}{% endgist %}


*	Start the NVIDIA Docker service:
```
$ systemctl daemon-reload
$ systemctl start nvidia-docker
```

## Install & Configure Docker Registry

*	Install the Docker Registry package from the Docker repository:
```
$ yum install docker-distribution
```

*	Adjust the default configuration by modifying the following configuration file:
```
/etc/docker-distribution/registry/config.yml
```

  *	Specify the custom location of Docker images storage:
```
/install/custom/registry
```

*	Start the private registry:
```
$ systemctl start docker-distribution
```

## Generate CUDA + cuDNN Images

Once the NVIDIA Docker has been enabled, a first baseline of Docker images needs to be created:
*	CUDA Image: Operating System + CUDA Toolkit.
*	cuDNN Image: Operating System + CUDA Toolkit + cuDNN Library.

### CUDA Image

The process for generating CUDA images is the following:

* Create the following Dockerfile:
{% gist id="nicolas-tallet/bdd814838fa66b18fc9053f7d8747b5d" %}{% endgist %}

* Launch image generation using the above Dockerfile:
```
$ docker build -f Dockerfile.cuda-devel-8.0 -t cuda/devel:8.0.61-1 .
```
> A remote access to both the Docker public registry and the CUDA repository is required.

* Check image existence:
```
$ docker images cuda/devel
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
cuda/devel          8.0.61-1            3f2ae272ee0f        8 weeks ago         1.65 GB
```

### cuDNN Image

The process for generating cuDNN images is the following:

*	Download the NVIDIA cuDNN 6.1 Developer Library Debian packages (Development):
  * https://developer.nvidia.com/rdp/cudnn-download#a-collapse6-8
> A registration to NVIDIA's Accelerated Computing Developer Program is required.

* Create the following Dockerfile:
{% gist id="nicolas-tallet/6dfcce9eeb390a9795239b59b86f11db" %}{% endgist %}

* Launch image generation using the above Dockerfile:
```
$ docker build -f Dockerfile.cudnn-devel-6 -t cudnn/devel:6.0.21-1 .
```

* Check image existence:
```
$ docker images cudnn/devel
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
cudnn/devel         6.0.21-1            4cefb6f3b28d        4 days ago          6.01 GB
```

*	Test the newly-created image:
```
$ nvidia-docker run --rm cudnn/devel:6.0.21-1 /bin/bash -c "nvidia-smi"
```

## Generate PowerAI Docker Image

The following procedure makes it possible to create a PowerAI Docker image:

*	Download the PowerAI repository Debian package:
```
$ wget https://public.dhe.ibm.com/software/server/POWER/Linux/mldl/ubuntu/mldl-repo-local_4.0.0_ppc64el.deb
```

*	Create a Dockerfile in the same directory where the PowerAI repository Debian packages resides:
{% gist id="nicolas-tallet/e545548eb38c859b1328e54b9000c10a" %}{% endgist %}

*	Build the Docker image from previously created Dockerfile:
```
$ docker build -f Dockerfile.powerai-base-4 -t powerai/base:4.0.0 .
```

*	Validate the PowerAI Docker image:

  *	Start a container with the image previously created:
```
$ docker run -it powerai/base:4.0.0 /bin/bash
```

  * Install some prerequisites packages :
```
apt-get update && apt-get install –y vim wget
```

  * Load the caffe framework:
```
source /opt/DL/caffe/bin/caffe-activate
```

  * Install the Caffe example:
```
caffe-install-samples $HOME/caffe
```

  * You will first need to download and convert the data format from the MNIST website. To do this, simply run the following commands:
```
cd /root/caffe/
./data/mnist/get_mnist.sh
```

  * After running the script there should be two datasets, mnist_train_lmdb, and mnist_test_lmdb.
```
./examples/mnist/create_mnist.sh
```

  * We are going to train the model over 1000 iterations. Based on the solver setting, we will print the training loss function every 100 iterations, and test the network every 500 iterations.
```
./examples/mnist/train_lenet.sh
```

  *	More details can be found from this page  the "Training LeNet on MNIST with Caffe" (http://caffe.berkeleyvision.org/gathered/examples/mnist.html).

## Configure IBM Spectrum LSF

In order for Docker containers to be instantiated through Spectrum LSF, the following changes must be applied to the Spectrum LSF configuration:

*	Add the following settings to the `lsf.conf` configuration file:
```
LSB_RESOURCE_ENFORCE="cpu memory"
LSF_LINUX_CGROUP_ACCT=Y
LSF_PROCESS_TRACKING=Y
```

*	Add the following resource definition to the `lsf.shared` configuration file:
```
Begin Resource
RESOURCENAME  TYPE    INTERVAL INCREASING CONSUMABLE DESCRIPTION  # Keywords
docker        Boolean ()       ()         ()         (Docker container)
End Resource
```

*	Assign the *docker* resource to the Compute Nodes in the `lsf.cluster` configuration file:
```
Begin Host
    HOSTNAME  model  type  server  r1m  mem  swp  RESOURCES
    [...]
    host01    !      !     1       3.5  ()   ()   (docker)
    [...]
End Host
```

*	Define a Docker application in the `lsb.applications` configuration file:
```
Begin Application
CONTAINER = docker[image(registry:5000/powerai/base:4.0.0) --device=/dev/infiniband --device=/dev/nvidia0 --device=/dev/nvidia1 --device=/dev/nvidia2 --device=/dev/nvidia3 --device=/dev/nvidia-uvm --device=/dev/nvidia-uvm-tools --device=/dev/nvidiactl --ipc=host --network=host --rm --ulimit memlock=819200000:819200000 --volume /etc/group:/etc/group:ro --volume /etc/passwd:/etc/passwd:ro --volume /gpfs/home:/gpfs/home --volume /gpfs/scratch:/gpfs/scratch --volume-driver=nvidia-docker --volume=nvidia_driver_375.:/usr/local/nvidia:ro)]
DESCRIPTION = PowerAI
NAME = powerai
End Application
```

> Note 1: The filesystems that need to be visible inside the container are to be specified as arguments to the Docker start command through the `-v` option.

> Note 2: A second application declaration is required if the Docker image is expected to be used interactively.

*	Reconfigure LSF Batch daemons:
```
$ badmin reconfig
$ lsadmin reconfig
```

*	Check that the Docker resource is well associated to the Compute Nodes:
```
$ lshosts host01
HOST_NAME      type    model  cpuf ncpus maxmem maxswp server RESOURCES
host01      LINUXPP   POWER8 250.0   160   128G      -    Yes (docker)
```

It is also required to add the user that will own the docker process instantiated by Spectrum LSF to the `docker` user group. By default, unless specific setting at the Spectrum LSF application definition level, the user owning this process is `lsfadmin`; it should therefore be added to the `docker` group:
```
docker:x:596:lsfadmin
```

## Current Issues / Limitations

* Restrictive User Mask Prevents Docker-Based LSF Job Startup
  * Status:
    * Service Request opened: 54109,661,706
    * Fix to be implemented in IBM Spectrum LSF 10.1.0.3

* Handling of Bash Functions in User Environment at Docker Startup
  * Status:
    * Issue opened on GitHub: https://github.com/moby/moby/issues/33677
  * Workaround:
    * Prevent Bash functions transfer to Docker through `-env` option of `bsub` command:
```
$ bsub -app powerai -env "all,~BASH_FUNC_ml(),~BASH_FUNC_module()" < job.sh
```
