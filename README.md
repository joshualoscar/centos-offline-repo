# centos-offline-repo
This is an example of how to use a docker to pull all the packages from centos and epel for an offline repo.



On Ubuntu

```
sudo apt-cache search docker

sudo apt-get install docker.io -y
```

or on centos

```
sudo yum search docker 

sudo yum install docker.io -y
```

==== Run these on your host OS after you have installed docker ====

This command looks for the newest centos version available


```
sudo docker search centos
```


This command is to make a directory where you will store your repo files. This could be a shared drive or portable drive.


```
sudo mkdir /repo-drive
```

In this example we will be pulling centos 7, so we will make a centos folder inside of our "repo-drive"

```
sudo mkdir /repo-drive/centos7
```

Now we will pull centos


```
sudo docker pull centos
```

In this step we want to name the docker image "centos7repos",
we then want to mount a shared volume from the host to the docker which is the path after the "-v"


```
sudo docker run -it --name centos7repos -v /repo-drive/centos7:/repos centos /bin/bash
```

At this point you should be placed inside of the docker


# Inside the docker or start here on a local machine
You will need to install a few packages.

```
yum install yum-utils.noarch epel-release.noarch ntfs-3g ntfs-progs createrepo -y
```

Then the next step is to run the repo sync command, the download_path is to where we mounted the shared volume before.
If you don't know what your "repoid" is, you can run the "sudo yum repolist" and use the names that are listed.

```
reposync --repoid=base --repoid=centosplus --repoid=updates --repoid=extras --repoid=epel --download_path=/repos/centos7

cd /repos

createrepo base && createrepo centosplus && createrepo updates && createrepo extras && createrepo epel
```

# If you are using docker follow these directions, but if you are using a local box you are finished
When the createrepo is compleate, you will need to exit docker, you can type exit or CTRL+C.

In the future when you want to open the docker again you can start it and attach to it with the following.

```
sudo docker start centos7repos

sudo docker attach centos7repos
```
