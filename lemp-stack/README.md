# LEMP on Ubuntu 18.04

This playbook will install a LEMP environment on an Ubuntu 18.04 machine, as explained in the guide on
[How to Use Ansible to Install and Set Up LEMP on Ubuntu 18.04](https://www.digitalocean.com/community/tutorials/how-to-use-ansible-to-install-and-set-up-lemp-on-ubuntu-18-04).
A virtualhost will be created with the options specified in the `vars/default.yml` variable file.

## Settings

- `mysql_root_password`: the password for the MySQL root account.
- `http_host`: your domain name.
- `http_conf`: the name of the configuration file that will be created within nginx.
- `http_port`: HTTP port, default is 80.


## Running this Playbook

Quick Steps:

### 1. Obtain the playbook
```shell
git clone https://github.com/do-community/ansible-playbooks.git
cd ansible-playbooks/lemp_ubuntu1804
```

### 2. Customize Options

```shell
nano vars/default.yml
```

```yml
#vars/default.yml
---
mysql_root_password: "mysql_root_password"
http_host: "your_domain"
http_conf: "your_domain.conf"
http_port: "80"
```

### 3. Run the Playbook

```command
ansible-playbook -l [target] -i [inventory file] -u [remote user] playbook.yml
```

For more information on how to run this Ansible setup, please check this guide: [How to Use Ansible to Install and Set Up LEMP on Ubuntu 18.04](https://www.digitalocean.com/community/tutorials/how-to-use-ansible-to-install-and-set-up-lemp-on-ubuntu-18-04).

## Other

### MySQL

I had some trouble with the initial `mysql` installation. I kept getting the following error:

```terminal
ERROR 1045 (28000): Access denied for user 'root'@'localhost' (using password: YES)
```

I can't be sure how I got past it, but it was a combination of manuall attempting to login to `mysql` using `root` and `password` as the default root user login. Then, the playbook ran successfully. It appears that `root` is the default password when installing `mysql` for the first time headlessly, but then i had to make the `.my.cnf` password entry match what I had in the `default.yml` variable file.

### Nginx

I need to add a task to change the ownership of the doc root so that nginx will serve the 'php.info' page instead of forcing the browser to download it.

```bash
sudo chown -R $USER:$USER /var/www/html
```
