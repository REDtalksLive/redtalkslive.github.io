---
title: 'HOW TO: Create Single Node Consul Server Cluster'
date: '2022-10-25'
draft: false
description: "Learn how to create a single-node Consul server cluster for dev/test"
tags: ["Consul", "How To", "Consul Server"]
featured_image: ''
omit_header_text: true
toc: true
---

## Overview

How to create a Consul single-node dev/test cluster!

## Install the consul binary

### Ubuntu/Debian - Latest Version

```sh
wget -O- https://apt.releases.hashicorp.com/gpg | gpg --dearmor | sudo tee /usr/share/keyrings/hashicorp-archive-keyring.gpg
echo "deb [signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] https://apt.releases.hashicorp.com $(lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/hashicorp.list
sudo apt update && sudo apt install consul
```

### Ubuntu/Debian - Specific Version

In this example we shall first get a list of available versions:
```sh
wget -O- https://apt.releases.hashicorp.com/gpg | gpg --dearmor | sudo tee /usr/share/keyrings/hashicorp-archive-keyring.gpg
echo "deb [signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] https://apt.releases.hashicorp.com $(lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/hashicorp.list
sudo apt update && apt list -a consul-enterprise
```

The select the version you want from the list and install, e.g `1.13.3+ent-1`:

```sh
sudo apt install consul-enterprise=1.13.3+ent-1
```

*NOTE:* For community (test/dev):
```sh
apt list -a consul
sudo apt install consul=1.13.3-1
```
---

## Launch Secure Consul Server

### Create CA, Server and Client Certs, and Gossip Key

```sh
sudo mkdir /opt/consul/certs
consul tls ca create
consul tls cert create -dc=dc1 -server
consul tls cert create -dc=dc1 -client
sudo mv consul-agent-ca.pem /opt/consul/certs/
sudo mv dc1-server-consul-0.pem /opt/consul/certs/
sudo mv dc1-server-consul-0-key.pem /opt/consul/certs/
sudo chown consul:consul -R /opt/consul/certs
```

# Generate Gossip key and save to config

```sh
consul keygen
```

Output will look something like this:

`tyMLsxNNfugnoeKGR5QPaqCHQ5C4n1Ri2h2d2DM0LTg=`

Add this to the `encrypt` key in the server config below

Create a server config file `sudo vi /etc/consul.d/server.hcl` and add the following:

```hcl
license_path            = "/etc/consul.d/consul.hclic"
server                  = true
datacenter              = "dc1"
bootstrap_expect        = 1

client_addr             = "0.0.0.0"   # UI Access
bind_addr               = "{{ GetInterfaceIP \"ens160\" }}"
ui_config               = {
  enabled                 = true
}

tls = {
  defaults = {
    ca_file = "/opt/consul/certs/consul-agent-ca.pem"
    cert_file               = "/opt/consul/certs/dc1-server-consul-0.pem"
    key_file                = "/opt/consul/certs/dc1-server-consul-0-key.pem"
    verify_incoming         = true
    verify_outgoing         = true
  }
}
encrypt                 = "tyMLsxNNfugnoeKGR5QPaqCHQ5C4n1Ri2h2d2DM0LTg="

auto_encrypt {
  allow_tls = true
}
```

Enable ACLs:


Create a new file `sudo vi /etc/consul.d/acl.hcl` with the following:

```hcl
acl = {
  enabled = true
  default_policy = "deny"
  enable_token_persistence = true
}
```

Create the initial boostrap token:

```sh
consul acl bootstrap
```

You will need the value of `SecretID:` to bootstrap client agents.

Restart the server and verify it loads:

```sh
sudo systemctl restart consul
sudo systemctl status consul
```

You should see additional CA/cert process, e.g.:

```sh
Oct 26 18:11:05 consul-dc1 consul[59917]: 2022-10-26T18:11:05.013-0700 [INFO]  connect.ca: updated root certificates from primary datacenter
Oct 26 18:11:05 consul-dc1 consul[59917]: 2022-10-26T18:11:05.014-0700 [INFO]  connect.ca: initialized primary datacenter CA with provider: provider=consul
Oct 26 18:11:05 consul-dc1 consul[59917]: 2022-10-26T18:11:05.014-0700 [INFO]  agent.leader: started routine: routine="intermediate cert renew watch"
Oct 26 18:11:05 consul-dc1 consul[59917]: 2022-10-26T18:11:05.014-0700 [INFO]  agent.leader: started routine: routine="CA root pruning"
Oct 26 18:11:05 consul-dc1 consul[59917]: 2022-10-26T18:11:05.014-0700 [INFO]  agent.leader: started routine: routine="CA root expiration metric"
Oct 26 18:11:05 consul-dc1 consul[59917]: 2022-10-26T18:11:05.014-0700 [INFO]  agent.leader: started routine: routine="CA signing expiration metric"
Oct 26 18:11:05 consul-dc1 consul[59917]: 2022-10-26T18:11:05.014-0700 [INFO]  agent.leader: started routine: routine="virtual IP version check"
Oct 26 18:11:05 consul-dc1 consul[59917]: 2022-10-26T18:11:05.015-0700 [INFO]  agent.leader: stopping routine: routine="virtual IP version check"
Oct 26 18:11:05 consul-dc1 consul[59917]: 2022-10-26T18:11:05.015-0700 [INFO]  agent.leader: stopped routine: routine="virtual IP version check"
Oct 26 18:11:05 consul-dc1 consul[59917]: 2022-10-26T18:11:05.080-0700 [INFO]  agent: Synced node info
```

