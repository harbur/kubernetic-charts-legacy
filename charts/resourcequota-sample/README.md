# Sample Resource Quota

This chart installs `object-counts` Resource Quota.

Once installed the Cluster's Namespace will have hard limits how many objects can exist on that Namespace.

For example there can only exist 4 Pods on the Namespace. If the number of existing Pods exceeds the hard limit, no actions will be taken to existing Pods, but no more Pods will be able to be created. If the number of existing Pods is less than the hard limit, Pods will be able to be created up to the number of hard limit.
