
# vagrant-ansible-mongodb

Example of MongoDB sharded cluster with 3 database servers, 3 configuration servers and 1 query server.


## Prerequisites:

Download and install the following tools:

- [VirtualBox & Extension Pack](https://www.virtualbox.org/wiki/Downloads)
- [Vagrant](https://www.vagrantup.com/downloads.html)
- [Ansible](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html) 2.8+

## Running the code

- Status:
  ```bash
  -:$ vagrant status
  ```

- Instantiate servers:
   ```bash
   -:$ vagrant up --provision mongo{01,02,03,04}
   ```

> Note: ensure that `mongo04` is created after all other vms are created. 

## Ports Information

|  Server                |  Process Name  |  Port   |
| ---------------------- | -------------- | ------- |
|  Query Server          |  mongos        |  27017  |  
|  Database Server       |  mongod        |  27018  |
|  Configuration Server  |  mongoc        |  27019  |

## Servers Information

```text
# Query server:
 - 192.168.53.104

# Database servers:
 - 192.168.53.101
 - 192.168.53.102
 - 192.168.53.103

# Configuration servers:
 - 192.168.53.101
 - 192.168.53.102
 - 192.168.53.103
```

## Connect to the Query Server

  ```bash
  # Connect to the mongos instance in the query server
  -:$ vagrant ssh mongo04 -- -t "mongo --host 192.168.53.104 --port 27017"
  mongos>
  
  # Check shard status
  mongos> sh.status()

  # Exit
  mongos> quit()
  ```

