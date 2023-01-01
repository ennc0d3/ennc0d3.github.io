# What is the heck is Kubernetes Operators?

## My first time with *K8s Operator*
Though i have used, i mean installed and used an application which is based of
this pattern, i couldn't really appreciate and understand the reasoning behind
this approach.

A service, oh no application that i was using is based on Apache Geode and i
didn't know the details but it seems like a consistent, high performant Db.
But, then installing could have been a nightmare, lucikly it was using
Operators, back then I didn't know what they are but apparantely it was 
convienent and easier to get the application up quickly without browsing
through the documentation. Here, am talking about and internal version of
this Db not the one from the public, may the public one also has this approach.

But at that time, i was curious and browsed few articles on
*Kubernetes Operators*, honestly much of that didn't make much sense to simple
brain. I was thinking why can't Helm solve them, why not their hooks, the
initContainers, the batch jobs etc.


## Why not Helm?

Not really i think now, Helm allows as to package dependencies but setting up
dependency order or prerequisites sort of things doesn't work very well. Typically,
an application's installation instructions comes with a *prerequisite* section 
which make us think, oh no this is not installable. How the hell i will address
these prerequisites quickly in my local environement? For example, we might have
seen installation instructions stating that this and that has to be installed, 
this resource must be created, the other service must be in ready state, db must 
be initialized,and blah blah.


## The adminstrator's nightmare

I hated those sort of applications as the burden is now on the
adminstrator of the application, now imagine maintaining it, for example
taking a backup, reconfiguring, etc

And developers would not typically provide very good documentation there are always
hidden assumption which turns out to be long debug sessions and endless reading
of documentation.

Now i think of automation, why not use something configuration
or provisioning tool to handle those sort of things, Ansible, Chef, etc.

## And oh maybe *Operators* can be the ones
Re-Reading the Kubernetes Operators documentation made me
realize that it -solves- addresses some of the shortcomings of Helm,
automation tools etc, by usign the K8s core concepts, Controller & Reconciler
(the control-loop).

So, if we can code the adminstration logic using the Kubernetes concepts we
get something called Operators. The Operators are Kubernetes way of coding the
application adminstrator (operator) knowledge in to the K8s system by using
Custom Resource Definitions (CRDs) and a Custom Controller for the CRD.

## An Un-typical Operator example

Typical, example used is memcached application, where as an user of the 
application is concerned about installing & configuring and maintaining it
which delving much into the internals.

For example, Let us assume our application is provides an Memcached Db and 
the provider is nice to code the adminstration logic as a CRD 
example SimpleCache) and has provide an Controller (SimpleCacheController) 
by installing/introducing these new types to the Kubernetes cluster(API) 
we can simply install the memcached Db as,

```
kubectl apply -f simplecache-db.yaml
```

Here let us assume the resource definition (spec) has a simple attributee saying
the size as in,

```
apiVersion: ennc0d3.databases/v1
kind: SimpleCacheDb
spec:
  size: 3

```

Now the operator is expected to handle the details, it might install PVC,
ConfigMaps, StatefulSets or Pods, Deployments, Service to bring up the 
application to an usable stable. Interestingly they might also bring support
services to handle periodic backups, scaling, reconfiguring etc.

## How do I write one?

So how shall we write operators, simple, use an framework that helps to define
these new Controller and CRDS. The [Operator framework](https://operatorframework.io/) provides an SDK which 
allows to define those types and generates an simple controller boiler plate
which is quite understandable. But it is best to learn about the K8s controller
model and understand the API objects, Watch concepts.


### (Not-So) Easy Piecey 
It is not straightforward given that is not easy to navigate through the k8s
documentation but we are not the first, this has been here for a while and there
are good blogs on how to do it *Copy*. 

Using the Operator framework can make easy, I haven't done it, but if i find time
to do something i will write an update here for my future self.



