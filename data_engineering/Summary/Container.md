## Container

Continaers are encapsulation of an application with its dependencies. It shares resources with the host OS so it is much more efficient. Because of the lightweight, now developers can run many containers at the same time, and one machine can have much more containers than the VMs.

Meanwhile, it eliminates concerns from the changes in running environemnt.This also has the advantage for collaboration because outside developers can download the containers and spin up without much worry for environment or dependency installation. 

Now you might wonder why we still have VM in the production. There are still concerns regarding the security of the container. <https://www.oreilly.com/ideas/five-security-concerns-when-using-docker>. In the enterprise world, it is better to go risk-adverse and implement VM solutions for production. However, optimistically, the industry is pushing forward with better solutions for containers.

At the same time, VMs and containers are built for different initiatives. VM is for a total foreign environment while the container is for making application portable and self-contained.

Source:
* <https://learning.oreilly.com/library/view/using-docker/9781491915752/ch01.html#what_and_why>