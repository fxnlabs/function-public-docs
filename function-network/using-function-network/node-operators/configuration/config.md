# Node Configuration

This section details the configuration for the `node` section of the `config.yml` file.

A full configuration template can be found [here](https://github.com/fxnlabs/function-node/blob/main/fixtures/config/config.yaml.template).

```yaml
node:
  listenPort: 8080
  listenAddress: "127.0.0.1"
```

## Parameters

### `listenPort`

*   **Description**: The port on which the Function node will listen for incoming connections.
*   **Default**: `8080`
*   **Type**: `integer`

### `listenAddress`

*   **Description**: The address on which the Function node will listen for incoming connections. Using `127.0.0.1` (localhost) is recommended for security, as it restricts access to the local machine. To allow external access, you can set this to `0.0.0.0`. It is recommended that you expose the node through a reverse proxy, i.e NGINX or HAProxy
*   **Default**: `"127.0.0.1"`
*   **Type**: `string`