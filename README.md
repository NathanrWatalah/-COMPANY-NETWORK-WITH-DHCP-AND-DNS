# PROJECT 3: COMPANY NETWORK WITH DHCP AND DNS

## Cisco Packet Tracer Project Documentation

**Student Name:** Nathan R. Watalah
**Program:** Information Technology
**Level:** Sophomore 2, System Administration
**Project Title:** Company Network with DHCP and DNS
**Software Used:** Cisco Packet Tracer
**Project Type:** Network Design, Server Configuration, IP Address Management, and DNS Resolution

## 1. Introduction

Computer networks allow computers and servers to communicate and share information. In a company with many computers, configuring every computer manually can take time and lead to mistakes.

Two important network services help solve this problem: Dynamic Host Configuration Protocol (DHCP) and Domain Name System (DNS).

DHCP automatically provides computers with network settings such as IP addresses, subnet masks, default gateways, and DNS server addresses. DNS allows computers to find devices and services by using names instead of requiring users to remember IP addresses.

This project demonstrates how to configure a company network with a router, a switch, multiple computers, and a server that provides DHCP and DNS services using Cisco Packet Tracer.

## 2. Project Scenario

Imagine a small company with several employees who need access to the company network.

The company wants to reduce the time spent manually configuring computers. It also wants employees to access the company server using an easy-to-remember name instead of an IP address.

To solve this problem, the network administrator installs a server and enables DHCP and DNS services.

The DHCP service automatically assigns IP addresses to client computers, while the DNS service resolves a server name to its corresponding IP address.

### Network Topology

```text
                  Router
             192.168.40.1
                     |
                     |
                   Switch
              _______|_______
             |       |       |
            PC1    Server    PC2
                   DHCP
                    DNS
```

The router connects to the switch, and the switch connects the client computers and server.

## 3. Problem Statement

Manually assigning IP addresses to every computer increases administrative work and creates opportunities for configuration errors.

Employees may also find it difficult to remember server IP addresses.

This project addresses these problems by using DHCP to automate client network configuration and DNS to provide name resolution for the company server.

## 4. Project Aim

The main aim of this project is to design and configure a company network that uses DHCP and DNS services to simplify IP address management and server access.

## 5. Project Objectives

The objectives are to:

1. Design a basic company network in Cisco Packet Tracer.
2. Connect a router, switch, server, and client computers.
3. Configure the router's LAN interface.
4. Assign a static IP address to the server.
5. Configure the server's DHCP service.
6. Automatically assign IP addresses to client computers.
7. Configure DNS records for the company server.
8. Test DNS name resolution.
9. Verify communication between clients, the server, and the router.
10. Troubleshoot common DHCP and DNS problems.

## 6. Purpose of the Project

The purpose is to learn how companies automate network configuration and allow users to access network resources through names.

Instead of configuring every computer manually, the network administrator can use DHCP to provide the required network settings.

DNS makes network access easier by allowing users to use a name such as `server.company.local` instead of remembering an IP address.

## 7. Devices and Software Required

| Device or software             |    Quantity | Purpose                            |
| ------------------------------ | ----------: | ---------------------------------- |
| Cisco router                   |           1 | Provides the default gateway       |
| Cisco switch                   |           1 | Connects the network devices       |
| Server                         |           1 | Provides DHCP and DNS services     |
| PCs                            |   2 or more | Represent employee computers       |
| Copper straight-through cables | As required | Connect the devices                |
| Cisco Packet Tracer            |           1 | Used to build and test the network |

This project uses one IPv4 network to keep the configuration simple. DHCP requests can reach the server directly because the server and clients share the same LAN.

## 8. Network Design and IP Addressing Plan

The network uses the following IPv4 subnet:

- Network address: `192.168.40.0/24`
- Subnet mask: `255.255.255.0`
- Default gateway: `192.168.40.1`
- DNS server: `192.168.40.2`
- DHCP client address range: `192.168.40.10` onward

### IP Addressing Table

| Device | IP address method | IP address             | Subnet mask   | Default gateway |
| ------ | ----------------- | ---------------------- | ------------- | --------------- |
| Router | Static            | 192.168.40.1           | 255.255.255.0 | Not applicable  |
| Server | Static            | 192.168.40.2           | 255.255.255.0 | 192.168.40.1    |
| PC1    | DHCP              | Assigned automatically | 255.255.255.0 | 192.168.40.1    |
| PC2    | DHCP              | Assigned automatically | 255.255.255.0 | 192.168.40.1    |

The server uses a static IP address so clients can consistently reach the DHCP and DNS services. Client addresses are assigned dynamically from the DHCP pool.