and ACL output:
```sh
Oct 26 18:24:48 consul-dc1 consul[60470]: 2022-10-26T18:24:48.055-0700 [INFO]  agent.server: initializing acls
Oct 26 18:24:48 consul-dc1 consul[60470]: 2022-10-26T18:24:48.056-0700 [INFO]  agent.server: Created ACL 'global-management' policy
Oct 26 18:24:48 consul-dc1 consul[60470]: 2022-10-26T18:24:48.056-0700 [INFO]  agent.server: Created ACL anonymous token from configuration
Oct 26 18:24:48 consul-dc1 consul[60470]: 2022-10-26T18:24:48.056-0700 [INFO]  agent.leader: started routine: routine="legacy ACL token upgrade"
Oct 26 18:24:48 consul-dc1 consul[60470]: 2022-10-26T18:24:48.057-0700 [INFO]  agent.leader: started routine: routine="acl token reaping"
```

---

## Launch Unsecure Consul Server

Verify the location of the configuration files:

```sh
cat /lib/systemd/system/consul.service
```

Expected output:

```sh
[Unit]
Description="HashiCorp Consul - A service mesh solution"
Documentation=https://www.consul.io/
Requires=network-online.target
After=network-online.target
ConditionFileNotEmpty=/etc/consul.d/consul.hcl

[Service]
EnvironmentFile=-/etc/consul.d/consul.env
User=consul
Group=consul
ExecStart=/usr/bin/consul agent -config-dir=/etc/consul.d/
ExecReload=/bin/kill --signal HUP $MAINPID
KillMode=process
KillSignal=SIGTERM
Restart=on-failure
LimitNOFILE=65536

[Install]
WantedBy=multi-user.target
```

**NOTE:** specifially the `EnvironmentFile` and `ExecStart` config directory.

Get an entperprise license and put it here:
`/etc/consul.d/consul.hclic`

### Insecure Server

Create a server config file `sudo vi /etc/consul.d/server.hcl` and add the following:

```
license_path      = "/etc/consul.d/consul.hclic"
server            = true
datacenter        = "dc1"
bootstrap_expect  = 1
client_addr       = "0.0.0.0"   # UI Access
bind_addr         = "{{ GetInterfaceIP \"ens160\" }}"
ui_config         = {
  enabled           = true
}
```

Start the server:
```
sudo systemctl start consul
```

Verify it loaded:
```
sudo systemctl status consul
```

### Troubleshooting

Successful load generates the following output:

```sh
● consul.service - "HashiCorp Consul - A service mesh solution"
     Loaded: loaded (/lib/systemd/system/consul.service; disabled; vendor preset: enabled)
     Active: active (running) since Wed 2022-10-26 12:43:41 PDT; 7s ago
       Docs: https://www.consul.io/
   Main PID: 46327 (consul)
      Tasks: 6 (limit: 2337)
     Memory: 29.3M
        CPU: 92ms
     CGroup: /system.slice/consul.service
             └─46327 /usr/bin/consul agent -config-dir=/etc/consul.d/

Oct 26 12:43:41 consul-dc1 consul[46327]: 2022-10-26T12:43:41.375-0700 [WARN]  agent.server.serf.lan: serf: Failed to re-join any previously known node
Oct 26 12:43:41 consul-dc1 consul[46327]: 2022-10-26T12:43:41.375-0700 [INFO]  agent.server: Adding LAN server: server="consul-dc1 (Addr: tcp/10.0.0.124:8300) (DC: dc1)"
Oct 26 12:43:41 consul-dc1 consul[46327]: 2022-10-26T12:43:41.375-0700 [INFO]  agent.server: Handled event for server in area: event=member-join server=consul-dc1.dc1 area=wan
Oct 26 12:43:41 consul-dc1 consul[46327]: 2022-10-26T12:43:41.375-0700 [WARN]  agent: [core]grpc: addrConn.createTransport failed to connect to {dc1-10.0.0.124:8300 consul-dc1 <nil> 0 <nil>}. Err: connection error: desc = "transport: Error while dialing dial tcp 10.0.0.124:0->10.0.0.124:8300: operation was canceled". Reconnecting...
Oct 26 12:43:41 consul-dc1 consul[46327]: 2022-10-26T12:43:41.376-0700 [INFO]  agent: Started DNS server: address=0.0.0.0:8600 network=udp
Oct 26 12:43:41 consul-dc1 consul[46327]: 2022-10-26T12:43:41.376-0700 [INFO]  agent: Started DNS server: address=0.0.0.0:8600 network=tcp
Oct 26 12:43:41 consul-dc1 consul[46327]: 2022-10-26T12:43:41.377-0700 [INFO]  agent: Starting server: address=[::]:8500 network=tcp protocol=http
Oct 26 12:43:41 consul-dc1 consul[46327]: 2022-10-26T12:43:41.377-0700 [INFO]  agent: started state syncer
Oct 26 12:43:41 consul-dc1 consul[46327]: 2022-10-26T12:43:41.377-0700 [INFO]  agent: Consul agent running!
Oct 26 12:43:41 consul-dc1 consul[46327]: 2022-10-26T12:43:41.377-0700 [WARN]  agent.license: Feature "Audit Logging" is unlicensed
```

For more detailed output, execute:
```
sudo journalctl -u consul
```

#### Error binding to Interface

If there are multple IP Addresses (exluding loopback), Consul may error when binding to "0.0.0.0". Verify this issue by selecting an IP address to bind to. Solve this by using go templating, e.g. "{{ GetInterfaceIP \"ens160\" }}"

```sh
```
