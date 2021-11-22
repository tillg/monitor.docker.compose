# Monitoring on Docker

A docker-compose based setup of a monitoring  - fully deployed via Ansible.

## Configuring - Deploying - Running 

**Prerequites:** On your machine you need git and Ansible (I use 4.4.0 in Aug 2021). And a server on which you can install & run the setup. The server just needs to have SSH and Python installed. 

Configure your setup by following these steps:

* Create an inventory file per environment. For example one called `TEST.yml`
* Enter the server contact details (like described [here](https://docs.ansible.com/ansible/latest/user_guide/intro_inventory.html#inventory-basics-formats-hosts-and-groups)). Make sure in the inventory you set your `env` variable, i.e. `env: TEST`
* Create a corresponding variable file, i.e. `vars/TEST.yml`
* Fill in the required variables in `TEST.yml`, i.e.
  
  ```yaml
  server_url: "test.myserver.com" 
  certbot_use_local_ca: 1
  ```

Then execute ` ansible-playbook setup.yml`. Then be patient - and eventually your Monitoring will be up & running 😜

For a list of all the configuration variables, have a look in `vars/all.yml` or for some configuration explkanations [here](##configuration).

If you are interested in running local tests, see [Test with Multipass](test_with_multipass.md).
## Configuration

All configuration is done via the environment variable files. To check what vareiables are available and what default values they are set to, check `vars/all.yml`. Make sure you do not change `all.yml`, but rather override them in your environment variable file, i.e. `vars/TEST.yml`.

Some variables and options that need explanation are described below.

### TODO: Backup & Restore

The setup containes a backup tool wrapped in a docker container: We use the [backup container from Tim Bennet](https://github.com/bennetimo/ghost-backup). 

In order to perform a backup of an existing environment (docker-based, that is)


## TO DO & DONE

Things on my to do list:

- Create proper routes, naming, accesss logic
- Add monitoring using ELK or Grafana for my servers (ghost PROD & TEST, MONITOR) with metrics:
  - Disk space
  - Memory usage
  - CPU usage
  - Certificates validity
- Add AWS costs monitoring
- Log files
  - Count errors
  - Count warnings
- Send emails when threshold are taken
### DONE 

* 2021-11-22: First version works witzh uptime-kuma, but services are still available w/o security
* 2021-11-17: Started the project based on my ghost on Docker setup
## Reading / Sources / Tech background

* [Uptime Kuma](https://github.com/louislam/uptime-kuma): A lightweight uptime-checking system
* 