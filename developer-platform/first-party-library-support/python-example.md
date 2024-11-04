# Python Example

See our full reference implementation [here](https://github.com/fxnlabs/function-python-api-demo).

Note: This page assumes you are using MacOS or Linux. See the reference implementation for Windows instructions.

### Step 1: Create a virtual environment

```bash
python3 -m venv venv
source venv/bin/activate

# Add Buf registry to virtual environment
printf "
[global]
extra-index-url = https://buf.build/gen/python
" > venv/pip.conf
```

### Step 2: Add required dependencies

```bash
pip install fxnlabs-api-gateway-community-danielgtaylor-betterproto
pip install grpclib
```

### Step 3: Create the API Client

```python
api_key = 'MY_FXN_API_KEY'

channel = Channel('api.function.network')
client = APIGatewayServiceStub(channel, metadata=[('authorization', f'Bearer {api_key}')])
```

### Step 4: Make an API call

```python
async def main():
    # Create a text embedding
    response = await client.embed(
        model='baai/bge-small-en-v1.5',
        input='Embed me so I can use it for RAG Pipelines',
    )

    print(response)


if __name__ == "__main__":
    asyncio.run(main())
```
