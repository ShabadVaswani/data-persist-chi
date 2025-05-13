# Persistent storage on Chameleon

Note that only the docker compose etl was updated in this project to make it convinient for our XRAY Project

The commmands to follow are outlined below:


curl https://rclone.org/install.sh | sudo bash

run on node-persist
this line makes sure user_allow_other is un-commented in /etc/fuse.conf
sudo sed -i '/^#user_allow_other/s/^#//' /etc/fuse.conf

mkdir -p ~/.config/rclone
nano  ~/.config/rclone/rclone.conf

[chi_tacc]
type = swift
user_id = YOUR_USER_ID
application_credential_id = APP_CRED_ID
application_credential_secret = APP_CRED_SECRET
auth = https://chi.tacc.chameleoncloud.org:5000/v3
region = CHI@TACC


rclone lsd chi_tacc:

docker compose -f ~/data-persist-chi/docker/docker-compose-etl.yaml run extract-data
docker compose -f ~/data-persist-chi/docker/docker-compose-etl.yaml run load-data
docker volume rm processed-etl_processed


run on node-persist
sudo mkdir -p /mnt/object
sudo chown -R cc /mnt/object
sudo chgrp -R cc /mnt/object

rclone mount chi_tacc:object-persist-project44-1 /mnt/object --read-only --allow-other --vfs-cache-mode=full --dir-cache-time=72h --swift-fetch-until-empty-page --daemon
