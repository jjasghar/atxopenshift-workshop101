# FAQ

This page would list all know questions and potential problems, with possible answers or remedies.

## Log in to IBM Cloud

If you have created an IBM Cloud account before as Pay-as-You-Go account you might be needing to create a new account.
You can't create a new account

Ask for whitelisting, or use your own smart phone with data plan (do not use local WiFi).

### I don't see the Web Console of OpenShift.

Try restarting the VPN pod with the following command (log in to the environement using CLI token):

```bash
kubectl delete pod -l app=vpn -n kube-system --wait=false
```

### I see an error while deploying my image to an internal container registry

Try restarting the VPN with the following command:

```bash
kubectl delete pod -l app=vpn -n kube-system --wait=false
```

If that doesn't work please rebuild a cluster, or ask to get a new cluster.
