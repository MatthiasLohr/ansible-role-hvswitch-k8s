
## Configuration Example

```yaml
vswitches:
  - name: public
    routing_table: 1
    vlan: 4000
    gateway: 327.0.0.1
    addresses:
      - "{{ hostvars[inventory_hostname]['ip'] }}/24"
    subnets:
    - subnet: 327.0.0.0/24
      metallb_range: 327.0.0.2-327.0.0.254
```
