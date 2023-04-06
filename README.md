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


### Set static IP for the certain client

Create file named like client's vpn config, but without `.ovpn` in this folder:
```
~/docker-compose-openvpn/openvpn-data/conf/ccd
```

If your client linux, insert in this file string
```
# for the linux clients
ifconfig-push 192.168.255.3 255.255.255.0
```
If your client windows, the local and remote VPN endpoints must exist within the same 255.255.255.252 subnet. This is a limitation of —dev tun when used with the TAP-WIN32 driver. What does that mean? That mean each windows client has separate subnet with VPN server and consist of 4 IP addresses:
```
192.168.255.0 network address
192.168.255.1 VPN server
192.168.255.2 VPN client
192.168.255.3 network broadcast adress
```
And every new VPN client on windows get's a new 4-range IP addresses:
```
192.168.255.4 network address
192.168.255.5 VPN server
192.168.255.6 VPN client
192.168.255.7 network broadcast adress
```
and etc.. Each windows client require 4 IP, you need 2 and 3. So, you should set in each file for every new windows client two IP addresses: VPN server IP and VPN client IP:
```
# for the windows client#1 
# ifconfig-push VPN-client-IP, VPN-server-IP 
ifconfig-push 192.168.255.2 192.168.255.1
```
```
# for the windows client#2
# ifconfig-push VPN-client-IP, VPN-server-IP 
ifconfig-push 192.168.255.6 192.168.255.5
```
etc..
