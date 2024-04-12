# Docker-PyEnv

This repository creates GNU/Linux Docker images with a specific Python version
using [PyEnv]. Current tested base images include [Ubuntu 20.04], [CentOS 7]
and [openSUSE 15.3].


## Features

The images provide the following libraries:

- GCC and GFortran.
- BLAS and LAPACK.
- HDF4, HDF5 and NetCDF4.
- PyEnv with one specific Python version preinstalled.
- Latest available working versions of [`pip`], [`wheel`] and [`setuptools`].
- Latest available working versions of [`numpy`], [`scipy`] and [`cython`].

Below there is a summary table with the preinstalled packages:

| Version    | Py2.6  | Py2.7  | Py3.2  | Py3.3  | Py3.4  | Py3.5   | Py3.6   | Py3.7+  |
|------------|--------|--------|--------|--------|--------|---------|---------|---------|
| pip        | <10    | <21    | <7.1.1 | <18    | <20    | <21     | <21     | <21     |
| setuptools | <37    | <45    | <30    | <40    | <44    | <50     | <50     | <50     |
| wheel      | <0.30  | <0.36  | <0.32  | <0.30  | <0.34  | <0.36   | <0.36   | <0.36   |
| numpy      | <1.12  | <1.17  | <1.12  | <1.12  | <1.17  | <1.19   | <1.20   | <1.21   |
| scipy      | <0.18  | <1.3   | <0.18  | <1.0   | <1.3   | <1.5    | <1.6    | <1.7    |
| cython     | <3.0   | <3.0   | <0.27  | <3.0   | <3.0   | <3.1    | <3.1    | <3.1    |


## Installation

1. Install [Docker].

2. Download the automated build from the public [Docker Hub Registry]
   located at [molinav/pyenv].


## Usage

Given a Python version `X.Y`:
```sh
docker run --rm -it molinav/pyenv:X.Y-ubuntu-20.04
docker run --rm -it molinav/pyenv:X.Y-centos-7
docker run --rm -it molinav/pyenv:X.Y-opensuse-15.3
```

If not running interactively, you must configure the shell manually by calling
```sh
. /etc/profile
```
which will activate [PyEnv] and configure the shell to use the preinstalled
Python version.


[Ubuntu 20.04]:
https://hub.docker.com/_/ubuntu
[CentOS 7]:
https://hub.docker.com/_/centos
[openSUSE 15.3]:
https://hub.docker.com/r/opensuse/leap
[PyEnv]:
https://github.com/pyenv/pyenv
[`pip`]:
https://pypi.org/project/pip/
[`setuptools`]:
https://pypi.org/project/setuptools/
[`wheel`]:
https://pypi.org/project/wheel/
[`numpy`]:
https://numpy.org/
[`scipy`]:
https://scipy.org/
[`cython`]:
https://cython.org/
[Docker]:
https://www.docker.com/
[Docker Hub Registry]:
https://hub.docker.com/
[molinav/pyenv]:
https://hub.docker.com/r/molinav/pyenv

## Build and run image

```shell
ljohnson@Lees-MBP:[docker-pyenv](master)$ 
ljohnson@Lees-MBP:[docker-pyenv](master)$ docker build --tag ubuntu-pyenv-3.11.7 . --build-arg BASE_IMAGE=ubuntu:22.04 --build-arg PYTHON_VERSION=3.11.7
[+] Building 357.0s (20/20) FINISHED                                                                                                                                                                                   docker:desktop-linux
 => [internal] load build definition from Dockerfile                                                                                                                                                                                   0.0s
 => => transferring dockerfile: 3.06kB                                                                                                                                                                                                 0.0s
 => [internal] load metadata for docker.io/library/ubuntu:22.04                                                                                                                                                                        0.9s
 => [auth] sharing credentials for media.johnson.int:5000                                                                                                                                                                              0.0s
 => [internal] load .dockerignore                                                                                                                                                                                                      0.0s
 => => transferring context: 2B                                                                                                                                                                                                        0.0s
 => [host  1/13] FROM docker.io/library/ubuntu:22.04@sha256:77906da86b60585ce12215807090eb327e7386c8fafb5402369e421f44eff17e                                                                                                           0.0s
 => [internal] load build context                                                                                                                                                                                                      0.0s
 => => transferring context: 4.33kB                                                                                                                                                                                                    0.0s
 => CACHED [host  2/13] RUN ln -snf /usr/share/zoneinfo/UTC /etc/localtime && echo UTC > /etc/timezone                                                                                                                                 0.0s
 => [host  3/13] COPY scripts /home/scripts                                                                                                                                                                                            0.0s
 => [host  4/13] RUN sh /home/scripts/manager install openssl ca-certificates wget                                                                                                                                                    14.3s
 => [host  5/13] RUN sh /home/scripts/manager install git tar gzip patch                                                                                                                                                              20.5s 
 => [host  6/13] RUN sh /home/scripts/manager install pkg-config make gcc-full                                                                                                                                                        21.0s 
 => [host  7/13] RUN sh /home/scripts/manager install pyenv-dev                                                                                                                                                                       18.9s 
 => [host  8/13] RUN sh /home/scripts/manager install python-3.11.7                                                                                                                                                                  259.5s 
 => [host  9/13] RUN sh /home/scripts/manager remove pyenv-dev                                                                                                                                                                         2.8s 
 => [host 10/13] RUN sh /home/scripts/manager install python-pip python-wheel python-setuptools                                                                                                                                        8.3s 
 => [host 11/13] RUN pyenv_root=$(home/scripts/manager info pyenv-root)                      &&    find ${pyenv_root} -type f -name "*.pyc" | xargs rm -f                  &&    find ${pyenv_root} -type f -name "*.pyo" | xargs rm   0.6s 
 => [host 12/13] RUN rm -rf /home/scripts                                                                                                                                                                                              0.4s 
 => [host 13/13] RUN echo "Done!"                                                                                                                                                                                                      0.3s 
 => [stage-1 1/1] COPY --from=host / /                                                                                                                                                                                                 2.9s 
 => exporting to image                                                                                                                                                                                                                 3.3s 
 => => exporting layers                                                                                                                                                                                                                3.3s 
 => => writing image sha256:94a1ec745f278c0221e21ad7f47d02d5cc8cc7354a5d2de9bef4fb4973723fa0                                                                                                                                           0.0s
 => => naming to docker.io/library/ubuntu-pyenv-3.11.7                                                                                                                                                                                 0.0s

View build details: docker-desktop://dashboard/build/desktop-linux/desktop-linux/15l067xckbz19f9m5n2ixjj7x
ljohnson@Lees-MBP:[docker-pyenv](master)$ 
ljohnson@Lees-MBP:[docker-pyenv](master)$ 
ljohnson@Lees-MBP:[docker-pyenv](master)$ docker run --name py311-live --rm -it ubuntu-pyenv-3.11.7
root@e94810923e71:/# 
root@e94810923e71:/# which python
/usr/local/share/pyenv/shims/python
root@e94810923e71:/# python -V
Python 3.11.7
root@e94810923e71:/# exit
logout
ljohnson@Lees-MBP:[docker-pyenv](master)$ 
```

