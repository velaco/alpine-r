# alpine-r

Docker images for R based on Alpine Linux.

## Supported Tags

* **base-3.5.0-r1** ([base-3.5.0-r1/Dockerfile](https://github.com/velaco/alpine-r/blob/master/base-3.5.0-r1/Dockerfile)): R base and dev built from Alpine Linux 3.8 branch native packages (APK).
* **base-3.5.1-r1** ([base-3.5.1-r1/Dockerfile](https://github.com/velaco/alpine-r/blob/master/base-3.5.1-r1/Dockerfile)): R base and dev built from Alpine Linux edge branch native packages (APK).

## Why Use alpine-r Images

Alpine Linux is a lightweight distribution. Using it as a base to build Docker images can significantly reduce the size of the final image. 

|REPOSITORY|TAG|BASE|COMPRESSED SIZE|
|-|-|-|-|
|velaco/alpine-r|base-3.5.0-r1|alpine:3.8|143MB|
|rocker/r-base|latest|debian:testing|291 MB|

*Notes*. Table last updated 27 Oct 2018. The rocker/r-base image also includes additional packages of interest to R users, e.g. littler.

Although smaller images are usually preferred, *I don't recommend using Docker images for R based on Alpine Linux in production*. This is not an official repository and images from [rocker](https://hub.docker.com/r/rocker/) are much more reliable than the ones you'll find here.

## How to Use alpine-r Images

If you decide to use these images, you can get them with the docker pull command. For example, if you want to pull the image tagged `base-3.5.0-r1`, use this command:

```shell
docker pull velaco/alpine-r:base-3.5.0-r1
```
To run the container, which starts R by default, run:

```shell
docker run --rm -it velaco/alpine-r:base-3.5.0-r1
```

You can also specify a different command you want to run. For example, running the container with following command will start a shell:

```shell
docker run --rm -it velaco/alpine-r:base-3.5.0-r1 sh
```
To use the image as a base for building your images, add the following line to your Dockerfile:

```shell
FROM velaco/alpine-r:base-3.5.0-r1

# Your instructions from here
```

## License

The code in this repository is MIT licensed (see the `LICENSE` file for details).
