# Docker JS images

Js teams requires a nodejs container with nginx preinstalled for node server rendering and nginx static.

Docker JS is based on nginx official alpine container. Over it we install nodejs and npm. We use basically the same dockerfile as the official nodejs container: https://github.com/nodejs/docker-node

Docker containers are build on [Teamcity](http://teamcity.mwm.local/admin/editProject.html?projectId=JsFrontier_DockerRegistrySpbtvComDockerJs) using the following bash script:

```bash
NODE_VERSION=$(grep -m1 'ENV NODE_VERSION' %NODE%/Dockerfile | cut -d' ' -f3)
DOCKER_IMAGE=docker-registry.spbtv.com/docker-js

cd %NODE%

docker build --rm -t "${DOCKER_IMAGE}:${NODE_VERSION}-alpine" --no-cache=true .

docker push "${DOCKER_IMAGE}:${NODE_VERSION}-alpine"

echo "##teamcity[buildNumber '${NODE_VERSION}-alpine']"
```

where %NODE% is a configuration parameter set on Teamcity.
