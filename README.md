## docker-compose-openvpn

Based on [kylemanna/docker-openvpn](https://github.com/kylemanna/docker-openvpn)

### Requirements

* [Docker](https://docs.docker.com/engine/install/)
* [Docker-Compose](https://docs.docker.com/compose/install/)
* Make

### Start


#### Generate configs

All commands should be run with superuser priveleges

Create new config with name or IP host:

```
make genconfig host=vpn.example.com
```

And then:

```
make initpki
```

*Please save entered password. You'll need it for certificate management.*


#### Generate new cert

```
make new username=example
```

Copy client_configs/example.ovpn to your local machine and use them in OpenVPN client.


#### Start VPN server

```
make up
```

### Additional management commands


#### Stop VPN server

```
make stop
```


#### View the status

```
make ps
```

#### Revoke cert

```
make revoke username=example
```
