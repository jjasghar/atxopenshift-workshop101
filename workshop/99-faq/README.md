# FAQ

## Log in to IBM Cloud

If you have created an IBM Cloud account before as Pay-as-You-Go account, you might need to create a new account.

## You can't create a new account

Ask for whitelisting, or use your own smartphone with a data plan (do not use local WiFi <!-- a rationale here would probably be helpful -->).

### I don't see the Web Console of OpenShift.

Try restarting the VPN pod with the following command (log into the environment using your CLI token):

```bash
$ kubectl delete pod -l app=vpn -n kube-system --wait=false
```

### I see an error while deploying my image to an internal container registry

Try restarting the VPN with the following command:

```bash
$ kubectl delete pod -l app=vpn -n kube-system --wait=false
```

If that doesn't work please rebuild a cluster, or ask to get a new cluster.
