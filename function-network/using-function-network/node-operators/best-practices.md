# Best Practices

### Use a Reverse Proxy

It is highly recommended to expose your node through a reverse proxy, such as NGINX or HAProxy. This provides several benefits:

- **Efficient Connection Pooling**: A reverse proxy can manage incoming connections more effectively, reducing the load on your node.
- **Enhanced Security**: By acting as an intermediary, a reverse proxy can protect your node from direct exposure to the internet, mitigating potential security risks.
- **Customizability**: You can implement custom rules, caching, and other advanced features to further optimize your setup.

### For Nvidia Users

For optimal performance with Nvidia GPUs, we recommend leveraging our CUDA docker binaries for GPU acceleration. This will ensure that your hardware is utilized efficiently, providing faster processing and better overall performance.
