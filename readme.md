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

## Configuration

All configuration is done via the environment variable files. To check what vareiables are available and what default values they are set to, check `vars/all.yml`. Make sure you do not change `all.yml`, but rather override them in your environment variable file, i.e. `vars/TEST.yml`.

Some variables and options that need explanation are described below.


## TO DO & DONE

Things on my to do list:

### TODO: Private access

Give the site private access, using [OAuth2 Proxy](https://oauth2-proxy.github.io/oauth2-proxy/) and Google login. The structure of the site should be:

* `monitoring.grtnr.de/` shows a top level status site that also contains links to more detailed monitoring info
* `monitoring.grtnr.de/details/*` contains all the detailed monitoring data

#### Reading

* A good and thorough explanation of how nginx and Letsencrypt interact within a docker setup: [How to set up an easy and secure reverse proxy with Docker, Nginx & Letsencrypt](https://medium.com/free-code-camp/docker-nginx-letsencrypt-easy-secure-reverse-proxy-40165ba3aee2)
* May be I want to use [Statping](https://github.com/statping/statping)
* A complete guide: [Add Google Authentication to any Website using Nginx and Oauth Proxy](https://dev.to/ahmedmusaad/add-google-authentication-to-any-website-using-nginx-and-oauth-proxy-259l)
* [Validating OAuth 2.0 Access Tokens with NGINX and NGINX Plus](https://www.nginx.com/blog/validating-oauth-2-0-access-tokens-nginx/#production-configuration)
* Another alternative to OAuth2 Proxy that I don't plan to use (mainly because the guides I found use OAuth Proxy): [Vouch](https://github.com/vouch/vouch-proxy)
* Two explanations on how to use Vouch: [Use nginx to Add Authentication to Any Application](https://developer.okta.com/blog/2018/08/28/nginx-auth-request) and [Enforce Google Authentication for Any Application with nginx and Vouch Proxy](https://medium.com/lasso/use-nginx-and-lasso-to-add-google-authentication-to-any-application-d3a8a7f073dd)
* The docker image I plan to use is from [Bitnami](https://hub.docker.com/r/bitnami/oauth2-proxy/)

### TODO: Backup & Restore

The setup containes a backup tool wrapped in a docker container: We use the [backup container from Tim Bennet](https://github.com/bennetimo/ghost-backup). 

In order to perform a backup of an existing environment (docker-based, that is)

### Further TODOs

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

* 2021-11-22: First version works with uptime-kuma, but services are still available w/o security
* 2021-11-17: Started the project based on my ghost on Docker setup
## Reading / Sources / Tech background

* [Uptime Kuma](https://github.com/louislam/uptime-kuma): A lightweight uptime-checking system
* 