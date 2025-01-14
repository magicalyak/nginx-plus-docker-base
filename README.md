# Nginx Plus Base

A NGINX Plus base dockerfile and configuration for testing

## File Structure

```
etc/
└── nginx/
│    ├── conf.d/ **ADD your HTTP/S configurations here**
│    │   ├── default.conf ...........Default Virtual Server configuration with example rewrite rules (Delete if needed)
│    │   └── dummy_servers.conf  ....Dummy loopback web servers reponds with plain/text
│    │   └── upstreams.conf..........Upstream configurations
│    │   └── stub_status.conf .......NGINX Open Source basic status information available http://localhost/nginx_status only
│    │   └── status_api.conf.........NGINX Plus Live Activity Monitoring available on port 8080 - [Source](https://gist.github.com/nginx-gists/a51341a11ff1cf4e94ac359b67f1c4ae)
│    ├── nginx.conf .................Main NGINX configuration file with global settings
│    ├── includes/
│    │   └── proxy_headers/
│    │       └─── proxy_headers.conf .....Best practice headers to pass to backend proxied servers
│    └── stream.conf.d/ **ADD your TCP and UDP Stream configurations here**
└── ssl/
    └── nginx/
    │   ├── nginx-repo.crt...........NGINX Plus repository certificate file (**Use your own license here**)
    │   └── nginx-repo.key...........NGINX Plus repository key file (**Use your own license here**)
    ├── example.com.key...........Self generated certificate for testing
    └── example.com.crt...........Self signed private key for testing
html/
└──demo-index.html ..........Status page for dummy servers
```

## Build Docker container

 1. Copy and paste your `nginx-repo.crt` and `nginx-repo.key` into `etc/ssl/nginx` directory first
   ```bash
   $ cp ~/nginx-repo.* etc/ssl/nginx/
   ```

 2. Build an image from your Dockerfile:
    ```bash
    # Run command from the folder containing the `Dockerfile`
    $ docker build -t nginx-plus-alpine .
    ```
 3. Start the Nginx Plus container, e.g.:
    ```bash
    # Start a new container and publish container ports 80, 443 and 8080 to the host
    $ docker run -d -p 80:80 -p 443:443 -p 8080:8080 nginx-plus-alpine
    ```

    **To mount local volume:**

    ```bash
    docker run -d -p 80:80 -p 443:443 -p 8080:8080 -v $PWD/etc/nginx:/etc/nginx nginx-plus-alpine
    ```

 4. To run commands in the docker container you first need to start a bash session inside the nginx container
    ```bash
    docker ps # get Docker IDs of running containers
    sudo docker exec -i -t [CONTAINER ID] /bin/bash
    ```
   
 5. To open logs
    ```bash
    docker ps # get Docker IDs of running containers
    sudo docker logs -f [CONTAINER ID]
    ```