# Walk through active proxy service

ActiveProxy service is the first service provided by Carrier super node, and it's used to forward&#x20;

## Active Proxy Service Usage

The Active Proxy service is the first service provided by Carrier Super Node. It is used to forward service entries for third parties that are originally located within a LAN network, making them accessible to the public

## Preparing for Active Proxy Services on Carrier Super Nodes

Here is an example configuration file for Carrier Super Node for your reference.

```json
{
  "ipv4": true,
  "ipv6": false,
  "port": 39001,
  "dataDir": "/var/lib/carrier",

  "bootstraps": [
    {
	"id": "HZXXs9LTfNQjrDKvvexRhuMk8TTJhYCfrHwaj3jUzuhZ",
	"address": "155.138.245.211",
	"port": 39001
    }	// more bootstrap nodes. 
  ],
  
  "services": [
    {
      "class": "elastos.carrier.service.activeproxy.ActiveProxy",
      "configuration": {
        "port": 8090,
        "portMappingRange": "20000-22000"
      } 
    }
  ]
}
```

In this configuration file, there is only one service included, named 'ActiveProxy.' Typically, the '**`8090`**' TCP port is reserved for use by the ActiveProxy service, and during runtime, the TCP port mapping range of '**`20000-22000`**' will be utilized to map ports and forward the services.

💡 _Generally, the `TrinityTech` team deploys a list of Carrier Super Nodes, some of which provide Active Proxy services for users. All services from the Super Nodes use the same reserved port '8090' and port mapping range of '20000-22000'.”_

_At present, the Active Proxy service only supports the mapping of TCP-based connection services from internal sources to public-facing ones._

## Configuration of Active Proxy Service in the Carrier Node Application

An application, named "`Launcher`," is utilized as a daemon service running as a Native DHT node to make use of the Active Proxy service. The appropriate configuration for this is illustrated in the following example:

```json
{
  "ipv4": true,
  "ipv6": false,
  "dataDir": "./data",

  "bootstraps": [
    {
      "id": "HZXXs9LTfNQjrDKvvexRhuMk8TTJhYCfrHwaj3jUzuhZ",
      "address": "155.138.245.211",
      "port": 39001
    },
	  // more bootstrap nodes
  ],

  "services": [
    {
      "name": "ActiveProxy",
      "configuration": {
        "serverId": "YOUR-TARGET-CARRIER-SUPER-NODE-ID",
        "serverHost": "TARGET-CARRIER-SUPER-IP-ADDRESS-OR-HOSTNAME",
        "serverPort": "TARGET-ACTIVE-PROXY-SERVICE-PORT",
        "upstreamHost": "YOUR-SERIVCE-IP-ADDRESS", // local IP address
        "upstreamPort": "YOUR-SERVICE-PORT"
      }
    }
  ]
}
```

💡 Notice: The "\`Launcher\`" application can be constructed under the "launcher" directory of the \[Elastos.Native]\([https://github.com/elastos/Elastos.Carrier.Native](https://github.com/elastos/Elastos.Carrier.Native)) repository.

Here are the explanations for the fields used in “services” part of the service configuration file:

* **Name:** The name of the target service.
* **configuration.serverId:** The Node ID of the target super carrier node.
* **configuration.serverHost:** The IP address of the carrier super node.
* **configuration.serverPort:** The active port number of the proxy service.
* **configuration.upstreamHost:** The host name or IP address of the local service to be mapped out.
* **configuration.upstreamPort:** The port number of the local service to be mapped out.

## llRun a local service to be forwarded

Deploy a local website service on a Raspberry Pi device in a LAN environment. The website should be accessible at [http://192.168.1.101:80](http://192.168.1.101/)within the local network. Once the website is deployed, launch the `launcher` service using the "sample.conf" configuration file on the same Raspberry Pi device.

```json
{
  "ipv4": true,
  "ipv6": false,
  "dataDir": "./data",

  "bootstraps": [
    {
      "id": "HZXXs9LTfNQjrDKvvexRhuMk8TTJhYCfrHwaj3jUzuhZ",
      "address": "155.138.245.211",
      "port": 39001
    },
	  // more bootstrap nodes
  ],

  "services": [
    {
      "name": "ActiveProxy",
      "configuration": {
        "serverId": "HZXXs9LTfNQjrDKvvexRhuMk8TTJhYCfrHwaj3jUzuhZ",
        "serverHost": "155.138.245.211",
        "serverPort": 8090,
        "upstreamHost": "192.168.1.101", // local IP address of your raspberry device
        "upstreamPort": 80 // http-based website.
      }
    }
  ]
}
```

with the command under a directory with “**launcher**” binary

```sh
$ ./launcher -c sample.conf
```

Once you have launched the `launcher` service, the Carrier Super Node will assign a mapped port for your service. In my case, the assigned port is `20000`, which is the first port in the mapping range.

To check if your website is working properly, you can access it at [http://155.138.245.211:20000](http://155.138.245.211:20000/).
