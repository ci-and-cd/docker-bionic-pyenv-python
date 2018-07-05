# docker-bionic-pyenv-python

Python (pyenv) for multi-stage docker image build.

Dockerfile [ci-and-cd/docker-bionic-pyenv-python on Github](https://github.com/ci-and-cd/docker-bionic-pyenv-python)

[cirepo/bionic-pyenv-python on Docker Hub](https://hub.docker.com/r/cirepo/bionic-pyenv-python/)

## Use this image as a “stage” in multi-stage builds

```dockerfile

FROM ubuntu:18.04
COPY --from=cirepo/bionic-pyenv-python:2.7.14_3.6.5-archive /data/root /
RUN sudo chown -R $(whoami):$(id -gn) /home/$(whoami) \
  && sudo apt-get -y install gcc libbz2-dev libsqlite3-dev libssl-dev make zlib1g-dev \
  && touch home/$(whoami)/.bash_profile \
  && echo 'export PATH="${HOME}/.pyenv/bin:${PATH}"\
if which pyenv > /dev/null; then eval "$(pyenv init -)"; eval "$(pyenv virtualenv-init -)"; fi' >> home/$(whoami)/.bash_profile

```
