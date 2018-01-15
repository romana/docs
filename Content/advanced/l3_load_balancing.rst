Layer 3 Load Balancing
~~~~~~~~~~~~~~~~~~~~~~

If you run your Kubernetes cluster on premises on a Layer 3 network that supports ECMP load balancing, Romana can be configured with MetalLB to load balance traffic across your nodes. You must run Romana's Route Publisher to announce the pod network and also install and configure MetalLB to announce Service IPs.

`MetalLB <https://metallb.universe.tf/>`__ is an open source load balancer that configures network devices to load balance across Kubernetes nodes. It assigns IP addresses to Services and announces a route to these IP addresses from multiple nodes. 

Detailed configuration instructions are available `here <https://master--metallb.netlify.com/configuration/romana/>`__
