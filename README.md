
**Namespaces, openvswitch and subnets scheme in the ovs_schema.jpg**

**Install openvswitch**
apt-get install openvswitch-switch openvswitch-common c-y

**1. Create 2 namespace V1 and V2**

ip netns add V1 && ip netns add V2
ip netns list
V1
V2


**2. Create virtual ethernet ports Veth1 and veth2 and connect them to each other**

ip link add veth1 type veth peer name veth2
ip link add veth3 type veth peer name veth4
ip link


**3. Move veth1 to V1 and veth3 to v2**

ip link set veth1 netns V1
ip link set veth3 netns V2
ip link


**4. Assign IP addresses to veth in V1 and V2**
ip netns exec V1 ip link
ip netns exec V2 ip link


ip netns exec V1 ip addr add 10.10.10.1/24 dev veth1
ip netns exec V1 ip link set veth1 up
ip netns exec V1 ip a


ip netns exec V2 ip addr add 10.10.10.2/24 dev veth3
ip netns exec V2 ip link set veth3 up
ip netns exec V2 ip a

ip netns exec V1 ip -br a
ip netns exec V2 ip -br a
ip netns exec V1 ip route
ip netns exec V2 ip routec

ip netns exec V1 ping -c5 10.10.10.2


**5. Create vSwitch1**

ovs-vsctl show
ovs-vsctl add-br vSwitch1
ovs-vsctl show
ip link

**6. Assign veth2 and veth4 to vSwitch1**

ovs-vsctl add-port vSwitch1 veth2
ovs-vsctl add-port vSwitch1 veth4
ip link
ovs-vsctl show

**7. TEST CONNECTIVITY**

ip netns exec V1 ping -c5 10.10.10.2
ip a
ip link

ip link set veth2 up
ip link set veth4 up
ifconfig
ip netns exec V1 ping -c5 10.10.10.2
PING WORK

**8. Assign IP address to bridge interface of vSwitch1**

ip addr add 10.10.10.3/24 dev vSwitch1
ip link set vSwitch1 up

ping 10.10.10.1 -c 5
ping 10.10.10.2 -c 5
ping 10.10.10.3 -c 5

ovsdb-client dump 

**9. Check the MAC address table of vSwitch1**

ip netns exec V1  ip a | grep link
ip netns exec V2  ip a | grep link
ip a | grep -A2 vSwitch1 | grep link
ovs-appctl fdb/show vSwitch1


**10. Delete namespace and bridge**

ip netns del V1
ip netns del V2
ovs-vsctl del-br vSwitch1
