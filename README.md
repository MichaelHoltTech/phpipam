# docker-phpipam

phpIPAM is an open-source web IP address management application. Its goal is to provide light and simple IP address management application.

phpIPAM is developed and maintained by Miha Petkovsek, released under the GPL v3 license, project source is [here](https://github.com/phpipam/phpipam)

Learn more on [phpIPAM homepage](http://phpipam.net)

![phpIPAM logo](http://phpipam.net/wp-content/uploads/2014/12/phpipam_logo_small.png)

## How to use this Docker image

### Mysql

Run a MySQL database, dedicated to phpipam

```bash
$ docker run --name phpipam-mysql -e MYSQL_ROOT_PASSWORD=my-secret-pw -v /my_dir/phpipam:/var/lib/mysql -d mysql:5.6
```

Here, we store data on the host system under `/my_dir/phpipam` and use a specific root password. 

### Phpipam 

```bash
$ docker run -ti -d -p 80:80 --name ipam --link phpipam-mysql:mysql michaelholttech/phpipam
```

We are linking the two containers and expose the HTTP port. 

### Specific integration (HTTPS, multi-host containers, etc.)

Regarding your requirements and docker setup, you've to expose resources. 

For HTTPS, run a reverse-proxy in front of your phpipam container and link it to. 

For multi-host containers, expose ports, run etcd or consul to make service discovery works etc. 

### Configuration 

* Browse to `http://<ip>[:<specific_port>]/`
* Step 1 : Choose 'Automatic database installation'

![step1](https://cloud.githubusercontent.com/assets/4225738/8746785/01758b9e-2c8d-11e5-8643-7f5862c75efe.png)

* Step 2 : Re-Enter connection information

![step2](https://cloud.githubusercontent.com/assets/4225738/8746789/0ad367e2-2c8d-11e5-80bb-f5093801e139.png)

* Note that these two first steps could be swapped by patching phpipam (see https://github.com/phpipam/phpipam/issues/25)
* Step 3 : Configure the admin user password

![step3](https://cloud.githubusercontent.com/assets/4225738/8746790/0c434bf6-2c8d-11e5-9ae7-b7d1021b7aa0.png)

* You're done ! 

![done](https://cloud.githubusercontent.com/assets/4225738/8746792/0d6fa34e-2c8d-11e5-8002-3793361ae34d.png)

### Docker compose 

You can create an all-in-one YAML deployment descriptor with Docker compose, like this : 

```yaml
ipam:
  image: michaelholttech/phpipam
  ports:
   - "80:80"
  links:
   - phpipam-mysql
phpipam-mysql:
  image: mysql:5.6
  environment: 
   - MYSQL_ROOT_PASSWORD=my-secret-pw
  volumes:
   - /my_dir/phpipam:/var/lib/mysql
```

And next :

```bash 
$ docker-compose up -d
```

### Notes

phpIPAM is under heavy development by the amazing Miha. 
To upgrade the release version, just change the `PHPIPAM_VERSION` environment variable to the target release (see [here](https://github.com/phpipam/phpipam/releases)) 
