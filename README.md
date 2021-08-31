# reverse-proxy
*reverse-proxy*, as the name says, is a flexible reverse proxy setup running on [Docker](https://docs.docker.com/) that can be configured to work with multiple services.

This reverse proxy implementation is based on [Traefik](https://doc.traefik.io/), which offers a *dashboard* to check its status as well as *whoami* and *ping* services. These are accessible at:
```
HOST/dashboard/
HOST/ping
HOST/whoami
```
Where `HOST` is the hostname.
Notice the trailing forward slash for the dashboard, this is required.

A dedicated reverse proxy has the advantage of working as a stand-alone, independent component of a more complex setup.

The reverse proxy directly handles TLS encryption, removing any need for the services behind it to take care of encryption.


# Setup
As the first step, clone the repository:
```
git clone git@github.com:guescio/reverse-proxy.git
cd reverse-proxy
```

Second, set `HOST`, `EMAIL` and `IPWHITELIST` in `.env`:
  - `HOST`: hostname;
  - `EMAIL`: email address to get a TLS certificate from Let's Encrypt;
  - `IPWHITELIST`: allowed IPs (CIDR notation).

Now create a file with authentication credentials (`.auth`) by running the following command in a terminal:  
```
htpasswd -Bc .auth USERNAME
```
These credentials will be used to access the dashboard and any other service that will require it.

Finally create a docker network named `reverse-proxy`:
```
docker network create reverse-proxy
```

That's it. Now get the reverse proxy up and running:
```
docker-compose up -d
```

Check the status of the reverse proxy and its services by pointing a browser to `HOST/dashboard/` and providing the authentication credentials chosen during the setup.
Once again, notice the trailing forward slash for the dashboard, this is required.


# Additional services

Any additional Docker service can be configured to work with this reverse proxy setup by using the same `reverse-proxy` Docker external network and telling Traefik about the new service by using the `traefik.enable` label:
```
- traefik.enable=true
```

Further Traefik parameters can be set using the relevant labels, similarly to what is done for the dashboard, ping and whoami services here.


# Documentation

- https://docs.docker.com/
- https://doc.traefik.io/traefik/
- https://doc.traefik.io/traefik/providers/docker/
