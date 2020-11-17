# fort-installer

## How to

Download the latest release from https://github.com/qlik-download/fort-installer/releases

```console
mv qliksense-darwin-amd64 qliksense
chmod +x qliksense
mv qliksense /usr/local/bin/
```

Just type `qliksense help` it will print help, please follow the steps to create fort

```console
qliksense cli tool provides functionality
	- create k3d kubernetes cluster
	- deploy qliksense operator in the cluster
	- install fort in the cluster

Prerequisites: please make sure following tools are in your system path
	- Docker
	- Kubectl (v1.18.8)
	- k3d

	# install k3d if not installed already
	qliksense tools download k3d

	# install kubectl if not installed already
	qliksense tools download kubectl

How to create Fort:
	# it will generate two files fort_private.key and fort_certificate.crt
	qliksense generate keypair

	# login to your tenant and create fort in Administration console, provide fort_certificate.crt content, fort url must be https
	# then add fort url in your host file, i.e. (/etc/hosts) like this 127.0.0.1 myfort.example
	# chose "File System Root" in the UI as the directory path where this installer has write access
	# execute following command to install fort
	qliksense install --acceptEULA yes --fortName <FORT_NAME> --fortPrivateKeyFile fort_private.key --fortTenantUrl <tenantUrl>

	# WARNING: It might take 5-20 minitues depending on your internet speed
	# execute following command to check the status of fort deployment
	qliksense status
	qliksense status -w

	# IMPORTANT: Once Fort is running, goto tenant url in your browser and do login, then open another tab https://myfort.example/sense/apps and accept the certificate

How to clean up:
	# To remove the fort deployment
	qliksense uninstall

	# To remove underlying kubernetes(k3d) cluster
	qliksense delete

Usage:
  qliksense [command]

Available Commands:
  apply       install qliksense based on provided cr file
  config      do operations on/around CR
  crds        crds for qliksense and operators
  delete      delete the kubernetes(k3d) cluster
  fetch       fetch a release from qliksense-k8s repo, if version not supplied, will use from context
  generate    generate keypair, config, etc for qliksense and operators
  help        Help about any command
  install     install a fort
  keys        keys for qliksense
  load        load a CR a file and create necessary structure for future use
  operator    Configuration for operator
  status      Check the status of fort if it is ready to use
  tools       support various operations for the tools dependency for qliksense
  uninstall   Uninstall the deployed qliksense.
  version     Print the version number of qliksense cli

Flags:
  -h, --help   help for qliksense

Use "qliksense [command] --help" for more information about a command.
```

## Clean up

`qliksense install` creates a kubernetes cluster on top of `k3d` and install fort in that cluster.

To uninstall fort 

- `qliksense uninstall` # it will remove the fort deploument from kubernetes cluster (k3d), So next time doing `qliksense install` will be faster as all the docker images are already present in cluster

To remove cluster

- `qliksense delete` # it will delete kubernetes cluster, so next time doing `qliksense install` will take as much time as it took initially
