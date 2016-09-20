# Nginx Ingress

This chart deploys an Nginx Ingress Controller Daemon Set at `kube-system` namespace.

The Daemon Set makes sure that Nginx instance is run on all nodes. It is configured to listen to host ports `80` and `443`.

One Ingress Controller is handling traffic for all namespaces.

For more information about the Nginx Ingress Controller see documentation [here](https://github.com/nginxinc/kubernetes-ingress/tree/master/examples/complete-example).

# Usage Example

In order to test the Ingress controller, do the following steps:

* **Install `nginx-ingress` Chart**. This will create the Ingress Controller on `kube-system` namespace.
* **Install `coffee` Chart**. This will install the `coffee` and `tea` deployments on the current namespace.
* **Install `ingress-coffee` Chart**. This will install the `coffee` Ingress and the Secret used for HTTPS connection on the current namespace.
* **Configure your `/etc/hosts`**: Add `cafe.example.com` with the IP of one of your cluster nodes (With minikube do `minikube ip` to get the IP)
* Open https://cafe.example.com/coffee with your favorite browser.
* Open https://cafe.example.com/tea with your favorite browser.

The first URL redirects to the `coffee` Service and the second URL redirects to the `tea` Service.