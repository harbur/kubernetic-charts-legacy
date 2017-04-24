# Ghost

This Chart installs Ghost on the cluster.

## Requirements

It needs an available Persistent Volume, so before installing this make sure to have one on the Cluster with at least `1Gb` of space.

Once installed the `ghost-claim` Persistent Volume Claim will attach to the PV and the `ghost` Deployment will use the PVC as storage unit.

