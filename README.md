# dhdanno.container-startup
Playbook role for automatically starting your compose based docker services

## Requirements?
Tell us about your containers and we will create an appropriate startup script which composes that file

## How it works
Input is an array of service names and respective path

- We then generate the appropriate file
- systemctl enable {{ service_name }}
- systemctl start {{ service_name }}



### systemd
1. creates the service file in /etc/systemd
2. set it to startup
```
[Unit]
Description={{ service_name }} Startup Script
After=docker.service
Requires=docker.service

[Service]
Type=simple
ExecStart=/usr/local/bin/docker-compose up
ExecStop={{ compose_app_path }}/docker-compose down
Restart=always
WorkingDirectory={{ compose_app_path }}

[Install]
WantedBy=multi-user.target
```

### sysinitv
TODO

### upstart
TODO
