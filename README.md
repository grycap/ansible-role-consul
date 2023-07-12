Hashicorp Consul Role
=====================

Install Hashicorp Consul Role (recipe for AI4EOSC)

Role Variables
--------------

The variables that can be passed to this role and a brief description about them are as follows.

	# Consul version to install
	consul_version: 114.3
	# Consul Web UI version to install
	consul_webui_version: 1.14.3
	# Flag to set if consul will work as server
	consul_server: true
	# Flag to set if consul will install the web ui
	consul_webui: true
	# Consul datacenter name
	consul_dc_name: dc1
	# Consul domain name
	consul_domain: consul
	# Consul rejoin IPs: Comma separated list of IPs to join
	consul_join_ips: ["{{ ansible_default_ipv4.address }}"]
	# Consul advertise address
	consul_advertise_addr: "{{ ansible_default_ipv4.address }}"
	# Number of servers to bootstrap recommended (1, 3, 5))
	consul_bootstrap_expect: 1
	# Consul agent token
	consul_agent_token: ""
	# Flag to configure systemd-resolved with consul dns
	consul_dns: true
	# URL to download consul
	consul_url: https://releases.hashicorp.com/consul/{{ consul_version }}/consul_{{ consul_version }}_linux_amd64.zip
	# Path to download consul zip
	consul_download_dir: /tmp
	# Path to install consul binaries
	consul_install_dir: /usr/bin
	# Path to install consul config
	consul_config_dir: /var/lib/consul/conf
	# Path to install consul data
	consul_data_dir: /var/lib/consul
	# Consul user
	consul_user: consul
	# Consul group
	consul_group: consul
	# Consul service name
	consul_service_name: consul

Example Playbook
----------------

To configure the set of servers (recommended 1, 3 or 5):

```
  - hosts: server
    vars:
     server_list: "{{ groups['server'] | map('extract', hostvars,'IM_NODE_PRIVATE_IP') | list }}"
    roles:
    - role: 'consul'
      consul_server: true
      consul_join_ips: "{{ server_list }}"

```

To the agents:

```
  - hosts: agent
    vars:
     server_list: "{{ groups['server'] | map('extract', hostvars,'IM_NODE_PRIVATE_IP') | list }}"
    roles:
    - role: 'consul'
      consul_server: false
      consul_join_ips: "{{ server_list }}"

```

Contributing to the role
========================
In order to keep the code clean, pushing changes to the master branch has been disabled. If you want to contribute, you have to create a branch, upload your changes and then create a pull request.  
Thanks
