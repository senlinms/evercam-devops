## Evercam Development Environment Setup

If you want to configure evercam server without vagrant and ansible then follow the instructions provided here: https://github.com/evercam/evercam-devops/wiki/Evercam-Configuration

Otherwise, follow the instructions bellow.

This repo is used for automated provisioning and configuration of developer and production environments needed to run Evercam system. This README will explain how to setup the developer environment.

When finished with the README, you will have a Vagrant Virtual Machine running Ubuntu 14.04 LTS with the following software:

* Ruby
* Elixir
* Erlang
* Node.js
* Redis
* Memcached
* Postgres with Postgis
* Nginx with Nginx-RTMP

### Installation

These instructions assume that you're running either Linux or OS X (Ansible doesn't work on Windows).

##### Install Host dependencies

* [Virtualbox](https://www.virtualbox.org/wiki/Downloads)
* [Vagrant](http://www.vagrantup.com/downloads.html)
* [Python](https://www.python.org/downloads/)
* [Ansible 2.0.1.0](http://docs.ansible.com/ansible/intro_installation.html)
* [Git](http://git-scm.com/downloads)

##### Clone this repository and it's submodules

```
git clone https://github.com/evercam/evercam-devops.git && cd evercam-devops
git pull && git submodule init && git submodule update && git submodule status
git submodule foreach --recursive "git checkout master && git pull"
```

##### Install Ansible dependencies

```
sudo ansible-galaxy install -r ansible/requirements.yml --force
```

##### Copy example `.env` file into required directories

```
cp .env evercam-dashboard/ && cp .env evercam-models/ && cp .env evercam-media/
```

##### Run Vagrant and ssh into the VM

```
vagrant up && vagrant ssh
```

Grab a cup of your favorite beverage, this is going to take some time...

### Testing

```
cd /vagrant/evercam-dashboard
bundle exec rspec --pattern 'c*/*_spec.rb,h*/*_spec.rb'

cd /vagrant/evercam-media
mix test
```

### Usage

Now start the Media server:

```
cd /vagrant/evercam-media
EVERCAM_LOCAL=true mix phoenix.server
```

And in another terminal tab/window start the Dashboard server:

```
cd /vagrant/evercam-dashboard
EVERCAM_LOCAL=true bundle exec rails server -b 0.0.0.0
```

And in another terminal tab/window start the Phony camera for offline tests:

```
cd /vagrant/phony_camera
EVERCAM_LOCAL=true node index
```

Open [http://localhost:3000/](http://localhost:3000/) in a browser and you'll see the dashboard of the Next Big Thing&trade;

### Documentation

**All** of the Evercam documentation can be found here: https://github.com/evercam/evercam-media/wiki
