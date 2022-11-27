# Deploying the website on a single machine

## prerequisite
* your domain name management service
* lower your ttl settings on your dns records to as low as possible, raise it back after all the steps are done and the website is running fine.
* git
* docker
* docker-compose 


### Steps
1. Clone the repo

2. Clone the repo of the website
```
git clone <website git link>
```
```
Your directory should look like this: 
├── traefik-deploy
│   ├── traefik
│   │   ├── docker-compose.yml 
│   ├── docker-compose.yml
│   ├── webiste
```

3. open port 443, 80 on the machine (and ssh if youre using SSH to access the machine)
```
# for ubuntu
sudo ufw allow https
sudo ufw allow http
# sudo ufw allow ssh
sudo ufw enable 
sudo ufw reload
```

4. add an A record on your domain name management
```
A your-domain.com <this machine's ip address>
```


5. Go to the docker-compose file in the traefik folder. \
Change the ```certificatesResolvers.myresolver.acme.email``` to the domain name's registered email. \
Then start the traefik service, which will configure the proxy automatically.
```
cd traefik
docker network create traefik-public
docker-compose up -d
```

6. Go up one level
```
cd ..
```

7. Go to the docker-compose file in that level. Replace your domain name in  ```traefik.http.routers.app.rule=Host(`your-domain`)```. Then start the app service, then the traefik service will connect the app to proxy automatically.
```
docker-compose up -d
```