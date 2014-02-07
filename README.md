Drone is a Continuous Integration platform built on Docker

### System

Drone is tested on the following versions of Ubuntu:

* Ubuntu Precise 12.04 (LTS) (64-bit)
* Ubuntu Raring 13.04 (64 bit)

Drone's only external dependency is the latest version of Docker (0.8)

### Setup

Drone is packaged and distributed as a debian file. You can download an install
using the following commands:

```sh
$ wget http://downloads.drone.io/latest/drone.deb
$ dpkg -i drone.deb
$ sudo start drone
```

Once Drone is running (by default on :80) navigate to **http://localhost:80/install**
and follow the steps in the setup wizard.

### Builds

Drone use a **.drone.yml** configuration file in the root of your
repository to run your build:

```
image: mischief/docker-golang
env:
  - GOPATH=/var/cache/drone
script:
  - go build
  - go test -v
service:
  - redis
notify:
  email:
    recipients:
      - brad@drone.io
      - burke@drone.io
```

### Images

In the above example we used a custom Docker image from index.docker.io **mischief/docker-golang**

Drone also provides official build images. These images are configured specifically for CI and
have many common software packages pre-installed (git, xvfb, firefox, libsqlite, etc).

Official Drone images are referenced in the **.drone.yml** by an alias:

```sh
image: go1.2   # same as bradrydzewski/go:1.2
```

Here is a list of our official images:

```sh
# these are the base images for all Drone containers.
# these are BIG (~3GB) so make sure you have a FAST internet connection
docker pull bradrydzewski/ubuntu
docker pull bradrydzewski/base

# clojure images
docker pull bradrydzewski/lein             # image: lein

# dart images
docker pull bradrydzewski/dart:stable      # image: dart

# erlang images
docker pull bradrydzewski/erlang:R16B      # image: erlangR16B
docker pull bradrydzewski/erlang:R16B02    # image: erlangR16B02
docker pull bradrydzewski/erlang:R16B01    # image: erlangR16B01

# gcc images (c/c++)
docker pull bradrydzewski/gcc:4.6          # image: gcc4.6
docker pull bradrydzewski/gcc:4.8          # image: gcc4.8

# go images
docker pull bradrydzewski/go:1.0           # image: go1
docker pull bradrydzewski/go:1.1           # image: go1.1
docker pull bradrydzewski/go:1.2           # image: go1.2

# haskell images
docker pull bradrydzewski/haskell:7.4      # image: haskell

# java and jdk images
docker pull bradrydzewski/java:openjdk6    # image: openjdk6
docker pull bradrydzewski/java:openjdk7    # image: openjdk7
docker pull bradrydzewski/java:oraclejdk7  # image: oraclejdk7
docker pull bradrydzewski/java:oraclejdk8  # image: oraclejdk8

# node images
docker pull bradrydzewski/node:0.10        # image node0.10
docker pull bradrydzewski/node:0.8         # image node0.8

# php images
docker pull bradrydzewski/php:5.5          # image: php5.5
docker pull bradrydzewski/php:5.4          # image: php5.4

# python images
docker pull bradrydzewski/python:2.7       # image: python2.7
docker pull bradrydzewski/python:3.2       # image: python3.2
docker pull bradrydzewski/python:3.3       # image: python3.3
docker pull bradrydzewski/python:pypy      # image: pypy

# ruby images
docker pull bradrydzewski/ruby:2.0.0       # image: ruby2.0.0
docker pull bradrydzewski/ruby:1.9.3       # image: ruby1.9.3

# scala images
docker pull bradrydzewski/scala:2.10.3     # image: scala2.10.3
docker pull bradrydzewski/scala:2.9.3      # image: scala2.9.3

```

### Environment

Drone clones your repository into a Docker container
at the following location:

```
/var/cache/drone/src/github.com/$owner/$name
```

Please take this into consideration when setting up your build commands, or
if you are using a custom Docker image.

### Databases

Drone can launch database containers for your build: 

```
service:
  - cassandra
  - couchdb
  - elasticsearch
  - neo4j
  - mongodb
  - mysql
  - postgres
  - rabbitmq
  - redis
  - riak
  - zookeeper
```

**NOTE:** database and service containers are exposed over TCP connections and
have their own local IP address. If the **socat** utility is installed inside your
Docker image, Drone will automatically proxy localhost connections to the correct
IP address.

### Deployments

Drone can trigger a deployment at the successful completion of your build:

```
deploy:
  heroku:
    app: safe-island-6261

publish:
  s3:
    acl: public-read
    region: us-east-1
    bucket: downloads.drone.io
    access_key: C24526974F365C3B
    secret_key: 2263c9751ed084a68df28fd2f658b127
    source: /tmp/drone.deb
    target: latest/

```

### Notifications

Drone can trigger email, hipchat and web hook notification at the completion
of your build:

```
notify:
  email:
    recipients:
      - brad@drone.io
      - burke@drone.io

  urls:
    - http://my-deploy-hook.com

  hipchat:
    room: support
	token: 3028700e5466d375
```

### Docs

Coming Soon to [drone.readthedocs.org](http://drone.readthedocs.org/)


