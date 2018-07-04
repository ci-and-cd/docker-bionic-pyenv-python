# docker-bionic-pyenv-python

Python (pyenv) for multi-stage docker image build.

Dockerfile [ci-and-cd/docker-bionic-pyenv-python on Github](https://github.com/ci-and-cd/docker-bionic-pyenv-python)

[cirepo/bionic-pyenv-python on Docker Hub](https://hub.docker.com/r/cirepo/bionic-pyenv-python/)

## Use this image as a “stage” in multi-stage builds

```dockerfile
FROM alpine:3.7
COPY --from=cirepo/bionic-pyenv-python:2.7.14_3.6.5-archive /data/root /
RUN sudo chown -R $(whoami):$(id -gn) /home/$(whoami)
```
