# Docker images

## Background

[Docker](http://docker.com) is a project that automates deployment of applications inside software containers. Docker   
application is defined by _images_ that contain all of the components and steps needed to initialize the application instance. A _container_ is a running instance of the image. We provide an image that contains the compiled `dcmqi` library. By using `dcmqi` Docker container you can use `dcmqi` on any operating system without having to compile it. All you need to do is install Docker on your system, and download the `dcmqi` Docker image.

## Usage

You will first need to install Docker on your system following [these instructions](https://www.docker.com/products/overview). Docker is available for Mac, Windows and Linux. For the most part Docker installation is straightforward, but some extra steps need to be taken on Windows as discussed below.

### If you use Docker on Windows ...

Note the [system requirements](https://docs.docker.com/docker-for-windows/):

* you will need to have Windows 10 Pro or above
* you will need to enable Hyper-V package \(Docker will prompt you\)
* you will need to enable virtualization; [read this](https://docs.docker.com/docker-for-windows/troubleshoot/#virtualization-must-be-enabled) to learn how to check if it is enabled, and if it is not - here is [one guide](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/5/html/Virtualization/sect-Virtualization-Troubleshooting-Enabling_Intel_VT_and_AMD_V_virtualization_hardware_extensions_in_BIOS.html) that may help you do that, but it assumes you can access your BIOS settings

**IMPORTANT**: You will also need to share the drive you will be using to communicate data to and from Docker image in Docker Settings as shown in the screenshot below.

Most likely you will experience the display of an error message similar to the one shown below.

```text
C:\Program Files\Docker\Docker\Resources\bin\docker.exe: Error response from daemon: \ 
C: drive is not shared. Please share it in Docker for Windows Settings.
See 'C:\Program Files\Docker\Docker\Resources\bin\docker.exe run --help'.
```

If you have this error, make sure that the drive, where the `HOST_DIR` is located, is shared:

1. right click onto the Docker task bar icon and choose "Settings" 
2. choose "Shared Drives" from the left menu \(a list of drives that are available to share will be displayed\)
3. select the drive for your `HOST_DIR` to be shared
4. confirm with Apply and continue

![](../../.gitbook/assets/docker-windows.jpg)

### Once Docker is installed

Pull the `dcmqi` image to your system to instantiate the `dcmqi` container:

```text
$ docker pull qiicr/dcmqi
Using default tag: latest
latest: Pulling from qiicr/dcmqi
38892065247a: Already exists
87b645034784: Pull complete
c72dd60da47b: Pull complete
01a53946a0eb: Pull complete
8bb93fc0167f: Pull complete
0cea230bb5f0: Pull complete
4b26feab9bc2: Pull complete
82b0182ab925: Pull complete
3af6a0b06e1b: Pull complete
Digest: sha256:af03e96c28b92d0108453da546217a38665404a2ec327478ce68eaef6b092b14
Status: Downloaded newer image for qiicr/dcmqi:latest
```

You can now run any of the command line converter provided by dcmqi by passing the name of the converter as shown below:

```text
$ docker run qiicr/dcmqi itkimage2segimage --help
```

The Dockerfile for `qiicr/dcmqi` is available in the main repository of `dcmqi` [here](https://github.com/QIICR/dcmqi/blob/master/Dockerfile). It does not rely on any proprietary or non-open-source components.

## Shared directory between Docker container and host

Docker containers cannot directly access the filesystem of the host. In order to pass files as arguments to the `dcmqi` converter and to access files that converters create, an extra step is required to specify which directories will be used for file exchange using the `-v` argument:

```text
-v <HOST_DIR>:<CONTAINER_DIR>
```

The argument above will make the `HOST_DIR` path available within the container at `CONTAINER_DIR` location. The files that will be read or written by the converter run from the docker container should be referred to via the `CONTAINER_DIR` path.

## Usage example

\(also see [`dcmqi` introduction tutorial](http://qiicr.org/dcmqi-guide/tutorials/intro.html)!\)

Assuming the docker image is installed, create an empty directory `docker_test`.

```text
$ mkdir docker_test
```

Put the following test files from dcmqi source code repository into the `docker_test` directory:

* [the input parametric map in NRRD format](https://github.com/QIICR/dcmqi/raw/master/data/paramaps/pm-example-float.nrrd)
* [the DICOM file containing the metadata to be propagated into the output](https://github.com/QIICR/dcmqi/blob/master/data/paramaps/pm-example-slice.dcm)
* [the metadata JSON describing the parametric map](https://github.com/QIICR/dcmqi/blob/master/doc/examples/pm-example-float.json)

```text
$ pwd
/Users/fedorov/docker_test
$ ls
pm-example-float.json     
pm-example-float.nrrd     
pm-example-slice.dcm
```

Run the `itkimage2paramap` converter

```text
$ docker run -v /Users/fedorov/docker_test:/tmp qiicr/dcmqi itkimage2paramap \ 
   --inputImage /tmp/pm-example-float.nrrd \
   --inputMetadata /tmp/pm-example-float.json \
   --outputDICOM /tmp/docker_output_paramap.dcm \
   --inputDICOM /tmp/pm-example-slice.dcm

Input image size: [256, 256, 20]
Directions: 0.999981 -0.00540165 -0.00298377
0.00480058 0.984755 -0.173879
0.00387751 0.173861 0.984763

(99QIICR,DWMPxxxxx1,Source image diffusion b-value): NUMERIC: 0, Units: (UCUM,s/mm2,seconds per square millimeter)), Float value(s): <none>
(99QIICR,DWMPxxxxx1,Source image diffusion b-value): NUMERIC: 1400, Units: (UCUM,s/mm2,seconds per square millimeter)), Float value(s): <none>
Saved parametric map as /tmp/docker_output_paramap.dcm
```

The output DICOM object will be saved as `docker_output_paramap.dcm` in the `docker_test` directory.

