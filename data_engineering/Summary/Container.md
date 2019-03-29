# Container

Continaers are encapsulation of an application with its dependencies. It shares resources with the host OS so it is much more efficient as it is constrained to run the same kernel as the host. Because of the lightweight, now developers can run many containers at the same time, and one machine can have much more containers than the VMs.

Meanwhile, it eliminates concerns from the changes in running environemnt.This also has the advantage for collaboration because outside developers can download the containers and spin up without much worry for environment or dependency installation. 

## VM vs. Container

Now you might wonder why we still have VM in the production. There are still concerns regarding the isolation of the container. See more here at <https://www.oreilly.com/ideas/five-security-concerns-when-using-docker>. VM has a long-time proven isolation in the industry with the battle-hardened technology. In the enterprise world, it is better to go risk-adverse and implement VM solutions for production. However, optimistically, the industry is pushing forward with better solutions for containers. For example, people can run containers inside VMs to take advantage of both technologies.

![alt text](https://blog.netapp.com/wp-content/uploads/2016/03/Screen-Shot-2018-03-20-at-9.24.09-AM.png "Container vs VM")

At the same time, VMs and containers are built for different initiatives. VM is for a total foreign environment while the container is for making application portable and self-contained.

## Docker

Containers are old concept back in Unix with `chroot` command. Organizations like FreeBSD, Solaris Zones, SWsoft and Google all have contributed to the development of container solution. And in the end Docker nowadays is the mainstream for containerization by putting all the puzzles together. 

Docker creates a complete solution for the creation and distribution of containers, with Docker Engine and Docker Hub.

* Docker Engine: creating and running containers which is totally open source
* Docker Hub: cloud service for distributing containers

Docker Hub has enormous number of public container images for download (e.g Java environment image), which reduces replication of same procedure.

## Sources

* <https://learning.oreilly.com/library/view/using-docker/9781491915752/ch01.html#what_and_why>
* <https://blog.netapp.com/blogs/containers-vs-vms/>