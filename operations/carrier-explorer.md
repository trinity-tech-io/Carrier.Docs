---
layout:
  title:
    visible: true
  description:
    visible: false
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: true
---

# Carrier Explorer

{% hint style="info" %}
We expect the community to develop and deploy carrier explorers that can present all active node information throughout the entire carrier network. Please inform us once you start working on this.
{% endhint %}

## Carrier Crawler

A carrier crawler is a specialized type of carrier node dedicated to crawling active carrier node information throughout the entire carrier network. Its purpose is to crawl carrier node information by iteratively sending the primitive protocol - findNode. It then collects all node information with a tuple value of {nodeId, IP address, port}, and extracts the node positions according to the IP address respectively.

## Carrier Explorer

A carrier explorer is a website integrated with [**carrier crawler**](carrier-explorer.md#carrier-crawler) that presents all active carrier nodes discovered by crawling the entire carrier network. Community users can deploy an explorer website integrated with a carrier crawler to display as many active carrier nodes as possible.

## More Links

* [The repository for using Carrier crawler](https://github.com/elastos/Elastos.Carrier.Native/tree/master/apps/crawler)
