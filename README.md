**Experimental repo. Maybe a bad idea. Don't use this for important purposes just yet.**

# hidserv-proxy

This forks the [SecureDrop repo](https://github.com/freedomofpress/securedrop/), utilizing it's base system (an [Ansible](http://www.ansible.com/home)-configured Ubuntu Linux system, hardened with a [grsec](https://grsecurity.net/) kernel provided by Freedom of the Press Foundation).

Instead of installing the SecureDrop application, this bootstraps a [Tor Hidden Service](https://www.torproject.org/docs/hidden-services.html.en), using Nginx (compiled with the [http subs module](http://wiki.nginx.org/HttpSubsModule)) to proxy to a normal website.

For news organizations and other website operators, this should provide an easy way to provide a well-configured Hidden Service route to the website.

As with SecureDrop, the machine that this service is deployed on should physically reside within your office. The oft-used method of deploying a hidden service within the same machine as the actual website means that all Tor-inbound traffic appears to come from `localhost` to the "normal" webserver -- allowing hidden service traffic to be separated from normal traffic. Deploying this service in-house allows the traffic to be mixed in somewhat with those of staff members visiting the website.

The extra [Nginx http subs module](http://wiki.nginx.org/HttpSubsModule) is used to rewrite URLs in web pages sent to the browser, so that the browser continues to make requests to the hidden service, rather than the "clear" version of the website. (i.e., URLs starting with "//www.example.com/" would be rewritten to "//ltty7biw4a4q7q6v.onion/" so that images, stylesheets, and scripts would be requested from the hidden service.)

## Developing

Edit `install_files/ansible-base/prod-specific.yml` and change `proxy_pass`, `proxy_domain_host`, and `proxy_replace_domain` as you desire.

Now, assuming [Ansible](http://www.ansible.com/home) (`brew install ansible`), [Virtualbox](https://www.virtualbox.org/wiki/Downloads), and [Vagrant](https://www.vagrantup.com/) are installed (+ the Vagrant hostmanager plugin: `vagrant plugin install vagrant-hostmanager`), you can run the server.

```
vagrant up
```

Once the server is up, you can get the hidden service's address and test it -- it should load `http://www.example.com` or whatever you set it to proxy to.

```
cat install_files/ansible-base/hostname-ths-proxy
```

## to-do

* Fix http-subs rewrite rules to work more consistently
* Way to provide tor hidden service private key? (for pre-existing or [generated](https://github.com/Freaken/Shallot) [vanity](https://github.com/lachesis/scallion) .onion names)
* Multiple hidden services/domains? (i.e.: generate hidden services for the "www.example.com" domain, one for the "projects.example.com" domain, etc. without having to launch multiple instances of this)

## License

[GNU Affero General Public License v3](/LICENSE)
