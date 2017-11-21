Network and Security Automation
===============================

Romana is a network and security automation solution for cloud native
applications.

-  Romana automates the creation of isolated cloud native networks and
   secures applications with a distributed firewall that applies access
   control policies consistently across all endpoints (pods or VMs) and
   services, wherever they run.
-  Through Romana's topology aware IPAM, endpoints receive natively
   routable addresses: No overlays or tunnels are required, increasing
   performance and providing operational simplicity.
-  Because IP addresses are assigned with network topology in mind,
   routes within the network are highly aggregated, reducing the impact
   on networking hardware, and allowing more secure configurations.
-  Supports Kubernetes and OpenStack clusters, on premise or on AWS.

Getting Started
---------------

To get started with Romana on Kubernetes, go
`here <docs/kubernetes/intro#getting-started>`__.

For OpenStack installations, please contact us by email or on Slack.

We are working on more detailed documentation to cover all the features
and installation methods. Reach out to the team via email, Slack or
GitHub if you need some help in the meantime.

Additional documentation
------------------------

-  `Romana core concepts and terminology <docs/romana/intro>`__:
   Find out how Romana is different and how it accomplishes simplified
   routing for endpoints.
-  `Romana's topology configuration <docs/romana/topology>`__:
   Explanation and examples of how to configure Romana for different
   networking environments.
-  `Romana VIPs <docs/romana/vips>`__: External IPs for Kubernetes
   clusters, managed by Romana with automatic failover.
-  `Romana DNS <docs/romana/dns>`__: How to setup DNS for Romana
   VIPs.
-  `Romana network policies <docs/romana/policies>`__: Introduction
   to Romana network policies.
-  `Romana route publisher <docs/romana/route_publisher>`__: In
   routed L3 networks, the route publisher announces the necessary
   routes either via BGP or OSPF.
