# Vagrant Build Environment For ChatScript on Linux

This Vagrantfile provides a quick mechanism to get a Linux environment configured to build/test any of the Linux
ChatScript binaries. This allows developers on Mac and Windows to easily generate and test the Linux binaries, including the Postgres and Mongo versions. Additionally, Linux users can build the binaries without needing the dependencies on their development machine.

## Pre-requisites

1. Install VirtualBox - https://www.virtualbox.org/wiki/Downloads
2. Install Vagrant - https://www.vagrantup.com/
3. (Optional) Install the vagrant-vbguest plugin - https://github.com/dotless-de/vagrant-vbguest
   1. This is optional as the base image should have the required Virtualbox Guest utilities installed. However, this plugin can fix minor Host/Guest version conflicts which should reduce friction.


## Running

The machine is configured to install all the required dependencies on first boot.

Running the following command from the command line (in this directory):

```
vagrant up
```

This will probably take a while the first time you do this, as it will download a base image for the VM (currently CentOS based. Subsequest runs will be much faster.

## Compiling - using helpers

Some helper scripts are installed into the VM by default to allow you to easily compile any of the versions you want.

Example (run from the host computer):

```
vagrant ssh -c build_pg
```

Will ssh into the VM and build the ChatScript Postgres binaries (which will then be available on the Host machine).

## Compiling - Manually

If you are updating code and testing, you will want to manually compile (to avoid the `make clean` in the utility scripts). You will need to SSH into the VM and change to the `ChatScript/SRC` directory.

To SSH to the instance:
```
vagrant ssh
```

Example:

```
~/public_forks/ChatScript/NON-WINDOWS NON-C/VAGRANT/LINUX_BUILD_ENVIRONMENT vagrant ssh
Last login: Thu Apr  6 05:49:13 2017 from 10.0.2.2
[vagrant@localhost ~]$ cd ~/ChatScript/SRC/
[vagrant@localhost SRC]$ make pgserver
************ LINUX VERSION ************
g++ constructCode.o evserver.o csocket.o cs_ev.c dictionarySystem.o englishTagger.o factSystem.o json.o functionExecute.o english.o infer.o javascript.o jsmn.o markSystem.o mongodb.o os.o outputSystem.o patternSystem.o postgres.o privatesrc.o scriptCompile.o spellcheck.o secure.o systemVariables.o tagger.o testing.o textUtilities.o tokenSystem.o topicSystem.o userCache.o userSystem.o variableSystem.o mainSystem.o -L/usr/lib64 -lrt -lcurl  --verbose -lpq -pthread -DLOCKUSERFILE=1 -DEVSERVER=1 -DEVSERVER_FORK=1 -DDISCARDMONGO=1 -DDISCARDMYSQL=1 -Ievserver -Ipostgres -Icurl -o ../BINARIES/ChatScriptpg
... truncated logs...
```

Note that Vagrant will synchronise the changes on the host system and the guest (VM), so you can edit the files on the host (Windows, Mac or Linux) and compile them immediately on Linux (in this case CentOS).


# Postgres Testing

An instance of Postgres is installed and configured on the VM to facilitate testing of the Postgres integration. A helper script exists to allow the server to be run with the required configuration.

Example: from the Host machine
 ```
 vagrant ssh -C pgserver
 ```

 From the Guest (VM) after `vagrant ssh`:
 ```
 [vagrant@localhost ~]$ pgserver 
 evcalled pid: 15653
 ChatScript EVSERVER Version 6.91 pid: 15653 64 bit LINUX compiled Apr  7 2017 00:41:09 host=local
 WordNet: dict=201381  fact=85709  stext=12704348 Dec07'16-14:44:33
     Dictionary building disabled.
```

## Host access to the database

By default, port 15432 on the Host is forwarded to the Postgres port on the client. You should be able to connect to the Postgres Database from the Host using the following details:

```
host: localhost
port: 15432
database: users
username: vagrant
password: vagrant
```


