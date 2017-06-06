# Docker JS images

Js teams requires a nodejs container with nginx preinstalled for node server rendering and nginx static.

Docker JS is based on nginx official alpine container. Above it we install nodejs and npm. We use basically use the same dockerfile as the official nodejs container: https://github.com/nodejs/docker-node

Docker containers are build on Teamcity using the following bash script:

```bash
NODE_VERSION=$(grep -m1 'ENV NODE_VERSION' %NODE%/Dockerfile | cut -d' ' -f3)
DOCKER_IMAGE=%DOCKER_REGISTRY_URL%/docker-js

cd %NODE%

docker build --rm -t "${DOCKER_IMAGE}:${NODE_VERSION}-alpine" --no-cache=true .

docker push "${DOCKER_IMAGE}:${NODE_VERSION}-alpine"

echo "##teamcity[buildNumber '${NODE_VERSION}-alpine']"
```

where %NODE% is a configuration parameter set on Teamcity.

# How to

To create a new container for a new version of Nodejs, please do the following:

- Create a folder with the name of the version (major.minor) you want to support, for example: 6.10, 7.10, 8.0, etc.
- Add dockerfile to this folder
- Update dockerfile `ENV` values for `NODE_VERSION` and if needed `NPM_VERSION`

## License

Copyright 2017 SPB TV AG

Licensed under the Apache License, Version 2.0 (the ["License"](LICENSE)); you may not use this file except in compliance with the License.
You may obtain a copy of the License at [http://www.apache.org/licenses/LICENSE-2.0](http://www.apache.org/licenses/LICENSE-2.0)
Unless required by applicable law or agreed to in writing, software distributed under the License is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and limitations under the License.