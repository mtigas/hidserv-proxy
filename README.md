**Experimental repo. Maybe a bad idea. Don't use this for important purposes just yet.**

# hidserv-proxy

This forks the [SecureDrop repo](https://github.com/freedomofpress/securedrop/), utilizing it's base system (an [Ansible](http://www.ansible.com/home)-configured Ubuntu Linux system, hardened with a [grsec](https://grsecurity.net/) kernel provided by Freedom of the Press Foundation).

Instead of installing the SecureDrop application, this bootstraps a [Tor Hidden Service](https://www.torproject.org/docs/hidden-services.html.en), using Nginx (compiled with the [http subs module](http://wiki.nginx.org/HttpSubsModule)) to proxy to a normal website.

For news organizations and other website operators, this should provide an easy way to provide a well-configured Hidden Service route to the website.

As with SecureDrop, the machine that this service is deployed on should physically reside within your office. The oft-used method of deploying a hidden service within the same machine as the actual website means that all Tor-inbound traffic appears to come from `localhost` to the "normal" webserver -- allowing hidden service traffic to be separated from normal traffic. Deploying this service in-house allows the traffic to be mixed in somewhat with those of staff members visiting the website.

The extra [Nginx http subs module](http://wiki.nginx.org/HttpSubsModule) is used to rewrite URLs in web pages sent to the browser, so that the browser continues to make requests to the hidden service, rather than the "clear" version of the website. (i.e., URLs starting with "//www.propublica.org/" would be rewritten to "//propub3r6espa33w.onion/" so that images, stylesheets, and scripts would be requested from the hidden service.)

## Developing

Assuming [Ansible](http://www.ansible.com/home) (`brew install ansible`), [Virtualbox](https://www.virtualbox.org/wiki/Downloads), and [Vagrant](https://www.vagrantup.com/) are installed:

```
vagrant plugin install vagrant-hostmanager
vagrant up
```

Once the server is up, you can get the hidden service's address and test it.

```
cat install_files/ansible-base/hostname-ths-proxy
```

And you can get into it the usual Vagrant ways:

```
vagrant ssh
```

## License

[GNU Affero General Public License v3](/LICENSE)
