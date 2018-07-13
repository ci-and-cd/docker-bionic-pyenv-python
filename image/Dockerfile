
FROM cirepo/nix:2.0.4-bionic


ARG IMAGE_ARG_APT_MIRROR
ARG IMAGE_ARG_PYTHON2_VERSION
ARG IMAGE_ARG_PYTHON3_VERSION


COPY --chown=ubuntu:ubuntu docker /data


# PYTHON_BUILD_MIRROR_URL (use checksum to locate files, hard to maintain)
RUN set -ex \
  && sudo sed -i "s/http:\/\/archive.ubuntu.com\/ubuntu\//http:\/\/${IMAGE_ARG_APT_MIRROR:-archive.ubuntu.com}\/ubuntu\//g" /etc/apt/sources.list \
  && sudo apt-get -y update \
  && echo "install tools and libs for building python" \
  && sudo apt-get -y install gcc git libssl-dev make zlib1g-dev \
  && echo "install optional extensions for building python" \
  && sudo apt-get -y install libbz2-dev libreadline-dev libsqlite3-dev \
  && echo "install pyenv and build python" \
  && echo "TODO fix git's 'Problem with the SSL CA cert' issue" \
  && git config --global http.sslverify false \
  && export USE_GIT_URI=true \
  && curl -L https://raw.github.com/yyuu/pyenv-installer/master/bin/pyenv-installer | bash \
  && touch ~/.bash_profile \
  && echo 'export PATH="${HOME}/.pyenv/bin:${PATH}"' >> ~/.bash_profile \
  && echo 'if which pyenv > /dev/null; then eval "$(pyenv init -)"; eval "$(pyenv virtualenv-init -)"; fi' >> ~/.bash_profile \
  && cp -r /data ~/.pyenv/cache \
  && ls -la ~/.pyenv/cache \
  && if [ ! -f ~/.pyenv/cache/Python-${IMAGE_ARG_PYTHON2_VERSION:-2.7.14}.tar.xz ]; then ${ARIA2C_DOWNLOAD} -d ~/.pyenv/cache -o Python-${IMAGE_ARG_PYTHON2_VERSION:-2.7.14}.tar.xz http://mirrors.sohu.com/python/${IMAGE_ARG_PYTHON2_VERSION:-2.7.14}/Python-${IMAGE_ARG_PYTHON2_VERSION:-2.7.14}.tar.xz; fi \
  && /bin/bash --login -c "pyenv install -v ${IMAGE_ARG_PYTHON2_VERSION:-2.7.14}" \
  && if [ ! -f ~/.pyenv/cache/Python-${IMAGE_ARG_PYTHON3_VERSION:-3.6.5}.tar.xz ]; then ${ARIA2C_DOWNLOAD} -d ~/.pyenv/cache -o Python-${IMAGE_ARG_PYTHON3_VERSION:-3.6.5}.tar.xz http://mirrors.sohu.com/python/${IMAGE_ARG_PYTHON3_VERSION:-3.6.5}/Python-${IMAGE_ARG_PYTHON3_VERSION:-3.6.5}.tar.xz; fi \
  && /bin/bash --login -c "pyenv install -v ${IMAGE_ARG_PYTHON3_VERSION:-3.6.5}" \
  && sudo apt-get -y remove gcc git libbz2-dev libsqlite3-dev libssl-dev make zlib1g-dev \
  && sudo apt-get -y install bzip2 sqlite3 openssl zlib1g \
  && echo "cleanup" \
  && rm -rf ~/.pyenv/cache/* \
  && sudo apt-get -q -y autoremove \
  && sudo apt-get -q -y clean && sudo rm -rf /tmp/* && sudo rm -rf /var/cache && sudo rm -rf /var/lib/apt && sudo rm -rf /var/log \
  && sudo mkdir -p /var/cache/apt/archives/partial && sudo mkdir -p /var/log/apt


#RUN set -ex \
#  && sed -i "s/http:\/\/archive.ubuntu.com\/ubuntu\//http:\/\/${IMAGE_ARG_APT_MIRROR:-archive.ubuntu.com}\/ubuntu\//g" /etc/apt/sources.list \
#  && apt-get update -y \
#  && apt-get install -y python python-pip \
#  && apt-get -q autoremove \
#  && apt-get -q clean -y && rm -rf /var/lib/apt/lists/* && rm -f /var/cache/apt/*.bin

#  && apt-get install -y --reinstall ca-certificates \
#  && mkdir /usr/local/share/ca-certificates/cacert.org \
#  && wget -P /usr/local/share/ca-certificates/cacert.org http://www.cacert.org/certs/root.crt http://www.cacert.org/certs/class3.crt \
#  && update-ca-certificates \
#  && git config --global http.sslCAinfo /etc/ssl/certs/ca-certificates.crt \
#  && git config --global http.sslverify true
