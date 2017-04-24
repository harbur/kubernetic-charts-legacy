# Multi Container Pod

This chart will install a `multi-container` Pod.

The Pod contains two containers: `nginx` and `alpine`.

- The `nginx` container runs a simple Nginx instance on port `80`. 
- The `alpine` container executes a GET request on the Nginx every 2 secs using `wget`.

Because the two containers run under the same Pod, they run on the same node and also share the same Pod IP, thus the `alpine` instance simply executes requests against `localhost` and since they use the same IP, nginx responds properly.
