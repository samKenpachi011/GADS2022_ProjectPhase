# Getting Started with VPC Networking

## Objectives
1. Explore the default VPC network

2. Create an auto mode network with firewall rules

3. Create VM instances using Compute Engine

4. Explore the connectivity for VM instances


## Task 1
1. Explore the default VPC network
#### Each Google Cloud project has a default network with subnets, routes, and firewall rules. The default network has a subnet in each Google Cloud Region
2. View subnets
3. View Routes
#### Routes tell VM instances and the VPC network how to send traffic from an instance to a destination
4. View the firewall rules
#### Each VPC network implements a distributed virtual firewall that you can configure. Firewall rules allow you to control which packets are allowed to travel to which destinations. Every VPC network has two implied firewall rules that block all incoming connections and allow all outgoing connections
5. Delete the Firewall rules
#### Navigate to and click on Firewall on the left pane. 
    5.1 Select all default network firewall rules.
    5.2 Delete the rules
6. Delete the default network
#### Navigate to and click on VPC Networks on the left pane. 
    6.1 Select the default.
    6.2 Delete the VPC network


## Task 2
#### Create a VPC network so that you can create VM instances
1. Navigate VPC network > VPC networks to create an auto mode network
2. For network name > mynetwork
3. For subnet creation mode use > Automatic (subnets will be created in each region automatically)
4. Select all available firewall rules
#### When the new network is ready, record the IP address range for the subnets in us-central1 and europe-west4. These will be referred to in the next steps.
5. Navigate to Compute Engine > VM instances through the navigation menu
#### Create a VM instance in the us-central1 region. Selecting a region and zone determines the subnet and assigns the internal IP address from the subnet's IP address range
6. Specify the following, and leave the remaining settings as their defaults:
    | Property               | Value                            |
    | ---------------------- | -------------------------------- |
    | Name                   | mynet-us-vm                      |
    | Region                 | us-central1                      |
    | Zone                   | us-central1-c                    | 
    | Series                 | N1                               |
    | Machine type           |f1-micro (1vCPU, 614MB memory)    |  

#### Hit the create button to create the instance

#### Create a VM instance in the europe-west4 region.
7. Specify the following, and leave the remaining settings as their defaults:
    | Property               | Value                            |
    | ---------------------- | -------------------------------- |
    | Name                   | mynet-eu-vm                      |
    | Region                 | europe-west4                     |
    | Zone                   | europe-west4-c                   | 
    | Series                 | N1                               |
    | Machine type           |f1-micro (1vCPU, 614MB memory)    |

#### Hit the create button to create the instance

#### Verify that the Internal IP for the new instance was assigned from the IP address range for the subnet in europe-west4 (10.164.0.0/20). The Internal IP should be 10.164.0.2 because 10.164.0.1 is reserved for the gateway and you have not configured any other instances in that subnet.


## Task 3
#### Explore the connectivity for VM instances
1. Navigate to Compute Engine > VM instances
#### Note the external and internal IP addresses for mynet-eu-vm.
2. For mynet-us-vm, click SSH to launch a terminal and connect.
3. Use the code below to reach the eu-vm. This is due to the mynetwork-allow-icmp.
    ```bash
        ping -c 3 <Enter mynet-eu-vm's internal IP here>
    ```

4. Remove the allow-icmp firewall rules
#### Navigate to VPC network > Firewall.
5. Select the mynetwork-allow-icmp rule and delete it.
6. To test connectivity to mynet-eu-vm's external IP, run the following command, replacing mynet-eu-vm's external IP:
    ```bash
        ping -c 3 <Enter mynet-eu-vm's external IP here>
    ```
#### The 100% packet loss indicates that you cannot ping mynet-eu-vm's external IP. This is expected after deleting the allow-icmp firewall rule!

7. Remove the allow-custom firewall rules
8. Navigate to VPC network > Firewall.
9. Select the mynetwork-allow-custom rule and delete it.
10. Return to the mynet-us-vm SSH terminal to test connectivity to mynet-eu-vm internal IP.
    ```bash
        ping -c 3 <Enter mynet-eu-vm's internal IP here>
    ```
#### The 100% packet loss indicates that you cannot ping mynet-eu-vm's external IP. This is expected after deleting the allow-custom firewall rule!


## Task 4
#### Removing the allow-ssh firewall rules
#### Navigate to VPC network > Firewall.
5. Select the mynetwork-allow-ssh rule and delete it.
#### Navigate to VPC network > Firewall.
#### Navigate to Compute Engine > VM instances.VPC network > Firewall. For mynet-us-vm, click SSH to launch a terminal and connect.5. Select the mynetwork-allow-icmp rule and delete it.

#### The Connection failed to SSH to mynet-us-vm because of the deleted allow-ssh firewall rule!

## End of lab