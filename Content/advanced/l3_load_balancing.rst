Layer 3 Load Balancing
~~~~~~~~~~~~~~~~~~~~~~

If you run your Kubernetes cluster on premises on a Layer 3 network that supports ECMP load balancing, Romana can be configured with `MetalLB <https://metallb.universe.tf/>`__ to load balance traffic across the nodes. MetalLB is an open source load balancer that configures routers to distribute traffic across available nodes. It assigns IP addresses to Services and announces a route to these IPs from multiple nodes. 

You must run Romana's Route Publisher to announce the pod network and also install and configure MetalLB to announce routes to Service IPs. MetalLB will `automatically expose <https://metallb.netlify.com/usage/>`__ services externally when ``spec.type`` is set to ``LoadBalancer``.

Installation
------------

MetalLB requires that its BGP speaker peer with Romana's Route Publisher where routes to Service IPs will be re-distributed upstream to the network router. Depending on your specific network requirements, other configuration changes may be required. Detailed installation and configuration instructions are available `here <https://metallb.netlify.com/configuration/romana/>`__
