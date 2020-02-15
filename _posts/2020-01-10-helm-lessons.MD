# Helm lessons
Helm being the defacto package manager for kubernetes, you like it not or you will end up using it. I have nothing against it but sometimes `helm` deceives you, I keep those issues that i faced for my reference here.

## Issues

### helm --debug/dry-run ?
Noticed the output that debug prints when running with dry-run is not the same as what happens with the real run. I don't remember exactly and don't have an example now. But watch out

### helm --debug with --wait?
No debug output on timeout.


### Why helm --wait is not waiting for the  deployment?
The *helm install* document states the command will wait until the timeout for pods, pvcs, service and the **_minimum number of pods_** of the deployment to be ready.

 But that is not what is happening in my case, why it did not wait?

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
For my case, status.Ready.replicas=0 and spec.replicas=1 , maxUnavailable=1, so the code return `true` and helm was not waiting for the deployment.

After changing the change the upgradeStrategy, maxUnavailable = 0 it worked, but i assumed this is used only during the upgrade case. Seem like a bug to me but i haven't check the bugtracker.

### helm upgrade --resue-values | new-values?
I will expand it later, but probably it some misunderstanding the i have on how the helm computes the new values( also notice with --dry-run sometimes you don't get what will happen)

Also beware of nested yaml values, and setting of null values.

### helm error diagnostic messages are not so obvious?

The following are list of issues that i encountoured, lately I after an upgrade of the kubernetes version, i was not to able to connect to tiller.

  ```bash
    kubectl get pods -n kube-system | grep tiller
  ```
   The command above showed that the tiller is still running, But why i wasn't not able to connect to the server.

   Attempted a reinstall of the helm client/tiller components,
   ```
    rm -rf ~/.helm
    helm init --upgrade ( the default namespace is kube-system only)
   ```
  It was complaining about `cannot install into empty namespace`, I couldn't guess the issue, installing into empty namespace but should this be kube-system and also a read a note somewhere saying **TILLER_NAMESPACE** and it seems to be the problem for my case. I had an empty value for this parameter and that was causing the issue.

  Could have been obvious if it had siad the tiller namespace is not valid?
