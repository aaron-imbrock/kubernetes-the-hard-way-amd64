# Provisioning Pod Network Routes

Pods scheduled to a node receive an IP address from the node's Pod CIDR range. At this point pods can not communicate with other pods running on different nodes due to missing network [routes](https://cloud.google.com/compute/docs/vpc/routes).

In this lab you will create a route for each worker node that maps the node's Pod CIDR range to the node's internal IP address.

> There are [other ways](https://kubernetes.io/docs/concepts/cluster-administration/networking/#how-to-achieve-this) to implement the Kubernetes networking model.

## The Routing Table

In this section you will gather the information required to create routes in the `kubernetes-the-hard-way` VPC network.

Print the internal IP address and Pod CIDR range for each worker instance:

```bash
{
  SERVER_IP=$(grep server machines.txt | cut -d " " -f 1)
  NODE_0_IP=$(grep node-0 machines.txt | cut -d " " -f 1)
  NODE_0_SUBNET=$(grep node-0 machines.txt | cut -d " " -f 4)
  NODE_1_IP=$(grep node-1 machines.txt | cut -d " " -f 1)
  NODE_1_SUBNET=$(grep node-1 machines.txt | cut -d " " -f 4)
}
```

```bash
ssh root@server <<EOF
  ip route add ${NODE_0_SUBNET} via ${NODE_0_IP}
  ip route add ${NODE_1_SUBNET} via ${NODE_1_IP}
EOF
```

```bash
ssh root@node-0 <<EOF
  ip route add ${NODE_1_SUBNET} via ${NODE_1_IP}
EOF
```

```bash
ssh root@node-1 <<EOF
  ip route add ${NODE_0_SUBNET} via ${NODE_0_IP}
EOF
```

## Verification

### Server

```bash
ssh root@server ip route
```
```text
default via XXX.XXX.XXX.XXX dev ens160
10.200.0.0/24 via XXX.XXX.XXX.XXX dev ens160
10.200.1.0/24 via XXX.XXX.XXX.XXX dev ens160
XXX.XXX.XXX.0/24 dev ens160 proto kernel scope link src XXX.XXX.XXX.XXX
```

#### Lab Output

```bash
default via 10.0.0.1 dev enp1s0
10.0.0.0/24 dev enp1s0 proto kernel scope link src 10.0.0.21
192.168.0.0/24 via 10.0.0.22 dev enp1s0
192.168.1.0/24 via 10.0.0.23 dev enp1s0
```

### Node-0

```bash
ssh root@node-0 ip route
```

```text
default via XXX.XXX.XXX.XXX dev ens160
10.200.1.0/24 via XXX.XXX.XXX.XXX dev ens160
XXX.XXX.XXX.0/24 dev ens160 proto kernel scope link src XXX.XXX.XXX.XXX
```

#### Lab Output

```bash
default via 10.0.0.1 dev enp1s0
10.0.0.0/24 dev enp1s0 proto kernel scope link src 10.0.0.22
192.168.1.0/24 via 10.0.0.23 dev enp1s0
```

### Node-1

```bash
ssh root@node-1 ip route
```

```text
default via XXX.XXX.XXX.XXX dev ens160
10.200.0.0/24 via XXX.XXX.XXX.XXX dev ens160
XXX.XXX.XXX.0/24 dev ens160 proto kernel scope link src XXX.XXX.XXX.XXX
```

#### Lab Output

```bash
default via 10.0.0.1 dev enp1s0
10.0.0.0/24 dev enp1s0 proto kernel scope link src 10.0.0.23
192.168.0.0/24 via 10.0.0.22 dev enp1s0
```

Next: [Smoke Test](12-smoke-test.md)
