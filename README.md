# Description  docker-nginx/Dockerfile

* In this demo, I created a Dockerfile to customize NGINX docker image based on Ubuntu. I have  including NGINX configurations under docker-nginx/nginx.conf and  docker-nginx/assets/conf.d/default.conf for server configuration. 

* The entrypoint.sh file is the script used as the entry point for container once created.  

* I removed the sites-available and sites-available folders from the image since I won't be using them

### Installation Guide 
1. install docker from the installation script [installation script](https://get.docker.com/) 
2. clone this repo 
3. ```cd docker-nginx ```
4. run this command ```docker build  -t image_name:tag .```
5. command to create a container using the new image ```docker run --name container_name -d -p 80:80 image_name:tag```
6. access nginx from the web browser by going to [localhost](http://localhost/)


### Push to docker hub 
* run this command ```docker tag 0303124918e6 nginx docker_name/image_name:tag_name``` then push the tagged image from your local docker console to docker hub

# Other commands
I am adding other commands for managing services and containers 

### Docker create service phpmyadmin
```
docker service create --name myadmin  —env PMA_HOST=mysql  —env MYSQL_ROOT_PASSWORD=Password —env PMA_PORT=3306 --publish 8080:80  phpmyadmin
```

### Docker create service mysql
```
docker service create --name mysql --hostname mysql --env MYSQL_ROOT_PASSWORD=Password --publish 3306:3306 mysql
```
### Docker create service wordpress
docker service create --name wordpress --hostname wordpress --publish 8080:80 --network name=network --env WORDPRESS_DB_HOST=mysql:3306 --env WORDPRESS_DB_USER=root --env WORDPRESS_DB_PASSWORD=Password --mount type=volume,source=wordpress,target=/var/www/html wordpress

### Docker create service nginx
```
docker service create --name nginx --hostname nginx  --publish 80:80 --publish 443:443  --network name=network  --mount type=volume,source=nginx_files,target=/usr/share/nginx/html/ --mount type=volume,source=nginx_configs,target=/etc/nginx/ image_name:tag 
```
### Docker network 
```
docker network create -d overlay network_name
```
### nginx restart 
```
docker exec -it container_ID service nginx reload
docker exec -it container_ID service nginx restart
docker exec -it container_ID service nginx status
```
### service update 
```
docker service update --mount-add type=volume,source=wordpress_volume,target=/var/www/html wordpress
docker service update --network-add name=network wordpress
docker service update --network-rm bridge network mysql
docker service update --publish-add published=80,target=80 nginx_server
docker service update --publish-rm published=8080,target=80 wordpress
```