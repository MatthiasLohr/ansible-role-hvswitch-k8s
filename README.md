# Ansible Role for Hetzner vSwitches for Kubernetes

This ansible role allows to configure Hetzner vSwitches and servers for Kubernetes clusters supporting additional vSwitch based subnets.

Neither this project nor the authors are affiliated with Hetzner.
This is a private project of Hetzner customers.


## Project Goals / Features

Using this ansible role, you can easily configure Hetzner vSwitches on your Hetzner bare metal servers.
Additionally, if you have a vSwitch based subnet, the role will configure all routes and ip rules required for using the subnet within your vSwitch network.
The configuration is compatible to MetalLB for allowing for MetalLB-based HA Kubernetes LoadBalancers.

The role will do the vSwitch configuration on the servers according to the [official Hetzner tutorial](https://wiki.hetzner.de/index.php/Vswitch/en#Server_configuration_.28Linux.29).
The role will **not** create the vSwitch nor register the IPs in the Hetzner robot.


## Configuration

All vSwitches to be configured have to be defined under the `vswitch` key.
The following example configuration shows how the configuration should look like for setting up a vSwitch with VLAN ID 4000:
```yaml
vswitches:
  - name: public        # vSwitch name, used for naming the routing table.
    routing_table: 1    # ID for the routing table.
    vlan: 4000          # VLAN ID for the vSwitch. 4000-4091 supported by Hetzner.
    gateway: 327.0.0.1  # If the vSwitch has a subnet, this variable should contain the subnet's gateway IP address
    addresses:          # IP addresses for the vSwitch network interface (per host)
      - "{{ hostvars[inventory_hostname]['ip'] }}/24"
    subnets:            # Subnets available on the vSwitch (need to be registered with Hetzner robot) for non-private networks
    - subnet: 327.0.0.0/24
```

The role will use this information to write a netplan configuration file.
