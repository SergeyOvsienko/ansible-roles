# This role have dependency: 
# Ubuntu 18.04
# Before start need generate tls certificate. How to do it you are can read in to [consul documentation](https://learn.hashicorp.com/consul/advanced/day-1-operations/certificates#creating-certificates)
# And set ec2 tag for join members in to consul cluster

Fore example, we are need  consul cluster in to AWS eu-west-3 region, and we set datacenter in  consul config in to eu-west-3 value.

1. Generate ca certificate
```sh
consul tls ca create
```
2. Generate consul server certificate
```sh
consul tls cert create -server -dc=eu-west-3
```
3. Generate consul client certificate
```sh
consul tls cert create -client -dc=eu-west-3
```
4. Generate consul cli certificate
```sh
consul tls cert create -cli -dc=eu-west-3
```

After generate all certificate we have
```sh
consul-agent-ca-key.pem
consul-agent-ca.pem
eu-west-3-cli-consul-0-key.pem
eu-west-3-cli-consul-0.pem
eu-west-3-client-consul-0-key.pem
eu-west-3-client-consul-0.pem
eu-west-3-server-consul-0-key.pem
eu-west-3-server-consul-0.pem
```

5. Copy certificates in to file directory of the consul-cluster-aws-ec2 role. We need this certificate:
```sh
consul-agent-ca.pem
eu-west-3-server-consul-0-key.pem
eu-west-3-server-consul-0.pem
eu-west-3-cli-consul-0-key.pem
eu-west-3-cli-consul-0.pem
```

6. Generate consul encrypt key
```sh
consul keygen
```

### Configure consul-cluster-aws-ec2 role variables. For example:
```sh
# Certificates for consul servers
ca_cert: "consul-agent-ca.pem"
server_cert: "eu-west-3-server-consul-0.pem"
server_key: "eu-west-3-server-consul-0-key.pem"
cli_cert: "eu-west-3-cli-consul-0.pem"
cli_key: "eu-west-3-cli-consul-0-key.pem"

consul_binary_url: "https://releases.hashicorp.com/consul/1.4.1/consul_1.4.1_linux_amd64.zip"
consul_download_path: "/tmp"
consul_group: "consul"
consul_user: "consul"
consul_shell: "/usr/sbin/nologin"
consul_home_dir: "/var/lib/consul"

# Example consul dc
consul_datacenter: "eu-west-3"
consul_bootstrap_expect: "3"
consul_encrypt_key: "qNW/bw3npTuVTviwYrDoKQ=="
consul_ec2_tag_key: "consul_join"
consul_ec2_tag_value: "{{ consul_datacenter }}"
```