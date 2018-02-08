Operations
============

Upgrading Romana on Kubernetes
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Romana can be upgraded by simply updating the conatiner image used in the deployment and/or daemonsets. The following commands update the Romana container images to the latest images.

.. note::  Be sure to use ``kubeclt`` commands that reference the specfic release you need. Commands shown below reference v2.0.2 only as an example.

.. code:: bash

 kubectl -n kube-system set image deployment/romana-daemon romana-daemon=quay.io/romana/daemon:v2.0.2
 kubectl -n kube-system set image deployment/romana-listener romana-listener=quay.io/romana/listener:v2.0.2
 kubectl -n kube-system set image daemonset/romana-agent romana-agent=quay.io/romana/agent:v2.0.2

Upgrading the `romana-agent` requires the additional step of changing the "update strategy" from the default `OnDelete` to `RollingUpdate`. 

This is done by running

.. code:: bash
        
   kubectl -n kube-system edit daemonset romana-agent

Then changing `OnDelete` to `RollingUpdate`.

For upgrades from preview.3 to v2.0 GA, no etcd data migration is necessary.

Upgrading on AWS
----------------

When upgrading Kubernetes clusters running in EC2 it is also necessary to upadate the ``aws`` and ``vpcrouter-romana-plugin`` container images.

.. code:: bash

 kubectl -n kube-system set image deployment/romana-aws romana-aws=quay.io/romana/aws:v2.0.2
 kubectl -n kube-system set image deployment/romana-vpcrouter romana-vpcrouter=quay.io/romana/vpcrouter-romana-plugin:latest

Upgrading Route Publisher
-------------------------

If you are running the Route Publisher the ``bird`` and ``route-publisher`` container images need to be upgraded as well.

.. code:: bash

 kubectl -n kube-system set image deployment/romana-bird romana-bird=quay.io/romana/x-bird:v2.0.2
 kubectl -n kube-system set image daemonset/romana-route-publisher romana-route-publisher=quay.io/romana/x-route-publisher:v2.0.2

Romana Command Line Tools
~~~~~~~~~~~~~~~~~~~~~~~~~

Since Romana is controlled via a cloud orchestration system, once it is installed and running requires little operational oversight. However, for certain adminstrative functions a CLI is provided. 

.. note::  The Romana CLI retains certain Romana v1.x elements, including commands to directly administer default (i.e. pre-defined) `tenant` and `segment` labels. An update to the CLI will soon be available that allows for dynamic label creation.

Romana command line tools provide a romana API reference implementation. 
They provide a simple command line interface to interact with Romana
services.

Setting up CLI
--------------

**./romana** CLI uses a configuration file ~/.romana.yaml which contains
various parameters for connecting to root service and root service port.
A sample ~/.romana.yaml file looks as follows:

.. code:: yaml

    $ cat ~/.romana.yaml 
    #
    # Default romana configuration file.
    # please move it to ~/.romana.yaml
    #
    RootURL: "http://192.168.99.10"
    RootPort: 9600
    LogFile: "/var/tmp/romana.log"
    Format: "table" # options are table/json 
    Platform: "openstack"
    Verbose: false

Basic Usage
-----------

Once a configuration is setup (by default the romana installer will
populate the ~/.romana.yaml with a valid configuration), running the
*romana* command will display details about commands supported by
romana.

.. code:: bash

    Usage:
      romana [flags]
      romana [command]

    Available Commands:
      host        Add, Remove or Show hosts for romana services.
      tenant      Create, Delete, Show or List Tenant Details.
      segment     Add or Remove a segment.
      policy      Add, Remove or List a policy.

    Flags:
      -c, --config string     config file (default is $HOME/.romana.yaml)
      -f, --format string     enable formatting options like [json|table], etc.
      -h, --help              help for romana
      -P, --platform string   Use platforms like [openstack|kubernetes], etc.
      -p, --rootPort string   root service port, e.g. 9600
      -r, --rootURL string    root service url, e.g. http://192.168.0.1
      -v, --verbose           Verbose output.
          --version           Build and Versioning Information.


Host sub-commands
-----------------

Adding a new host to romana cluster
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Adding a new host to romana cluster should be done using `static hosts <https://github.com/romana/romana/blob/romana-1.x/static_hosts.md>`__ and this feature is only avaiable here for debugging assistance.

::

    romana host add [hostname][hostip][romana cidr][(optional)agent port] [flags]

Removing a host from romana cluster
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

::

    romana host remove [hostname|hostip] [flags]

Listing hosts in a romana cluster
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

::

    romana host list [flags]

Showing details about specific hosts in a romana cluster
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

::

    romana host show [hostname1][hostname2]... [flags]

Tenant sub-commands
-------------------

Create a new tenant in romana cluster
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Creating a new tenant is only necessary on certain platforms like
openstack (where the tenant has to exist previously on that platform),
for platforms like kubernetes, tenants are created automatically and no
command line interaction is needed in those cases.

::

    romana tenant create [tenantname] [flags]

Delete a specific tenant in romana cluster
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

::

    romana tenant delete [tenantname] [flags]

Listing tenants in a romana cluster
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

::

    romana tenant list [flags]

Showing details about specific tenant in a romana cluster
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

::

    romana tenant show [tenantname1][tenantname2]... [flags]

Segment sub-commands
--------------------

Add a new segment to a specific tenant in romana cluster
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Adding a new segment to a specific tenant is only necessary on certain
platforms like openstack, for platforms like kubernetes, segments are
created automatically and no command line interaction is needed in those
cases.

::

    romana segment add [tenantName][segmentName] [flags]

Remove a segment for a specific tenant in romana cluster
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

::

    romana segment remove [tenantName][segmentName] [flags]

Listing all segments for given tenants in a romana cluster
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

::

    romana segment list [tenantName][tenantName]... [flags]

Policy sub-commands
-------------------

Sample Romana Policy
^^^^^^^^^^^^^^^^^^^^

A sample romana policy is shown `here <./policies.html#policy-definition-format>`__.

Add a new policy to romana cluster
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Adding policies to romana cluster involves them being applied to various
backends like openstack VMs, Kubernetes Pods, etc for various platforms
supported by romana.

::

    romana policy add [policyFile] [flags]

Alternatively policies can be added using standard input.

::

    cat policy.json | romana policy add

Remove a specific policy from romana cluster
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

::

    romana policy remove [policyName] [flags]
    Local Flags:
        -i, --policyid uint   Policy ID

Listing all policies in a romana cluster
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

::

    romana policy list [flags]

.. _here: ../policy/policy.sample.json