## 9. Project Implementation

### Step 1: Create the Network Topology

Open Cisco Packet Tracer and add:

- One router.
- One switch.
- One server.
- Two or more PCs.

Connect the router to the switch using a copper straight-through cable.

Connect the server and all PCs to the switch.

Example connections:

| Source device | Source port   | Destination device | Destination port |
| ------------- | ------------- | ------------------ | ---------------- |
| Router        | G0/0          | Switch             | Fa0/1            |
| Server        | FastEthernet0 | Switch             | Fa0/2            |
| PC1           | FastEthernet0 | Switch             | Fa0/3            |
| PC2           | FastEthernet0 | Switch             | Fa0/4            |

The interface names may differ depending on the devices you select.

### Step 2: Configure the Router

Open the router CLI and enter:

```text
enable
configure terminal
interface gigabitEthernet 0/0
ip address 192.168.40.1 255.255.255.0
no shutdown
exit
end
```

The router interface now acts as the default gateway for the company network.

Save the configuration:

```text
copy running-config startup-config
```

Verify the interface:

```text
show ip interface brief
```

Make sure the configured interface is operational.

### Step 3: Configure the Server's Static IP Address

Select the server and open:

`Desktop → IP Configuration`

Choose **Static** and enter:

```text
IP Address:      192.168.40.2
Subnet Mask:     255.255.255.0
Default Gateway: 192.168.40.1
DNS Server:      192.168.40.2
```

The server uses its own IP address as its DNS server because it will provide DNS service.

The server must keep this static address so clients can find the services it provides.

### Step 4: Configure the DHCP Service

Select the server and open:

`Services → DHCP`

Turn the DHCP service **On**.

Create a DHCP pool with these settings:

| Setting                 | Value         |
| ----------------------- | ------------- |
| Pool Name               | COMPANY       |
| Default Gateway         | 192.168.40.1  |
| DNS Server              | 192.168.40.2  |
| Start IP Address        | 192.168.40.10 |
| Subnet Mask             | 255.255.255.0 |
| Maximum Number of Users | 100           |

Enter the settings in the appropriate fields and click **Add** or **Save**, depending on your Packet Tracer version.

The DHCP pool will assign client addresses starting from `192.168.40.10`.

The server's address, `192.168.40.2`, is outside the dynamic pool, so DHCP will not assign that address to a client.

### Step 5: Configure PC1 to Use DHCP

Select PC1 and open:

`Desktop → IP Configuration`

Click **DHCP**.

Wait for the computer to receive its network settings.

PC1 should receive an address from the configured DHCP pool, along with the subnet mask, default gateway, and DNS server address.

### Step 6: Configure PC2 to Use DHCP

Select PC2 and open:

`Desktop → IP Configuration`

Click **DHCP**.

PC2 should receive a different IP address from the DHCP pool.

For example, PC1 may receive `192.168.40.10`, and PC2 may receive `192.168.40.11`. The actual addresses depend on the order in which clients request them.

### Step 7: Configure the DNS Service

Select the server and open:

`Services → DNS`

Turn the DNS service **On**.

Add a DNS record with these settings:

| Setting | Value                |
| ------- | -------------------- |
| Name    | server.company.local |
| Type    | A Record             |
| Address | 192.168.40.2         |

Click **Add** or **Save** to create the record.

The DNS record links the name `server.company.local` to the server's IP address.

When a client asks for the IP address associated with this name, the DNS service should return `192.168.40.2`.

## 10. Testing and Verification

Testing confirms whether the network and services work correctly.

### Test 1: Verify DHCP on PC1

Open PC1 and select:

`Desktop → IP Configuration`

Confirm that the computer has received:

- An IP address in the configured DHCP range.
- Subnet mask `255.255.255.0`.
- Default gateway `192.168.40.1`.
- DNS server `192.168.40.2`.

Repeat this check on PC2.

### Test 2: Test the Router Connection

On PC1, open:

`Desktop → Command Prompt`

Enter:

```text
ping 192.168.40.1
```

Expected result: PC1 receives replies from the router.

This confirms that PC1 can communicate with the router's LAN interface.

### Test 3: Test the Server Connection

From PC1, enter:

```text
ping 192.168.40.2
```

Expected result: PC1 receives replies from the server.

This confirms basic IP connectivity between the client and server.

### Test 4: Test Communication Between PCs

From PC1, enter:

```text
ping 192.168.40.11
```

Use the actual IP address assigned to PC2 if DHCP assigned a different address.

Expected result: PC1 receives replies from PC2.

