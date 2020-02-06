## Issues
The following are list of issues that i encountoured, lately I after an upgrade
of the kubernetes version, i was not to able to connect to tiller.

These are the steps, I took to find the issue and resolve

- Checking the required helm client/server version and upgraded the client and the
  tiller on the default tiller-namespace (i.e kube-system)
```bash
    kubectl version
    helm version
```
  I have an issue with the helm client connection towards tiller, trying to reinstall the
  helm client and upgrade the tiller
```
    kubectl get pods -n kube-system | grep tiller
```
   The command above showed that the tiller is still running, But why i wasn't not able to
   connect to the server

   Lets reinstall the helm components,
   ```
    rm -rf ~/.helm
    helm init --tiller-namespace kube-system --upgrade ( the default namespace is kube-system only)
    ```
    The command reported success, and checked for the presence of the running tiller and it was fine

    >Trying again to use helm command
    ```
    helm version
    ```
    Again the same issue, Why it can't to tiller ?
    Redo the installation again,
    ```
    rm -rf ~/.helm
    helm init --upgrade
    ```
    This time without the tiller-namespace, so it should be kube-system namespacee, the command did not work
    It was complaining about `cannot install into empty namespace`, I couldn't guess the issue, installing into
    empty namespace but should this be kube-system. Why the default value of the tiller namespace is in effect.

    Now, I check and docs and see it could be related to TILLER_NAMESPACE variable, I check my env
    ```
    env | grep TILLER_NAMESPACE
    ```
    Ahha, I see its set to an empty value, I don't remember when but i was doing for an unrelated test but i think it
    left it in my bashrc, which screwed by helm.

    ```
    helm init --upgrade

    helm version
    ```

What is with --reuse-values, upgrade, How does helm merge values ?

Why does --wait doesn't wait for my deployment ?
 ```
    helm install --help
 ```
 The helm install command will wait until the timeout for pods, pvcs, service
 and the minimum number of pods of the deployment to be ready

 But that is not what happened in my case, why it did not wait?

  The helm check for the ready replicas against the minumum number of
available pods

  ```code
  func (c *Client) deploymentsReady(deployments []deployment) bool {
	for _, v := range deployments {
		if !(v.replicaSets.Status.ReadyReplicas >= *v.deployment.Spec.Replicas-deploymentutil.MaxUnavailable(*v.deployment)) {
			c.Log("Deployment is not ready: %s/%s", v.deployment.GetNamespace(), v.deployment.GetName())
			return false
		}
	}
	return true
}
```
 In my case, status.Ready.replicas=0 and spec.replicas=1 - maxUnavailabel=1

 0 >= 1 - 1 , which is true and helm wait does not wait.

What should I do now?
	- Change the upgradeStragey, maxUnavailable = 0

But what does this mean?
  The maximum number of pods that can be unavailable during an update process
  The value can't be 0, if the maxSurge is 0
	What is maxSurge? The maximum number of Pods that can be created over the desired nuber of
during the update process
process


 Take a look at the deployment manifest?
