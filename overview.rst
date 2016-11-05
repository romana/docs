Overview
========

Romana Cloud Native Networks are based on a new layer 3 tenancy model that encodes tenant and segment identifiers directly in the IP address. This enables multi-tenant cloud networks to be built without a virtual network overlay.

Romana extends the physical network hierarchy of a [layer 3 routed access design](/how/background/#routed-access-datacenter) from spine and leaf switches on to hypervisor hosts, VMs and containers. Romana then assigns each tenant and segment a physical network CIDR on every host. This lets the Linux kernel forward traffic directly to endpoints and enforce network isolation without the overhead of encapsulation. 

Advantages
----------

Another advantage of this approach is that route aggregation makes route distribution unnecessary and collapses the number of Linux *iptables* rules required for segment isolation.

Even though Romana uses a layer 3 tenancy model, it can run on layer 2 or layer 3 networks, as well as on public cloud networks like Amazon's VPC.

Romana includes an intelligent [IP Address Management](/how/romana_details/#ip-address-management) (IPAM) system that assigns IP addresses to VM and container endpoints that maintain the physical address hierarchy. A [Route Manager](/how/romana_details/#route-manager-and-host-agent) configures new routes on the hypervisor and adds firewall rules for tenant isolation and other traffic management policies. 


