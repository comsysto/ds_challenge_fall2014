Data Science Challenge 3
====
See: http://www.cloudera.com/content/cloudera/en/training/certification/ccp-ds/challenge/challenge3.html

Online Travel Services, Digital Advertising, Social Networks

Challenge Period
---
The Fall 2014 Data Science Challenge runs October 11, 2014 through January 21, 2015.


Use AWS EC2 Machine of Spark Lab
================================

Description of EC2 machine see: https://github.com/comsysto/driver-telematics-kaggle/tree/spark1.2

Connect to the machine:

	ssh -Y -i spark-ml-lab.pem ec2-user@ec2-54-154-1-233.eu-west-1.compute.amazonaws.com

I installed R 3.1.1 (not 3.1.2!) there.


Setup GoGrid Server
==================

I marked steps with errors that I couldn't complete with :x:.

Connect to the master via:

	ssh root@216.121.66.98

Root stuff
------

Setup a user:

	useradd cindylamm
	passwd cindylamm

Setup ssh connection via public key, uncomment the lines with RSAAuthentication and PubkeyAuthentication:
:x: (Still asks for password) 

	vim /etc/ssh/sshd_config 

Install git:

	yum install git

Note: I tried to install apt-get (to install with this bash-completion) and followed [this guide](http://everyday-tech.com/apt-get-on-centos/). However I only got to the stage `yum install apt` where I got the message `No package apt available.`. I did not try to revert anything I've done up to that point.

Instead I followed [these steps](http://unix.stackexchange.com/questions/21135/package-bash-completion-missing-from-yum-in-centos-6) ton install bash-completion:
:x: didn't work on AWS EC2
	
	# import key
	sudo rpm --import https://fedoraproject.org/static/0608B895.txt
	# Download the bash-completion RPM
	wget http://www.caliban.org/files/redhat/RPMS/noarch/bash-completion-20060301-1.noarch.rpm
	# Install the RPM
	sudo rpm -ivh bash-completion-20060301-1.noarch.rpm
	# Execute the command
	. /etc/bash_completion

Install virtual box following [this guide](http://www.if-not-true-then-false.com/2010/install-virtualbox-with-yum-on-fedora-centos-red-hat-rhel/):
:x: (Some kernel error)

	cd /etc/yum.repos.d/
	wget http://download.virtualbox.org/virtualbox/rpm/rhel/virtualbox.repo
	yum update
	# load GPG key for EPEL
	rpm --import https://fedoraproject.org/static/0608B895.txt
	# enable EPEL repo for this system
	rpm -ivh http://mirror.chpc.utah.edu/pub/epel/6/x86_64/epel-release-6-8.noarch.rpm
	yum install binutils gcc make patch libgomp glibc-headers glibc-devel kernel-headers kernel-devel dkms
	echo export KERN_DIR=/usr/src/kernels/`uname -r` >> ~/.bashrc
	yum install VirtualBox-4.3
		# error: KERN_DIR does not point to a directory 

Install vagrant:

	wget https://dl.bintray.com/mitchellh/vagrant/vagrant_1.7.2_x86_64.rpm
	rpm -i vagrant_1.7.2_x86_64.rpm
	which vagrant

Install R 3.1.2 following [this guide](https://ashokharnal.wordpress.com/2014/01/16/installing-r-rhadoop-and-rstudio-over-cloudera-hadoop-ecosystem-revised/):
	
	# check that epel is already added
	yum repolist

	# Note that rpmforge has an older version of R. Disable rpmforge by opening file below and changing the line `enabled = 1` to `enabled = 0`
	sudo vim /etc/yum.repos.d/rpmforge.repo

	sudo yum install R R-devel

Install R packages that are needed for RStudio Server and those that are needed to run the code of this challenge:
	
	sudo R
	# packages that RStudio Server require:
	install.packages(c("rJava", "Rcpp", "RJSONIO", "bitops", "digest", "functional", "stringr", "plyr", "reshape2"))
	# packages that the challenge code requires
	install.packages(c("ggplot2", "caret", "randomForest", "glmnet", "knitr", "doMC", "doParallel", "e1071", "jsonlite", "Deducer"))
	source("http://www.bioconductor.org/biocLite.R")
	biocLite("limma")

:x: I couldn't install "Deducer" package on AWS EC2 because of error:
	
	X11 connection rejected because of wrong authentication.
	Error : .onLoad failed in loadNamespace() for 'iplots', details:
  		call: .jnew("org/rosuda/iplots/Framework")
  		error: java.lang.InternalError: Can't connect to X11 window server using 'localhost:10.0' as the value of the DISPLAY variable.

Install RStudio Server:

	echo 'export LC_ALL="en_US.UTF-8"' >> ~/.bashrc
	wget http://download2.rstudio.org/rstudio-server-0.98.490-x86_64.rpm
	yum install –nogpgcheck rstudio-server-0.98.490-x86_64.rpm

	# access RStudio server
	rstudio-server start
	rstudio-server stop
	

User stuff
-------

Setup ssh connection for user cindylamm:
:x: (Still asks for password) 
	
	# run from local machine
	cat ~/.ssh/id_rsa.pub | ssh cindylamm@216.121.66.98 "mkdir ~/.ssh; cat >> ~/.ssh/authorized_keys; chmod 600 ~/.ssh/authorized_keys"
	ssh cindylamm@216.121.66.98
	
Setup working environment for user cindylamm:
	
	# run from local machine
	cat ~/.bashrc_cloud | ssh cindylamm@216.121.66.98 "cat >> ~/.bashrc"
	scp ~/.alias_cloud cindylamm@216.121.66.98:~/.alias
	scp ~/.gitconfig cindylamm@216.121.66.98:~/.gitconfig
	scp ~/.gitconfig.user cindylamm@216.121.66.98:~/.gitconfig.user
	
Install the [Data Science Toolbox](http://datascienceatthecommandline.com/) to have R ready:
:x: (VirtualyBox not installed!) 

	mkdir -p ~/work/my_ds_toolbox
	cd ~/work/my_ds_toolbox
	vagrant init data-science-toolbox/data-science-at-the-command-line
	vagrant up

Get the challenge code

	cd ~/work
	git clone https://github.com/comsysto/ds_challenge_fall2014.git




