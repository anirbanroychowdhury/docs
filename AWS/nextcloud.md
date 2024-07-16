Setting Up Own Nextcloud on AWS
========================

# Aim:  
- [ ] Setup VPC
- [ ] set up security
- [ ] Setup nextcloud on ec2

## Step1: setting up nextcloud on EC2 
- we'll start with this since its much more fun this way
- things may get mixed up
- first setup a proper vpc aside form the default vpc in your select AWS zone.
- The vpc setup should split the VPC in 2 public and private subnets
- Create a t2.micro instance in your selected public VPC
- You can create your EC2 in a private subnet but then you will not able to access it over public internet
- Either we use cloudflalare zero trust to tunnel into the private subnet and then acces it, or the above
- Make sure your security group has the following rules
    - default ssh on port 22 to allow ssh connection to Ec2
    - open up the following ports for nextcloud to be able to talk to container
        - 3478/TCP
        - 3478/UDP
    - open up 8080 on 0.0.0.0/0 to be able to access nexcloud GUI
- Get the public IP from EC2 dashboard
- Visit `http://<public_ip_address_ec2>:8080`and you will be landed on nextcloud config gui
- You might need the admin password


## Helpful docker commands

| Action |Docker command|
|------------|----------|
|Install Docker|`curl -fsSL https://get.docker.com | sudo sh`|
| Install NextCloud-AIO|`sudo docker run --init  --sig-proxy=false --name nextcloud-aio-mastercontainer --restart always --publish 80:80 --publish 8080:8080 --publish 8443:8443 --volume nextcloud_aio_mastercontainer:/mnt/docker-aio-config --volume /var/run/docker.sock:/var/run/docker.sock:ro nextcloud/all-in-one:latest`|
|Check running container|`docker ps`|
|Get docker container name| `sudo docker ps --format {{.Names}}` |
| Get nextcloud-aio portal passoword| `sudo docker exec nextcloud-aio-mastercontainer grep password /mnt/docker-aio-config/data/configuration.json`|

### Links
- AIO github - https://github.com/nextcloud/all-in-one