# This repo is meant to display what is being learned and done in K8 

The way that I am running Kubernetes locally is through Rancher Desktop which is one of the many open source projects backed by OpenSuse

This is essentially a competitor to Docker Desktop that can be described in one simple way 
    - Docker Desktop is a docker first tool that just so happens to support kubernetes
    - Rancher Desktop is designed and tailored for Kubernetes from it's inception and makes it very easy to hit the ground running with working with kubernetes locally

Although Docker Desktop now has a preview release for debian based distros, it works best on Mac and Windows
However, Rancher Desktop works with most distros and works for mac, windows, and windows wsl/wsl2 as well.


Rancher Desktop installs 
    - docker or nerdctl , I would chose whatever you're familiar with most. Although nerdctl is often described as a super set bleeding edge version of docker, it does have a few cli command differences (specifically the tags of the cli commands). I personally am just going to go with docker because it is the most simple and will be easier to get help from others with since most devs will probably heard and used docker. 
    - Helm
    - docker-compose and docker compose

The local cluster is being managed through the rancher ui which is downloaded via the docker command given through Rancher's documentation https://rancher.com/docs/rancher/v2.5/en/installation/other-installation-methods/single-node-docker/


I am using Rancher UI because it is an open source kubernetes management tool in which you can completely manage a private, hybrid, public, and or multi-cloud k8 cluster that can handle your entire cluster through an web ui. The appeal to this approach is that you avoid vendor lock in and have cloud agnostic knowledge, meaning that if you have an eks cluster and then one day AWS doubled their prices, then you wouldn't have to deal with the pains of having to find a new provider and learning how to do what you already have done on AWS on another provider. Instead in Rancher, if you want to change your cloud provider, then all you have to do is create new nodes and have the deployments ran on those nodes. And from a business perspective, it makes it easy for the engineers to make clusters when the method to make deployments, services, cron jobs, etc.. is the same regardless of the client's cloud provider. So it just saves time on a businesses end and makes for a more predictable workflow. 
![Alt text](assets/create-cluster-or-node.png?raw=true "Title")

- Now one thing that I think will help prevent any potential vendor lock in is to use open source tools that are cloud agnostic as much as possible. This is very obvious but is worth stating because most developers are aware of how many open source alternatives theres are. For example, most businesses use a cloud's blob storage for file storage, data back ups, cloud stack resources, static site hosting, etc... with tools like s3, azure blob storage, and gcp blob with S3 being the most popular due it being the first ever cloud service ever offered (Don't believe me? Look up AWS first's service). However, an S3 api compatible storage service called minio is open source and can be ran on public, private, hybrid, and or multi cloud clusters. There is even a tooling for it called minio console which is an ui to manage your storage buckets that can be run an compute instance on any cloud. This makes the management of assets consistent regardless of cloud. Another example of a cloud agnostic tool is RabbitMQ which is multi-paradigm message queue which can also be managed through a ui tool that can used through another container of the docker ui image. But if you don't feel like setting up all of this, you could use SupaBase https://supabase.com or Hyper (in beta) https://hyper.io which are open source tools that have pre built solutions for caching, live updating, database setup, message queues, files, etc... There are so many open source tools that I didn't mention but I do want to stay this README to be focused on Rancher for now.


It contains ui pages and routes that show you other open source tooling that works well with rancher and can act as a plugin or (sort of like extra dlc for your cluster). For example, on the cluster tool page, you can find the most common applications used with rancher. These applications are essentially just helm charts that sort of automate a bunch of the manual configuring on a specific resource in a cluster. There's way more to it, but this is dumbed down simple way of explaining it. Some of the applications include istio which is an open source service mesh. It also includes Longhorn, Prometheus Monitoring with Grafana, NeuVector, Logging, OPA Gatekeeper. Now, there is another page that includes even more helm charts and that can be found on by clicking Apps on the left side drawer menu in the cluster dropdown.  


![Alt text](assets/cluster-tools.png?raw=true "Title")

![Alt text](assets/charts.png?raw=true "Title")




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


- Then once you are logged in, that is when you can start doing really cool stuff that would've have to be done via cli commands and yaml files before. Knowing how to do basic and some advanced configuration with the kubectl cli and yaml files is still recommended to know, because there are a few issues that can appear in advanced edge cases. And, you will need to go into the yaml editor in the rancher ui and edit the yaml directly. If you don't know how the yaml files work and solely rely on the ui to run your cluster, then you will be out of luck and will go back and have to spend time learning the basics just to solve an advanced edge case (which is not ideal). I have saw this happen in multiple projects in a professional work context, especially when the new rancher 2 ui updates being rolled out that had breaking changes and a brand redesigned ui that was wonky initially and was for some reason released in a 2.X minor version instead of going to rancher 3. The Ui is much stabler now, however, I am sure there some edge cases that can still cause problems. However, when learning rancher itself, you won't run into this much as many of low hanging fruit issues are being knocked out.

