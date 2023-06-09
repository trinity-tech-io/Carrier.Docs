---
description: A beginner's guide to deploy a Carrier super node service
---

# 🔆 Setting Up a Super Node

{% hint style="info" %}
_It's recommended to install the Carrier Super Node Service on **Ubuntu Linux version 22.04** or later, and need bind it to a public IP address._
{% endhint %}

## Installation of Runtime Dependencies

The **carrier super node** has dependencies on the following runtime components:

* Java Virtual Machine (JVM) >= Java 11
* sodium (libsodium) >= 1.0.16

To install these dependencies, please run the following commands:

```sh
$ sudo apt install openjdk-11-jre-headless libsodium23
```

## &#x20;Building Your Debian Package

Please ensure that JDK-11 has been installed on your building machine before building the Carrier deamon debian package. Use the following command to carry out the whole building process:

```sh
$ git clone git@github.com:elastos/Elastos.Carrier.Java.git  Carrier.Java
$ cd Carrier.Java
$ ./mvnw -Dmaven.test.skip=true
```

Once the build process finishes, the Debian package will be generated under the directory `launcher/target` with the name like _**carrier-launcher-\<version>-\<timestamp>.deb**_

After the build process completes, a Debian package will be generated in the `launcher/target`directory with a name following the format _**carrier-launcher-\<version>-\<timestamp>.deb**_. Please note that  `<version>` and `<timestamp>` will vary depending on the specific version of the package being built.

## Installing the Carrier Super Node Service

After uploading the Debian Package to the target VPS server, run the following command to install the Carrier super node:

```sh
$ sudo dpkg -i carrier-launcher-<version>-<timestamp>.deb
```

The Carrier super node installation includes several directories and files, which are organized as follows:&#x20;

* **/usr/lib/carrier**:  contains the runtime libraries, including jar packages
* **/etc/carrier**:  contains the configuration file **default.conf**
* **/var/lib/carrier**:  contains the runtime data store
* **/var/log/carrier**: contains the output log file **carrier.log**
* **/var/run/carrier**: contains the runtime directory.

The data cached under **`/var/lib/carrier`** is organized into the following structure:

* **/var/lib/carrier/key**: contains a randomly generated private key
* **/var/lib/carrier/id**: contains the node ID
* **/var/lib/carrier/dht4.cache**: contains the routing table information for IPv4 addresses
* _**/var/lib/carrier/dht6.cache**_: contains the routing table information for IPv6 addresses if IPv6 is enabled
* **/var/lib/carrier/node.db**: contains the information about Value and PeerInfo

Once the Carrier super node has been installed as a service, it is necessary to open the designated port for usage (the default is `39001`):

```sh
$ sudo ufw allow 39001/udp
```

To check if the port is accessible, use the following command. Additionally, you can review the log file for more detailed information on the current running status:

```sh
$ sudo ufw status verbose
$ tail -f /var/log/carrier/carrier.log
```

We would also recommend using the '`systemctl`' command to check the status of the Carrier daemon service or to start/stop the service:

```sh
$ systemctl status carrier
$ sudo systemctl start carrier
$ sudo systemctl stop carrier
```

## An example of config file

To officially launch the Carrier super node and improve the health of the Carrier network, the service config file should be updated to reference the following configuration file:

```json
{
  "ipv4": true,
  "ipv6": false,
  "address4": "your-ipv4-address",
  "address6": "your-ipv6-address",
  "port": 39001,
  "dataDir": "/var/lib/carrier",

  "bootstraps": [
    // carrier-node1
    {
      "id": "HZXXs9LTfNQjrDKvvexRhuMk8TTJhYCfrHwaj3jUzuhZ",
      "address": "155.138.245.211",
      "port": 39001
    },
    // carrier-node2
    {
      "id": "FRkR2NWhbSGMv3BqGui7FYAgCSAWySrz6xmTAx9Ny7zo",
      "address": "45.76.161.175",
      "port": 39001
    },
    // carrier-node3
    {
      "id": "8grFdb2f6LLJajHwARvXC95y73WXEanNS1rbBAZYbC5L",
      "address": "140.82.57.197",
      "port": 39001
    },
    // carrier-node4
    {
      "id": "4A6UDpARbKBJZmW5s6CmGDgeNmTxWFoGUi2Z5C4z7E41",
      "address": "107.191.62.45",
      "port": 39001
    },
    // carrier-node5
    {
      "id": "5BJ8SZZQ4z4izhw82W2ksyuTCQz3GwWUWBSaza4qzVT9",
      "address": "207.148.82.19",
      "port": 39001
    }
  ] 
}
```
