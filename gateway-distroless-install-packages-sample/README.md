# Add Packages to Gateway Container

## Description
The container image for Layer 7 API Gateway 11.2.0 is built on the distroless base image and does not have a package manager pre-installed. 
The Dockerfile provides an example of how to add packages to the 11.2.0 container image.

## Execute the following command to build the image:

   `DOCKER_BUILDKIT=1 docker buildx build -t <IMAGE_NAME>   --platform linux/amd 64 --no-cache --build-arg GW_IMAGE=<GATEWAY_IMAGE> -f Dockerfile .`
