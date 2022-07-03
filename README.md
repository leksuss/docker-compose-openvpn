## docker-compose-openvpn

Based on [kylemanna/docker-openvpn](https://github.com/kylemanna/docker-openvpn)

### Requirements

* [Docker](https://docs.docker.com/engine/install/) with Compose
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


#### Generate new cert without password

```
make new username=example
```

#### Generate new cert with password

```
make new_cert_withpass username=example
```

Copy client_configs/example.ovpn to your local machine and use them in OpenVPN client.


#### Start VPN server

```
make up
```

## Additional management commands


### Stop VPN server

```
make stop
```


### View the status

```
make ps
```

### Revoke cert

```
make revoke username=example
```


## Useful tips

### Multiply users via one configuration

This OpenVPN configuration don’t allow connect multiply users via one configuration file by default. If you need to change this behavior, just add this line in server config file:


```
# vim ~/docker-compose-openvpn/openvpn-data/conf/openvpn.conf
duplicate-cn
```

### Routing only internal traffic in VPN

With default OpenVPN configuration file all traffic pass through the OpenVPN server. You can change it by deleting `redirect-gateway def1` from **client config file** and adding this settings:

```
route-nopull
route 10.128.0.0 255.255.255.0
```
You should change IP range and IP mask in `route` settings to yours.
