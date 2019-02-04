# Docker role

### Dependency:
  - Ubuntu

By default, this role install latest stable version of docker from official docker apt repository. You can choose docker version. Set ansible variable DOCKER_VER in the right version.
For example:
```sh
DOCKER_VER: "docker-ce=<VERSION_STRING>"
```