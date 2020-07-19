version: '3.5'

services:

        mattermost:
                image: mattermost/mattermost-preview
                container_name: mattermost
                ports:
                        - "8065:8065"
                        - "80:80"
                        - "443:443"
                volumes:
                        - /root/docker/mattermost:/mm/*:rw
                        - /etc/localtime:/etc/localtime:ro
                        - /root/docker/mattermost/mysql:/var/lib/mysql:rw
