# openstack-ansible_deploys
Files related to different options of openstack-ansible depliyments

## Rebuilding the entire environment

It's useful to delete the all the configuration and start a new
configuration, but instead of starting a brand new OS installation and
configuration it's better delete the containers and all the
configuration made in the hosts:

1. Destroy all of the running containers:

    ```
	/opt/openstack-ansible/playbooks/openstack-ansible lxc-containers-destroy.yml
	```

1. Stop and remove systemd units running on the hosts:

	```
	for i in `systemctl list-units |grep 'nova\|neutron\|swift\|cinder'|awk '{print $1}'`
	do
		systemctl stop $i
		rm /etc/systemd/system/$i
	done
	```

1. Remove the python environments and configuration files:

	```
	rm -fr /openstack
	rm -fr /etc/{nova*,cinder*,neutron*,swift*}
	```

1. Uninstall keepalived and haproxy

	```
	apt purge keepalived haproxy
	rm -fr /etc/keepalived
	rm -fr /etc/haproxy
	```

1. Delete all directories related to swift:

    ```
	rm -fr /srv/node/sdb?/*
	```
