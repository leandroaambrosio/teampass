Hello,<br>
I created this project because I wanted to make a better Docker image for the TeamPass project. The Docker image is written from 0 and the code is published here in this GitHub repository, but the TeamPass project after the 1 run is automatically downloaded from the GitHub official repository. https://github.com/nilsteampassnet/TeamPass
<br><br>
<b>About TeamPass:</b><br>
TeamPass is an open-source password storage solution, that can be hosted on any Linux server including shared hosting servers or even Docker containers. Thanks to TeamPass you can keep your passwords secure and in your data center, if you need to generate some passwords, then using TeamPass you can do that in several difficulty levels. Also if you work in a team, then it is easy to share passwords among team members and keep track of who has accessed, changed, and deleted what kind of password. Also for organizations you can have several departments where only that department user group can access passwords that are meant for them and for each department you can even assign a manager. Teampass can be connected with LDAP and OpenLDAP. This is a great secure way of storing, sharing, and organizing your passwords + it has also a knowledge base that not only encrypts passwords but also attachments with an additional layer of security more information you can find in my video: https://youtu.be/eXieWAIsGzc from my point of view it is a great tool and best part it is free to use.
<br>
<br>
To run the TeamPass without MySQL container, execute the following:<br>
Replace the domain name from mysubdomain.domain.com with your domain or subdomain and also replace somemail@somedomainmail.com with your e-mail where Let's encrypt if you are using it to send information reminders about certificate expiration
<br>
```
docker run --name mysubdomain.domain.com --restart always --publish-all -p 8080:80 -p 4433:443 --hostname=mysubdomain.domain.com -e VIRTUAL_HOST=mysubdomain.domain.com -e LETSENCRYPT_EMAIL=somemail@somedomainmail.com -e LETSENCRYPT_HOST=mysubdomain.domain.com -d leandroaambrosio/teampass
```
<br>
```
version: '3.8'

services:
  teampass:
    image: leandroaambrosio/teampass
    container_name: teampass
    restart: always
    ports:
      - "8080:80"
      - "4433:443"
    hostname: passd.lasolucoes.net
    environment:
      VIRTUAL_HOST: mysubdomain.domain.com
      LETSENCRYPT_EMAIL: somemail@somedomainmail.com
      LETSENCRYPT_HOST: mysubdomain.domain.com
    depends_on:
      - mysqldb
    links:
      - mysqldb
    networks:
      mynetwork:
        ipv4_address: 192.168.32.3  # Assigning the fixed IP address to teampass service
    volumes:
      - teampass_data:/var/www/html/teampass  # Adjust the path inside the container as necessary

  mysqldb:
    image: leandroaambrosio/mysql:5.7
    container_name: teampass_db
    restart: always
    environment:
      MYSQL_USER: 'teampassdb'
      MYSQL_DATABASE: 'teampassdb'
      MYSQL_ROOT_PASSWORD: 'Senha'
      MYSQL_PASSWORD: 'senha'
      TZ: 'America/Sao_Paulo'
    networks:
      mynetwork:
        ipv4_address: 192.168.32.2  # Specify the fixed IP address here
    volumes:
      - mysql_data:/var/lib/mysql

volumes:
  teampass_data:
  mysql_data:

networks:
  mynetwork:
    driver: bridge
    ipam:
      config:
        - subnet: 192.168.32.0/24
```
<br><br>
Siga para análises mais interessantes sobre Infraestrutura, Cloud e Automation<br>
Docker Hub image: https://hub.docker.com/r/leandroaambrosio/teampass <br>

