# Vagrant How To Guide

#### Requirements

Install the following

- [Vagrant](https://www.vagrantup.com/docs/installation)
- [Virtual Box](https://www.virtualbox.org/)



#### Quick Start

Once everything is installed move to a working directory where you would like to store your Vagrant files (e.g. Vagrant Files). Once there create a sub-folder for your new Vagrant file (Debian Sandbox).

```bash
$ mkdir -p "Vagrant Files/Debian Sandbox";
$ cd "Vagrant Files/Debian Sandbox"
```

Now that the workspace is complete lets create a Vagrant. With this tutorial we will use Debian but there are many more [boxes](https://app.vagrantup.com/boxes/search) to choose from.

| Operating System | Vagrant Code Name |
| ---------------- | ----------------- |
| Ubuntu           | ubuntu/trusty64   |
| Debian           | debian/jessie64   |
| CentOS           | centos/7          |

```bash
$ vagrant init debian/jessie64
```

A vagrant file will have appeared within the folder. This file is scripted in Ruby. This file lets you configure the startup of your vagrant including, network config, ssh settings, install software, etc. Lets leave it default for now.

Something that should be mentioned is that persistent storage is possible with Vagrant but is known to be sometimes difficult.

#### Install A Box

Simply typing the following command will install the box specified into Vagrant storage ready for use across all users. This can save time spooling up new Vagrants.

```bash
$ vagrant box add debian/jessie64
```

#### On / Off Switch

While inside your "Vagrant Files/Debian Sandbox" folder run.

```bash
$ vagrant up
```

Once complete some errors might spill out but that might just be SSH issues. Use the following command too see the status.

```bash
$ vagrant global-status
```

To close the box down simply write.

```bash
$ vagrant destroy
```

Use this command to see a list of available commands and options.

```bash
$ vagrant -h
```



#### Interacting With The Box

Run the following command to interact with your Vagrant box

```bash
$ vagrant ssh
```



#### Provisioning

Open the Vagrant file in your favourite text editor. Uncomment the following sections and add the code to the provision section.

```ruby
config.vm.network "forward_port", guest: 80, host: 8080

config.vm.provision "shell", inline: <<-SHELL
	apt update
	apt install -y apache2 git
	git clone "https://github.com/albalamia/vagrant-tutorial"
	cp vagrant-tutorial/index.html /var/www/index.html
	sudo systemctl start apache2
SHELL
```

Now we need to reboot our vagrant so it takes on the new settings.

```bash
$ vagrant destroy
$ vagrant up
$ vagrant provision
```

Open a browser on your host machine and type "localhost:8080" into your browser to see your website.

