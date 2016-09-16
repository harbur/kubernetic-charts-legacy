# Sample Persistent Volume

This chart installs a `pv0001` Persistent Volume in the Cluster.

This PV is hosted locally inside the Node where the Pod runs at the directory: `/data/tutorial/pv0001/`.

This means that if the Pod migrates to another Node it will loose the hosted data and start fresh, thus this PV is only recommended for development environments.

The PV is configured for storage capacity up to `2Gi`.
