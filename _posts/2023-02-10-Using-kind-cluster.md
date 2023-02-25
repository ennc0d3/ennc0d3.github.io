## Using kind cluster


## How do i find the version of the kubernetes used in [[kind]]?

This is based on the assumption that you have running kind cluster and the kubeconfig is set to that, 

For example,
```
kind create cluster my-test-cluster
```

And assuming the **~/.kube/config** is the one that is update and is being pointed in the KUBECONFIG variable, We can do

```
kubectl version
```

With the above command we can see the server version, wondering why it is running an old version of k8s.

Also we can use the following command to get the version of the *node*, i.e the kubernetes server version used,

``` 
kubectl get nodes
```

Now the next problem is where do we see the correct version for the cluster to use? Ask help

```
kind create cluster --help
Creates a local Kubernetes cluster using Docker container 'nodes'

Usage:
  kind create cluster [flags]

Flags:
      --config string       path to a kind config file
  -h, --help                help for cluster
      --image string        node docker image to use for booting the cluster
      --kubeconfig string   sets kubeconfig path instead of $KUBECONFIG or $HOME/.kube/config
      --name string         cluster context name (default "kind")
      --retain              retain nodes for debugging when cluster creation fails
      --wait duration       wait for control plane node to be ready (default 0s)

Global Flags:
      --loglevel string   DEPRECATED: see -v instead
  -q, --quiet             silence all stderr output
  -v, --verbosity int32   info log verbosity
```

So they *--image* option is the thing to check and what is the default image used and how do i know that?

The [https://github.com/kubernetes-sigs/kind/releases](release notes) specifies the version the default version of the k8s used.

I think the first thing that i should have checked is the kind version itself, I had an old *kind* version 0.7.0 which is an old k8s version which is understandable. But first i need to update the version,

```
curl -Lo ./kind "https://kind.sigs.k8s.io/dl/v0.17.0/kind-$(uname)-amd64"
chmod +x ./kind
sudo mv ./kind /usr/local/bin/kind
```

[https://kind.sigs.k8s.io/docs/user/quick-start/#setting-kubernetes-version](Setting Kubernetes version)

Now that i have fixed the version, we can delete and create the cluster back again,

```
kind get clusters | xargs kind delete cluster --name 
```

```
 kind create cluster -n cm
```

And now it works,

```
kubectl get nodes
```

### Acessing services running on the Kind cluster

#### How do I access services/pod that are running in the Kind cluster?

I think we have these options,
* Port forwarding
* Adding a external loadbalancer like metallb
* Add routing

I will go with that last option, i.e adding route to the kind cluster via the docker bridge as it is 
convienent and easier of the other three options.

The source for this the following [Accessing services from host when using kind](https://dustinspecker.com/posts/resolving-kubernetes-services-from-host-when-using-kind/)

Get the IP address ranges allocated for the PODs in the cluster,

```
kubectl get node kind-control-plane \
  --output jsonpath='{@.spec.podCIDR}'
```

Get the control plane IP

```
docker container inspect kind-control-plane \
  --format '{{ .NetworkSettings.Networks.kind.IPAddress }}'
```

And finally, Add route,

```
sudo ip route add $(kubectl get node kind-control-plane \
  --output jsonpath='{@.spec.podCIDR}') via \
  $(docker container inspect kind-control-plane \
   --format '{{ .NetworkSettings.Networks.kind.IPAddress }}')
```

But this only lets you access PODs via their IPs, What about accessing Services which is usually the
case so we do the same thing except we use the "Services address range" instead

```
echo "sudo ip route add $(kubectl cluster-info dump |\
  sed -ne 's/.*.*--service-cluster-ip-range=\(.*\)".*/\1/p' | sed  -ne '$p' ) via \
  $(docker container inspect kind-control-plane \
   --format '{{ .NetworkSettings.Networks.kind.IPAddress }}')"
```
I have used *echo* to print the command to run, as it good to see the output before
executing.


### Custom Storage classes

#### How to do I create an non-custom storage class?

Define the custom storgage class,
```
cat > networkblock-storageclass.yaml  <<_EOD_                            
apiVersion: storage.k8s.io/v1
kind: StorageClass
metadata:
  annotations:
    storageclass.kubernetes.io/is-default-class: "false"
  name: network-block
  resourceVersion: "248"
provisioner: rancher.io/local-path
reclaimPolicy: Delete
volumeBindingMode: WaitForFirstConsumer
_EOD_
```

And create it,

```
kubectl apply -f networkblock-storageclass.yaml
```


