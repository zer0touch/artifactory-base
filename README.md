# Artifactory
[![Build Status](https://travis-ci.org/zer0touch/artifactory-base.svg?branch=master)](https://travis-ci.org/zer0touch/artifactory-base)
# Note this is the base image for artifactory

Run [Artifactory](http://www.jfrog.com/home/v_artifactory_opensource_overview) inside a Docker container.

Link: [zer0touch/artifactory-base](https://hub.docker.com/r/zer0touch/artifactory-base/)


## Volumes
Artifactories `data`, `logs` and `backup` directories are exported as volumes:

    /artifactory/data
    /artifactory/logs
    /artifactory/backup

## Ports
The web server is accessible through port `8080`.

## Example
To run artifactory do:

    docker run -p 8080:8080 zer0touch/artifactory

Now point your browser to http://localhost:8080.

## Runtime options
Inject the environment variable `RUNTIME_OPTS` when starting a container to set Tomcat's runtime options (i.e. `CATALANA_OPTS`). The most common use case is to set the heap size:

    docker run -e RUNTIME_OPTS="-Xms256m -Xmx512m" -P mattgruter/artifactory

## Switching to Artifactory Pro
If you are using Artifactory Pro, the artifactory war archive has to be replaced. The Dockerfile includes a `ONBUILD` trigger for this purpose. Unpack the Artifactory Pro distribution ZIP file and place the file `artifactory.war` (located in the `webapps` subdirectory) in the same directory as a simple Dockerfile that extends this image:

    # Dockerfile for Artifactory Pro
    FROM zer0touch/artifactory

Now build your child docker image:

    docker build -t yourname/myartifactory

The `ONBUILD` trigger makes sure that your `artifactory.war` is picked up and applied to the image upon build.

    docker run -P yourname/myartifactory
