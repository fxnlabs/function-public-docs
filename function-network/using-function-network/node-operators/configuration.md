# Node Configuration and Management

This guide covers the essential commands and file structures for configuring and running a Function node.

## Configuration Files

The Function node uses a set of configuration files located in the `~/.fxn` directory in your user's home folder.

*   `~/.fxn/config.yaml`: The main configuration file for the node server.
*   `~/.fxn/model_backend.yaml`: Configuration for the model backend
*   `~/.fxn/nodekey.json`: Stores your node's private key.

## Starting the Node

You can start the node using:

```bash
fxn start
```

`nodekey.json` will created for you upon start if it wasn't created before.

### Text-based User Interface (TUI)

For a more interactive experience with easier debugging and state monitoring, you can start the node with a Text-based User Interface (TUI):

```bash
fxn start --tui
```

---

## Account Management

Before starting a node, you need to generate a node key. This key is used to identify your node on the Function Network.

### Creating a New Account

To generate a new node key, run the following command:

```bash
fxn account new
```

This command will create a `nodekey.json` file in your `~/.fxn` directory.

### Viewing Your Account

To display your existing node key and address, use:

```bash
fxn account show
```

---

## Configuration Details
For a detailed breakdown of the configuration options, please see the following sections:

*   [Node](./configuration/config.md)
*   [Model Backend](./configuration/model-backend.md)
