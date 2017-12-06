Troubleshooting
===============

There are several simple tests you can perform to ensure the successful installation of Romana in your envrionment.

Verifying Installation
----------------------

Kubernetes
~~~~~~~~~~

Below are some simple commands you can use to verify Romana IPAM operations:

.. code:: bash

	# First try checking the pods and see if all of them
	# come up properly and are in running state. You may
	# want to wait few minutes before it settles, there
	# may be some restarts but eventually it does come up.
	kubectl get pods -a -o wide --all-namespaces
	#
	# once all pods are up, try installing cirros and see
	# if it comes up and if dns works correctly.
	kubectl run cirros --image=cirros --replicas=4
	#
	# check if the cirros pods are running
	kubectl get pods -a -o wide 
	#
	# once they are running login into them using
	kubectl exec -it <pod-name> /bin/sh
	#
	# once inside the pod, use nslookup to test dns
	nslookup kubernetes.default
	# the result would be as follows:
	#Server:    10.96.0.10
	#Address 1: 10.96.0.10 kube-dns.kube-system.svc.cluster.local
	#
	#Name:      kubernetes.default
	#Address 1: 10.96.0.1 kubernetes.default.svc.cluster.local
	#
	nslookup cirros-4036794762-9s46o
	#Server:    10.96.0.10
	#Address 1: 10.96.0.10 kube-dns.kube-system.svc.cluster.local
	#
	#Name:      cirros-4036794762-9s46o
	#Address 1: 100.112.11.131 cirros-4036794762-9s46o


