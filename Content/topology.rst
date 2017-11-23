Network Topology
================

To make Romana aware of important details of your network, it is
configured using a *network topology* configuration. This is a JSON
formatted file that describes the network(s) that will be used for
Kubernetes pods, and links them to the physical network that is hosting
them.

If you are deploying your Kubernetes cluster with a recognized tool such
as kops or kubeadm, your installation should use an existing predefined
topology. For other environments including customized installations and
baremetal deployments, the information about your networks will need to
be provided.

Network Topology Configuration Format
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Network Topology JSON
---------------------

.. code:: json

    {
        "networks": [ Network Definition, ... ]
        "topologies": [ Topology Mapping, ... ]
    }

-  ``networks`` (required)

A list of `Network Definition <#network-definition-json>`__ objects.
These describe the names of the networks and the CIDR that pod addresses
will be allocated from.

-  ``topologies`` (required)

A list of `Topology Mapping <#topology-mapping-json>`__ objects. These
link the network definitions and the topology of hosts within the
cluster.

Network Definition JSON
-----------------------

.. code:: json

    {
        "name": String,
        "cidr": IPv4 CIDR,
        "block_mask": Number
    }

-  ``name`` (required)

The name for this network. Each name must be unique. This name is used
to link network definitions and topology mappings.

-  ``cidr`` (required)

The IPv4 CIDR for pods created within this network. Each CIDR must be
unique, not overlapping with other values, and also not overlapping your
cluster's ``service-cluster-ip-range``.

-  ``block_mask`` (required)

The mask applied to `address
blocks <./networking.html##romana-address-blocks>`__. This must be longer than
the mask used for the CIDR, with a maximum value of 32. It implicitly
defines the number of addresses per block, eg: a value of /29 means the
address block contains 8 addresses.

Topology Mapping JSON
---------------------

.. code:: json

    {
        "networks": [ String, ... ],
        "map": [ Host Group, ... ]
    }

-  ``networks`` (required)

A list of network names. All values must match the name of an object
from the top-level ``networks`` list.

-  ``map`` (required)

A list of `Host Group <#host-group-json>`__ objects. This is a `topology
map <./networking.html#topology-map>`__ for the list of networks.

Host Group JSON
---------------

.. code:: json

    {
        "name": String,
        "hosts": [ Host Definition, ... ],
        "groups": [ Host Group, ... ],
        "assignment": { String: String, ... }
    }

-  ``name`` (optional)

A descriptive name for this mapping item.

-  ``hosts`` (conditional)

A list of `Host Definition <#host-definition-json>`__ objects. Only one
of "hosts" and "groups" can be specified.

-  ``groups`` (conditional)

A list of Host Group objects. Only one of "hosts" and "groups" should be
specified. This allows for nesting the definition of groups to match
your topology at each level, eg: spine and leaf. Nested groups are
treated as `prefix groups <./networking.html#prefix-groups>`__ for IP addressing
and routing..

An empty list may be specified. This indicates the lowest level of
grouping, but without defining hosts.

-  ``assignment`` (conditional)

A list of key-value pairs that correspond to Kubernetes ``node`` labels.
These are used to assign Kubernetes nodes to a specific Host Group. In
networks with multiple subnets, it is recommended that your Kubernetes
nodes use the appropriate ``failure-domain`` `lables <https://kubernetes.io/docs/reference/labels-annotations-taints/>`__, and matching those labels and values with the ``assignment`` in
your topology config.

Host Definition JSON
~~~~~~~~~~~~~~~~~~~~

.. code:: json

    {
        "name": String,
        "ip", String
    }

-  ``name`` (required)

The name of the host. Each name must be unique. This name must match the
node name registered in Kubernetes.

-  ``ip`` (required)

The IP address of the host. Each IP must be unique. This address must
match the node address registered in Kubernetes.

Examples
--------

-  `Network topology used for kubeadm
   installations <https://github.com/romana/romana/blob/master/containerize/targets/daemon/kubeadm-network.json>`__

This example defines a single network named ``romana-network``, and maps
to a topology containing 8 ``host-groups``. The empty groups are used as
placeholders, and Kubernetes nodes will be assigned to the host-groups
with round-robin placement.

-  `Network topology used for kops in us-west-1
   region <https://github.com/romana/romana/blob/master/containerize/targets/daemon/aws-us-west-1.json>`__

This example defined a single network named ``romana-network``, and
contains a host-group for each Availability Zone (AZ) within the
us-west-1 region. Inside each AZ host-group, there are 8 sub-groups with
``assignment`` labels specific to that AZ. Kubernetes nodes will be
assigned to one of those sub-groups based on round-robin placement after
matching the ``assignment`` labels.