### Test 5: Test DNS Name Resolution

From PC1's Command Prompt, enter:

```text
ping server.company.local
```

If DNS is working, the output should resolve the hostname to:

```text
192.168.40.2
```

A successful ping also confirms IP connectivity to the server.

If the name resolves but the ping fails, investigate connectivity separately. Name resolution and successful ping replies are two different tests.

## 11. Troubleshooting

| Problem                                | Possible cause                                         | Solution                                        |
| -------------------------------------- | ------------------------------------------------------ | ----------------------------------------------- |
| PC does not receive an IP address      | DHCP service is off or the pool is incorrect           | Check the DHCP service and pool settings        |
| PC receives an incorrect gateway       | Wrong gateway entered in the DHCP pool                 | Set the gateway to `192.168.40.1`               |
| PC cannot reach the server             | Incorrect IP settings or physical connection           | Check the cables and IP configuration           |
| DNS name does not resolve              | DNS service is off or the record is incorrect          | Check the DNS service and A record              |
| PC cannot ping the router              | Incorrect IP address, subnet mask, or router interface | Verify the client settings and router interface |
| Both PCs receive unexpected addresses  | DHCP pool configuration is incorrect                   | Check the starting address and pool size        |
| DNS name resolves to the wrong address | Incorrect DNS record                                   | Update the record to `192.168.40.2`             |

When troubleshooting, check the physical connections first, then the IP configuration, DHCP service, DNS service, and connectivity tests.

## 12. Expected Results

When the project is configured correctly:

1. The router connects to the switch.
2. The server uses its static IP address.
3. DHCP automatically assigns IP addresses to client computers.
4. Clients receive the correct subnet mask and default gateway.
5. Clients receive the correct DNS server address.
6. Computers can communicate with the router and server.
7. The DNS service resolves `server.company.local` to `192.168.40.2`.
8. The network passes the required connectivity tests.

These are expected results. Record your actual results after completing the configuration.

## 13. Skills and Knowledge Gained

This project develops practical skills in:

- Cisco Packet Tracer.
- Server configuration.
- DHCP configuration.
- DNS configuration.
- Static and dynamic IP addressing.
- Default gateway configuration.
- Automatic IP address assignment.
- DNS name resolution.
- Client/server communication.
- Network testing.
- Troubleshooting DHCP and DNS problems.

## 14. Benefits of the Project

### Reduced Administrative Work

DHCP reduces the need to configure every computer manually.

### Fewer IP Configuration Errors

A correctly configured DHCP service reduces the chance of duplicate addresses and incorrect network settings.

### Easier Server Access

DNS allows users to access the server by name instead of remembering its IP address.

### Centralized Network Services

The server provides important network services from one location, making them easier to manage.

### Career Development

The project provides practical experience with services commonly used in business networks and system administration.

## 15. Project Limitations

This project demonstrates DHCP and DNS on a single local network.

It does not include:

1. Multiple subnets or VLANs.
2. DHCP relay configuration.
3. Internet connectivity.
4. Redundant DHCP or DNS servers.
5. Advanced DNS records or external name resolution.
6. Access control lists or advanced security.
7. Centralized user management.

These features can be introduced in later projects.

## 16. Future Improvements

The network can be improved by:

1. Adding VLANs to separate departments.
2. Configuring DHCP for multiple subnets.
3. Adding a second DNS server for redundancy.
4. Configuring ACLs to control access to services.
5. Adding a company web server.
6. Connecting a branch office.
7. Implementing secure device administration with SSH.

## 17. Conclusion

The Company Network with DHCP and DNS project demonstrates how network administrators can automate IP address assignment and make server access easier.

The DHCP service automatically provides network settings to client computers, while DNS translates the company server's hostname into its IP address.

By completing this project, students gain practical knowledge of server configuration, DHCP, DNS, automatic IP addressing, client/server communication, and network troubleshooting.

These skills provide a foundation for more advanced networking and system administration projects.

## 18. Project Evidence

Add screenshots of your actual work to demonstrate that you completed the project.

Recommended screenshots:

1. Completed network topology.
2. Router interface configuration.
3. Server IP Configuration.
4. DHCP service and pool settings.
5. PC1 receiving an address through DHCP.
6. PC2 receiving an address through DHCP.
7. DNS service showing the A record.
8. Successful ping to the router.
9. Successful ping to the server.
10. Successful hostname resolution for `server.company.local`.

Save your Packet Tracer file as:

`Project3-Company-Network-DHCP-DNS.pkt`

**Project status:** Complete after you configure the devices, run the tests, and capture the evidence.
