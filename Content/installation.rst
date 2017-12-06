Installation
============

Fundamental to the operation of Romana is a valid representation of the network. This representation is captured in a `Network Topology <./networking.html>`__. Default network topologies are provided that are suitable for installation on premesis on flat networks and in all AWS EC2 regions.

For clusters created with ``kops`` or ``kubeadm`` with default settings, predefined YAML files are provided as well. If you are not using the default settings some changes to the YAML files may be required - see the `notes <#installation-in-other-environments>`__, below.

If you have your own custom installer for Kubernetes, or require a more complext network topology, please refer to the detailed `components <components.html>`__ page, and build a depoloyment manifest that includes details specific to your cluster. 


Installation using kubeadm
--------------------------

Follow the Kubernetes cluster configuration guide for `Using kubeadm to Create a Cluster <https://kubernetes.io/docs/setup/independent/create-cluster-kubeadm/#instructions>`__, and complete steps 1 and 2. Then, to install Romana, run

.. code:: bash

    kubectl apply -f https://raw.githubusercontent.com/romana/romana/master/docs/kubernetes/romana-kubeadm.yml

You will need to customize this manifest if: 

- you are using a non-default range for Kubernetes Service IPs 
- want to specify your own IP range for Pod IPs 
- are running in virtualbox - have cluster nodes in multiple subnets

See the `components <components.html>`__ page, for more detail on how to build a depoloyment that includes details specific to your cluster.

Installation with kops
----------------------

As of kops v1.8, Romana is a built-in CNI networking provider that can be installed directly by folloing the kops `documentation <https://github.com/kubernetes/kops/blob/master/docs/networking.md#supported-cni-networking>`__. 

If you install with kops, Romana v2.0.0 container images are pulled. Please check for the `latest Romana images <https://quay.io/repository/romana/daemon?tab=tags>`__ and what may have been added since the v2.0 release to see if an update is appropriate.

To update Romana on a running kops cluster 

Installation on earlier versions
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

If you are using an earlier version of kops, Romana can be installed by using the ``--networking cni`` option. You will need to SSH directly to your master node to install Romana after the cluster has finished launching.

.. code:: bash

    # Connect to the master node
    ssh admin@master-ip
    # Check that Kubernetes is running and that the master is in NotReady state
    kubectl get nodes

You should see output similar to the below example.

::

    NAME                                          STATUS            AGE       VERSION
    ip-172-20-xx-xxx.us-west-2.compute.internal   NotReady,master   2m        v1.7.0

Then, to install Romana, run

.. code:: bash

    kubectl apply -f https://raw.githubusercontent.com/romana/romana/master/docs/kubernetes/romana-kops.yml

It will take a few minutes for the master node to become ready, launch deployments, and for other minion nodes to register and activate.

You will also need to open port 4001 in the AWS Security Group for your "masters" instances. This can be edited in the AWS EC2 Management Console. Edit the rule for TCP Ports 1-4000 from "nodes", and change the
range to 1-4001.

The install for kops provides two additional components: 

- `romana-aws <./components.html#romana-aws>`__: A tool that automatically configures EC2 Source-Dest-Check attributes for nodes in your Kubernetes cluster 
- `romana-vpcrouter <./components.html#romana-vpcrouter>`__: A service that populates your cluster's VPC Routing tables with routes between AZs.

See the `components <components.html>`__ page, for more detail on how to build a custom depoloyment if your cluster requires

- a non-default range for Kubernetes Service IPs 
- a specific network CIDR for your pod IPs

Installation in other environments
----------------------------------

Please refer to the detailed `components <components.html>`__ page, or the `Advanced Topic <advanced.html>`__ for more detail on custom instalations.
