
# Deryl's Rails Development

I use Docker for development. This is a unified Dockerfile for developing Ruby on Rails applications.

By unified I mean to say that the database engine is included in the resulting image as well as the Rails related gems themselves.

**PLEASE NOTE**: Normally, the database engine would ***not*** be installed with the applicatiopn itself. It would normally reside in an additipnal image and then the use of Docker's networking options would link the Application and Database images together.

This image takes the unified (all in one) approach simply due to this being a first rendition of the entire stack pool. Basically, it answers the question: "What do I need exactly for a base image to build off of without refinements?"

# What's In An Image?

This image contains at least the following. Version information is as of initial commit of project.

    - Ruby 2.7
    - Rails 6.0.3
    - NodeJS 12.18
    - Yarn 1.22
    - PostgreSQL 12.2 (and its development header files)

# How To Build The Image

To build the base image, which means the primary image all others will be built from, simply follow this commandline.

```sh
docker build --force-rm --progress plain \
-t deryldowney/rails-toolset:latest \
-f Dockerfile.rails .
```

This commandline tells Docker to

`build` self-explanitory. Builds the image!

`--force-rm` forces the intermediate layers to be removed. Helps save space!

`--progress plain` displays the plain rolling output of the build process inside the image.

`-t deryldowney/rails-toolset:latest` tags the image under my [Docker Hub User](https://hub.docker.com/repository/docker/deryldowney/rails-toolset), calls it rails-toolset, and tags it as the latest version.

`-f Dockerfile.rails .` tells Docker to build the image using the file Dockerfile.rails located in the current directory using the current directory to do so.

## Deriving Additional Images

To derive additional images from this you would put

```
FROM deryldowney/rails-toolset:latest
```
at the top of your own `Dockerfile` and build out from there. Don't forget the `latest` so you always have the most up to date version. Simple as that!

## Changing The Build From The Command Line

If you want to change certain things, like the User's account info, you can either modify the Dockerfile.rails directly OR you can pass the `docker build` command some `--build-arg`.

Lets say you want to modify the User name to be 'appuser' instead of 'rails_user', nothing else. To do that you would run the command above but add ``--build-arg USER_NAME='appuser'``. This is why we used ARGs in the Dockerfile rather than ENV entries.

***Example***

```
    docker build --build-arg USER_NAME='appuser' \
    --force-rm --progress plain \
    -t deryldowney/rails-toolset:latest \
    -f Dockerfile.rails .
```

Bob's your uncle, there you go!

Have fun!
