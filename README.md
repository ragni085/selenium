### Install Selenium Hub.

For this you need to have docker already installed. 

You can follow the following process to install docker.

Install SeleniumHub using docker, But in order to run component which is required we better run it with docker compose.

Run the following steps to install docker-compose and install selenium Hub with it. 

```
$ curl -s https://raw.githubusercontent.com/linuxautomations/labautomation/master/tools/docker/install-docker-ce.sh | sudo bash
$ sudo curl -L "https://github.com/docker/compose/releases/download/1.24.0/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
$ sudo chmod +x /usr/local/bin/docker-compose
$ docker-compose version 
```

Create a file `docker-compose.yml` with following content.

```
version: "3"
services:
  selenium-hub:
    image: selenium/hub:3.141.59-lithium
    container_name: selenium-hub
    restart: always
    ports:
      - "4444:4444"
  chrome:
    image: selenium/node-chrome:3.141.59-lithium
    restart: always
    volumes:
      - /dev/shm:/dev/shm
    depends_on:
      - selenium-hub
    environment:
      - HUB_HOST=selenium-hub
      - HUB_PORT=4444
  firefox:
    image: selenium/node-firefox:3.141.59-lithium
    restart: always
    volumes:
      - /dev/shm:/dev/shm
    depends_on:
      - selenium-hub
    environment:
      - HUB_HOST=selenium-hub
      - HUB_PORT=4444
```

Run the following command. 

```
$ docker-compose up -d
```

You can run the maven commands to test with selenium.

```
mvn clean install "-Dremote=true" "-DseleniumGridURL=http://IP_ADDRESS:4444/wd/hub" "-Dbrowser=Chrome" "-Doverwrite.binaries=true"
```
##
