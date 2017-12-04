Installation
============

For clusters created with ``kops`` or ``kubeadm`` with default settings, predefined YAML files are provided so that you can install easily by using ``kubectl apply``. If you are not using the default settings, some changes to the YAML files will be required - see the `notes <#installation-in-other-environments>`__, below.

If you have made your own customized installation of Kubernetes or used a different tool to create the cluster, then you should refer to the detailed `components <components.html>`__ page, and align the example configuration with the details specific to your cluster.

Installation using kubeadm
--------------------------

Follow the Kubernetes cluster configuration guide for `Using kubeadm to Create a Cluster <https://kubernetes.io/docs/setup/independent/create-cluster-kubeadm/#instructions>`__, and complete steps 1 and 2. Then, to install Romana, run

.. code:: bash

    kubectl apply -f https://raw.githubusercontent.com/romana/romana/master/docs/kubernetes/romana-kubeadm.yml

Please see special notes below if - you are using a non-default range for Kubernetes Service IPs - want to specify your own IP range for Pod IPs - are running in virtualbox - have cluster nodes in multiple subnets

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

Please see special notes below if - you are using a non-default range for Kubernetes Service IPs - want to specify your own IP range for Pod IPs

Installation in other environments
----------------------------------

Please refer to the detailed `components <components.html>`__ page, or the `Advanced Topic <advanced.html>`__ for more detail on custom instalations.
