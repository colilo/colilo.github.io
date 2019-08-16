Assign fixed ip addresses to specific mac in a DHCP server
```
# Virtual ethernet segment 8
subnet 192.168.56.0 netmask 255.255.255.0 {
range 192.168.56.128 192.168.56.254;            # default allows up to 125 VM's
option broadcast-address 192.168.56.255;
option domain-name-servers 192.168.56.2;
option domain-name "localdomain";
option netbios-name-servers 192.168.56.2;
option routers 192.168.56.2;
default-lease-time 5529540;
max-lease-time 5529540;
}
host VMnet8 {
    hardware ethernet xx:xx:xx:xx:xx:xx;
    fixed-address 192.168.56.1;
    option domain-name-servers 0.0.0.0;
    option domain-name "";
    option routers 0.0.0.0;
}
host ceph-node-1 {
    hardware ethernet xx:xx:xx:xx:xx:xx;
    fixed-address 192.168.56.145;
}
host ceph-node-2 {
    hardware ethernet xx:xx:xx:xx:xx:xx;
    fixed-address 192.168.56.146;
}
host ceph-node-3 {
    hardware ethernet xx:xx:xx:xx:xx:xx;
    fixed-address 192.168.56.147;
}

```
