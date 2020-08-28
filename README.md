# Docker notes

## History

![history](\img\history.png)

## What is docker

**Docker** is a tool designed to make it easier to create, deploy, and run applications by using containers. (Isolates apps from each other)

**Containers** allow a developer to package up an application with all of the parts it needs, such as libraries and other dependencies, and deploy it as one package. Its decouples the OS from the application dependencies and the code.

## Why do you need docker

![compatibility_dependency_issue](\img\compatibility_dependency_issue.jpg)

* To avoid compatibility issues with an underlying OS or between the services & libraries dependencies with the OS. (So no more - It works on my machine!)

* To reduce local development environment setup time.

* Whenever your app needs to go through multiple phases dev/test/uat/prod (to operate as same on all the platforms).

* When you want to adopt a microservices architecture.

## What can docker do

![docker_ability](\img\docker_ability.jpg)

* Containerize an applications

* Run each service with its own dependencies in separate containers

## What are containers and images

**Container** is a completely isolated environment with their own processes, network interfaces and their own mounts except they all share the same OS kernel.

An **image** is a package or a template, it is used to create one or more containers. Containers are running instance of images.

## Docker command line structure (format)

```text
New way: docker <command> <sub-commands> (options)
Old way: docker <command> (options)

Management Commands:
  builder     Manage builds
  config      Manage Docker configs
  container   Manage containers
  context     Manage contexts
  image       Manage images
  network     Manage networks
  node        Manage Swarm nodes
  plugin      Manage plugins
  secret      Manage Docker secrets
  service     Manage services
  stack       Manage Docker stacks
  swarm       Manage Swarm
  system      Manage Docker
  trust       Manage trust on Docker images
  volume      Manage volumes

Example:
New way - docker container run
Old way - docker run
```

## Image vs Container

```text
- Image is the application we want to run
- Container is an instance of that image running as a process
```

## References

[1] <https://docs.docker.com/get-started/>

[2] <https://opensource.com/resources/what-docker>

[3] <https://docs.microsoft.com/en-us/dotnet/architecture/microservices/container-docker-introduction/docker-terminology>

[4] <https://www.redhat.com/en/topics/containers/what-is-docker>

[5] <https://github.com/collabnix/dockerlabs>
