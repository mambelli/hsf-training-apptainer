---
title: 'Reference'
---

## Glossary

[container]{#container}
:   A container is a standard unit of software that packages up code and all its dependencies so the application runs quickly and reliably from one computing environment to another. A Docker container image is a lightweight, standalone, executable package of software that includes everything needed to run an application: code, runtime, system tools, system libraries and settings.

[engine]{#engine}
:   A container engine is a piece of software that accepts user requests, including command line options, pulls images, and from the end user's perspective runs the container. There are many container engines, including docker, RKT, CRI-O, and LXD. Also, many cloud providers, Platforms as a Service (PaaS), and Container Platforms have their own built-in container engines which consume Docker or OCI compliant Container Images. Having an industry standard [Container Image Format](#imageformat) allows interoperability between all of these different platforms.

[host]{#host}
:   The container host is the system that runs the containerized processes, often simply called containers. Once a [container image](#image) (aka repository) is pulled from a [Registry Server](#registry) to the local container host, it is said to be in the local cache.

[HTTP]{#http}
:   The Hypertext Transfer [Protocol](#protocol) used for sharing web pages and other data
on the World Wide Web.

[image]{#image}
:   A container image is a static file with executable code that can create a container on a computing system. A container image is immutable—meaning it cannot be changed, and can be deployed consistently in any environment. It is a core component of a containerized architecture.

[image format]{#imageformat}
:   Historically, each [Container Engine](#engine) had its container images format. LXD, RKT, and Docker all had their own image formats. Some were made up of a single layer, while others were made up of a bunch of layers in a tree structure. Today, almost all major tools and engines have moved to a format defined by the [Open Container Initiative (OCI)](https://opencontainers.org/faq/), originally based on the [Docker V2 image format](https://blog.docker.com/2017/07/demystifying-open-container-initiative-oci-specifications/). This image format defines the [layers and metadata](https://docs.google.com/presentation/d/1OpsvPvA82HJjHN3Vm2oVrqca1FCfn0PAfxGZ2w_ZZgc/edit#slide=id.g2441f8cc8d_0_49) within a container image. Essentially, the OCI image format defines a container image composed of tar files for each layer, and a manifest.json file with the metadata.

[layer]{#layer}
:   Repositories are often referred to as [images](#image) or [container images](#image), but actually they are made up of one or more layers. Image layers in a repository are connected together in a parent-child relationship. Each image layer represents changes between itself and the parent layer.

[namespace]{#namespace}
:   A namespace is a tool for separating groups of [repositories](#repository). On the public [DockerHub](https://hub.docker.com/), the namespace is typically the username of the person sharing the [image](#image), but can also be a group name, or a logical name. 

[protocol]{#protocol}
:   A set of rules that define how one computer communicates with another.
Common protocols on the Internet include [HTTP](#http) and [SSH](#ssh).

[Registry Server]{#registry}
:   A registry server is essentially a fancy file server that is used to store docker repositories. Typically, the registry server is specified as a normal DNS name and optionally a port number to connect to.

[repository]{#repository}
:   When using the docker command, a repository is what is specified on the command line, not an image. In the following command, “rhel7” is the repository.
    ```
    docker pull rhel7
    ```
    This is actually expanded automatically to:
    ```
    docker pull registry.access.redhat.com/rhel7:latest  # REGISTRY/NAMESPACE/REPOSITORY[:TAG]
    ```
    This can be confusing, and many people refer to this as an image or a container image. In fact, the docker images sub-command is what is used to list the locally available repositories. Conceptually, these repositories can be thought of as container images, but it’s important to realize that these repositories are actually made up of layers and include metadata about in a file referred to as the manifest (manifest.json)

[runtime]{#runtime}
:   A container runtime a lower level component typically used in a [Container Engine](#engine) but can also be used by hand for testing. The [Open Containers Initiative (OCI)](https://opencontainers.org/faq/) Runtime Standard reference implementation is [runc](https://github.com/opencontainers/runc). This is the most widely used container runtime, but there are others OCI compliant runtimes, such as [crun](https://github.com/giuseppe/crun), [railcar](https://github.com/oracle/railcar), and [katacontainers](https://katacontainers.io/). Docker, CRI-O, and many other Container Engines rely on runc.

[SSH]{#ssh}
:   The Secure Shell [protocol](#protocol) used for secure communication between computers.
