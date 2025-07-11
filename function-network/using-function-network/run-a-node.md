# Node Operators

This guide will walk you through the process of setting up and running a Function Network node. You can run a node either as a standalone binary or using Docker.

## Getting Started

Regardless of how you choose to run the node, there are three initial steps to get started:

1.  **Get FUNC Tokens**: To stake your node, you'll need FUNC tokens. You can get testnet tokens from the [Function Network Faucet](https://www.function.network/faucet).
2.  **Start the Node & Get Your Node ID**: Follow one of the guides below to start your node. When you start the node, it will output your unique Node ID.
3.  **Stake Your Node**: Once you have your Node ID, go to the [staking page](https://www.function.network/provide) to stake it on the network.

## Running as a Binary

For users who prefer to run the node directly on their machine:

1.  **Install and Start the Node**: Use the following command to install and start the node. This will also provide your Node ID.
    ```bash
    brew tap fxnlabs/homebrew-tap && ./fxn start --tui
    ```
2.  Proceed to stake your node as described in the "Getting Started" section.

## Running with Docker

For users who prefer to use Docker, we provide images for different hardware configurations.

1.  **Pull the Docker Image**:
    *   For **Nvidia GPUs (CUDA acceleration)**:
        ```bash
        docker pull ghcr.io/fxnlabs/function-node:0.0.34-cuda
        ```
    *   For **standard hardware**:
        ```bash
        docker pull ghcr.io/fxnlabs/function-node:0.0.34
        ```
2.  **Run the Docker Container**: Start a container with the image you pulled. This will start the node and give you your Node ID.
3.  Proceed to stake your node as described in the "Getting Started" section.

## Open Source

The Function Network node software is fully open source. You can view the source code, contribute, and track development on our [GitHub repository](https://github.com/fxnlabs/function-node).
