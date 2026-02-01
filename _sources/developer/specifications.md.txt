# Specifications

Gershwin is designed to be friendly and welcoming. The desired user experience informs the specifications embraced for use in Gershwin and applications running on it.

## Specifications embraced for use in Gershwin

These specifications cover technologies that are enthusiastically embraced in Gershwin and/or are recommended to be used in applications running on it.

* __mDNS__: Allows hostnames of devices to be published to the network, so that devices can be accessed using their names rather than using IP numbers.
* __DNS-SD/Zeroconf__: Allows for the discovery of services on the network, such as websites, SSH servers, SFTP shares, etc. If an application can handle certain types of network services, it should discover such services using DNS-SD/Zeroconf. For example, a Terminal application might offer to connect to SSH servers on the network discovered with DNS-SD/Zeroconf.
* __IPP Everywhere__: Allows printers to "just work" without configuring printer drivers.
* __AppImage__: Allows applications to be shipped and used as single files. Gershwin may predominantly use this format for Linux applications in the future.
* __Markdown__: The standard format for all documentation and other written text.
* __UTF-8__: The default encoding for text.

This list is not exhaustive.

## Specifications with relevance for Gershwin

These specifications cover technologies that may have relevance for Gershwin and are mostly considered for compatibility reasons. Their use within Gershwin may decline over time as more native replacements become available.

* __ICCCM/EWMH__: Allows windows to be managed on Xorg. [This page in the picom wiki](https://github.com/yshui/picom/wiki/Window-states-management) gives an overview
* __D-Bus__: Allows processes on the system to interact with each other (inter-process communication, IPC)

This list is not exhaustive.
