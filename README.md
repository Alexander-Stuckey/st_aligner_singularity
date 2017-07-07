# A Singularity container for the ST aligner WebApp

This document details how to get set up and running with the ST aligner WebApp inside a singularity container.
Singularity can be found here: http://singularity.lbl.gov/

To install Singularity, follow the instructions on the following webpage relevant to your operating system:
- Linux		: http://singularity.lbl.gov/install-linux
- Mac OSX	: http://singularity.lbl.gov/install-mac
- Windows 	: http://singularity.lbl.gov/install-windows

Once Singularity has been installed, you need to create a container:

singularity create --size 8192 st_aligner.img

Once the container has been created, it can be bootstrapped with the bootstrap recipie provided. This command needs to be run as root in order to install all the components into the container.

sudo singularity bootstrap st_aligner.img st_aligner_bootstrap

Once the bootstrap procedure has finished (may take around 30 minutes), you are ready to run the st_aligner.

singularity run st_aligner.img

Accessing the webserver and other considerations
------------------------------------------------

If you are running the container outside a virtual machine, you should be able to connect to it with the following address in your web browser:

localhost:8080

If you are running the container inside a virtual machine then a few additional steps are necessary.
The following uses vagrant VM as an example.

Edit your Vagrantfile so that the following lines are uncommented:
- config.vm.network "forwarded_port", guest: 80, host: 8080, host_ip: "127.0.0.1"
- config.vm.network "private_network", ip: "192.168.33.10"

config.vm.provider "virtualbox" do |vb|

	vb.memory = "8192"

end

This will allow you to connect to the webserver on the ip specified, on port 8080. Enter the following in your web browser:
192.168.33.10:8080

The st_aligner requires around 8GB of RAM. Less RAM is required if the images are reduced in size prior to aligning them. If you get the message "An error has occurred" while uploading images it is likely that the web server has run out of RAM. Either resize the input images, or allocate more RAM to your virtual machine (or both).
