# This role have dependency: 
# Ubuntu 18.04
# And set ec2 tag for join members in to consul cluster

Fore example, we are need  consul cluster in to AWS eu-west-3 region, and we set datacenter in  consul config in to eu-west-3 value.

1. Copy certificates in to file directory of the consul-client-aws-ec2 role. We need this certificate:
```sh
consul-agent-ca.pem
eu-west-3-client-consul-0-key.pem
eu-west-3-client-consul-0.pem
```
 This certificates should be generated when you run consul-cluster-aws-ec2 role.


## Configure consul-client-aws-ec2 role variables. For example:
```sh
# Certificates for consul servers
ca_cert: "consul-agent-ca.pem"
client_cert: "eu-west-3-client-consul-0.pem"
client_key: "eu-west-3-client-consul-0-key.pem"

consul_binary_url: "https://releases.hashicorp.com/consul/1.4.1/consul_1.4.1_linux_amd64.zip"
consul_download_path: "/tmp"
consul_group: "consul"
consul_user: "consul"
consul_shell: "/usr/sbin/nologin"
consul_home_dir: "/var/lib/consul"

# Example consul dc
consul_datacenter: "eu-west-3"
# consul_encrypt_key should be generated when you run consul-cluster-aws-ec2 role
consul_encrypt_key: "9aBBNjSGurSYNUpvpjAh3Q=="
consul_ec2_tag_key: "consul_join"
consul_ec2_tag_value: "{{ consul_datacenter }}"
```