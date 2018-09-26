
FROM cirepo/nix:2.1.1-bionic


ARG IMAGE_ARG_APT_MIRROR
ARG IMAGE_ARG_PYTHON2_VERSION
ARG IMAGE_ARG_PYTHON3_VERSION


COPY --chown=ubuntu:ubuntu docker /data


# PYTHON_BUILD_MIRROR_URL (use checksum to locate files, hard to maintain)
# pyenv install 3.5.2, The Python ssl extension was not compiled. Missing the OpenSSL lib
# see: https://github.com/pyenv/pyenv/issues/950#issuecomment-328191825
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
  && touch ~/.profile \
  && echo 'export PATH="${HOME}/.pyenv/bin:${PATH}"' >> ~/.profile \
  && echo 'if which pyenv > /dev/null; then eval "$(pyenv init -)"; eval "$(pyenv virtualenv-init -)"; fi' >> ~/.profile \
  && cp -r /data ~/.pyenv/cache \
  && ls -la ~/.pyenv/cache \
  && if [ ! -f ~/.pyenv/cache/Python-3.5.3.tar.xz ]; then ${ARIA2C_DOWNLOAD} -d ~/.pyenv/cache -o Python-3.5.3.tar.xz ${IMAGE_ARG_PYTHON_MIRROR:-https://www.python.org/ftp/python}/3.5.3/Python-3.5.3.tar.xz; fi \
  && /bin/bash --login -c "CFLAGS=-I/usr/include/openssl LDFLAGS=-L/usr/lib pyenv install -v 3.5.3" \
  && if [ ! -f ~/.pyenv/cache/Python-${IMAGE_ARG_PYTHON2_VERSION:-2.7.15}.tar.xz ]; then ${ARIA2C_DOWNLOAD} -d ~/.pyenv/cache -o Python-${IMAGE_ARG_PYTHON2_VERSION:-2.7.15}.tar.xz ${IMAGE_ARG_PYTHON_MIRROR:-https://www.python.org/ftp/python}/${IMAGE_ARG_PYTHON2_VERSION:-2.7.15}/Python-${IMAGE_ARG_PYTHON2_VERSION:-2.7.15}.tar.xz; fi \
  && /bin/bash --login -c "pyenv install -v ${IMAGE_ARG_PYTHON2_VERSION:-2.7.15}" \
  && if [ ! -f ~/.pyenv/cache/Python-${IMAGE_ARG_PYTHON3_VERSION:-3.6.6}.tar.xz ]; then ${ARIA2C_DOWNLOAD} -d ~/.pyenv/cache -o Python-${IMAGE_ARG_PYTHON3_VERSION:-3.6.6}.tar.xz ${IMAGE_ARG_PYTHON_MIRROR:-https://www.python.org/ftp/python}/${IMAGE_ARG_PYTHON3_VERSION:-3.6.6}/Python-${IMAGE_ARG_PYTHON3_VERSION:-3.6.6}.tar.xz; fi \
  && /bin/bash --login -c "pyenv install -v ${IMAGE_ARG_PYTHON3_VERSION:-3.6.6}" \
  && sudo apt-get -y remove gcc git libbz2-dev libsqlite3-dev libssl-dev make zlib1g-dev \
  && sudo apt-get -y install bzip2 openssl sqlite3 zlib1g \
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
