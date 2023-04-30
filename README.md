# platys-trial-workspace
Gitpod for experimenting with Platys - a tool for generating and provisioning Modern Data Platforms based on Docker and Docker Compose from Trivadis. Its main use is for small-scale Data Lab projects, Proof-of-Concepts (PoC) or Proof-of-value (PoV) projects as well as trainings.

The user of platys can choose which services to use from a list of supported services and generate a fully working docker-compose.yml file including all necessary configuration files.

Steps:

download
sudo curl -L "https://github.com/TrivadisPF/platys/releases/download/2.4.3/platys_2.4.3_linux_x86_64.tar.gz" -o /tmp/platys.tar.gz

cd /tmp
tar zvxf /tmp/platys.tar.gz 
sudo mv platys /usr/local/bin/
sudo chown root:root /usr/local/bin/platys
sudo rm /tmp/platys.tar.gz 


[![Open in Gitpod](https://gitpod.io/button/open-in-gitpod.svg)](https://gitpod.io/#https://github.com/lucasjellema/platys-trial-workspace)


# Resources

[Introduction to Platys](https://github.com/TrivadisPF/platys)
[Getting started with Platys](https://github.com/TrivadisPF/platys-modern-data-platform/blob/master/documentation/getting-started.md)
[Platys Modern Data Platform - Overview of supported Services ](https://github.com/TrivadisPF/platys-modern-data-platform)
[Installing Platys](https://github.com/TrivadisPF/platys/blob/master/documentation/install.md)