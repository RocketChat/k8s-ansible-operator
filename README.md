# Rocket.Chat Operator

This will launch an instance of the Rocket.Chat collaboration platform on your OpenShift cluster.
It is designed as a "push-button-go" operator, leveraging Ansible for the underlying logic.
The operator handles the provisioning of MongoDB from Red Hat Software Collections, and the deployment of Rocket.Chat from the Red Hat Container Catalog.

## Quick Instructions

To get started, you can use the included RocketChat custom resource template (provided when installing from OperatorHub), which will launch Rocket.Chat with the following options:

* 1 GiB storage volume for MongoDB
* 512 MiB memory limit each for MongoDB and Rocket.Chat
* Single pod instances of MongoDB and Rocket.Chat
* Automatically generated credentials for MongoDB, stored in a Kubernetes secret

## Custom Installation

The default CR template is enough to get started, but for a production environment you'll likely want to adjust your resource size accordingly.

Storage, memory and pod replicas can be adjusted by changing the respective fields in the RocketChat custom resource:

```
apiVersion: open.rocket.chat/v1alpha1
kind: RocketChat
metadata:
  name: rocketchat-custom
spec:
  mongodbReplicas: 1
  memoryLimit: 512Mi
  rocketchatReplicas: 1
  volumeCapacity: 1Gi
```

You can also install Rocket.Chat using an ephemeral storage volume by setting the `spec.volumeCapacity` field of the CR to an empty string:

```
apiVersion: open.rocket.chat/v1alpha1
kind: RocketChat
metadata:
  name: rocketchat-custom
spec:
  volumeCapacity: ""
```

If you would like to provide your own MongoDB credentials, you can create a Kubernetes secret with the following fields (`keyfile` is a base64 encoded, 756 character randomized string):

```  
username: 
password:
admin_password:
keyfile:
```

The name of the kubernetes secret you created above should be set in the RocketChat custom resource:

```
apiVersion: open.rocket.chat/v1alpha1
kind: RocketChat
metadata:
  name: rocketchat-custom
spec:
  mongodbSecretName: mongodb-secret-name
```

You can customize or add labels that will be applied by the operator to all of the created objects:

```
apiVersion: open.rocket.chat/v1alpha1
kind: RocketChat
metadata:
  name: rocketchat-custom
spec:
  labels:
    app: rocketchat
    env: production
    region: na-east
```

If desired, you can also customize the MongoDB database and replicaset names:

```
apiVersion: open.rocket.chat/v1alpha1
kind: RocketChat
metadata:
  name: rocketchat-custom
spec:
  mongodbDatabase: rocketchat
  mongodbReplicaName: rs0
```

