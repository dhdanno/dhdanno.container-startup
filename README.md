# dhdanno.container-startup
Playbook role for automatically starting your compose based docker services by adding them to the relevant service manager for your OS

This allows docker-compose application stacks to be handled as native OS services with regard to state maintenance and allows them to auto-start on boot.

## Dependencies
ansible

python

docker

docker-compose

## How it works
Input is a dictionarty of service names and respective path

- We then generate the appropriate files
- systemctl enable {{ service_name }}
- systemctl start {{ service_name }}


## Usage
```
  roles:
  - {
     role: dhdanno.container-startup,
     services: [
       {  name: 'myservice1',
          path: '/data/<myservicepath>'
       },
       {
         name: 'awesomeness.com',
         path: '/data/awesomeness.com'
       }
     ]
    }
```

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
