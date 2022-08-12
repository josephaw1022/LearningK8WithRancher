# This repo is meant to display what is being learned and done in K8 

- The way that I am running Kubernetes locally is through Rancher Desktop which is one of the many open source projects backed by OpenSuse
- This is essentially a competitor to Docker Desktop that can be described in one simple way 
    - Docker Desktop is a docker first tool that just so happens to support kubernetes
    - Rancher Desktop is designed and tailored for Kubernetes from it's inception and makes it very easy to hit the ground running with working with kubernetes locally

- Although Docker Desktop now has a preview release for debian based distros, it works best on Mac and Windows
- However, Rancher Desktop works with most distros and works for mac, windows, and windows wsl/wsl2 as well.


- Rancher Desktop installs 
    - docker or nerdctl , I would chose whatever you're familiar with most. Although nerdctl is often described as a super set bleeding edge version of docker, it does have a few cli command differences (specifically the tags of the cli commands). I personally am just going to go with docker because it is the most simple and will be easier to get help from others with since most devs will probably heard and used docker. 
    - Helm
    - docker-compose and docker compose

- The local cluster is being managed through the rancher ui which is downloaded via the docker command given through Rancher's documentation https://rancher.com/docs/rancher/v2.5/en/installation/other-installation-methods/single-node-docker/


There is one pain point experienced when trying to get rancher ui working locally

- It seems as if the first command given in the documentation is what will work for everyone (simplest way to get going)
```bash

docker run -d --restart=unless-stopped \
  -p 80:80 -p 443:443 \
  --privileged \
  rancher/rancher:latest

```

- However, I experienced some issues trying to connecting to the container even with port forwarding. Instead, I had to navigate to the advanced use case page for this page https://rancher.com/docs/rancher/v2.5/en/installation/other-installation-methods/single-node-docker/advanced/ and found a command that worked for me personally. This is labelled 'Running rancher/rancher and rancher/rancher-agent on the Same Nodelink' on the page. 

```bash
docker run -d --restart=unless-stopped \
  -p 8080:80 -p 8443:443 \
  --privileged \
  rancher/rancher:latest

```

- Then I ran the following command and was able to find the local ip for the rancher ui container
```bash

docker ps 

```

- Once you connect to the local ip shown in the docker ps command and navigate to the private ip, any chromium based browser will warn you that the site is not safe do to the keys not being provided initially, however, this is just for local development and basic learning. This is beyond the point of this repo and meant for more production/enterprise clusters.

- You will be asked to do some weird bash command working to get the logs to get an first time use password. Follow the instructions and then create a new password. Also, one nuanced thing that you will have to know is that your immediate user name is 'admin'. I have had to start over with the previous steps before just because I had no idea what it was because of the fact that that is the default username that is given to the user that initializes the rancher ui container and accesses it. 

