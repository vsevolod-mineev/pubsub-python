# PubSub Python

Welcome to the documentation for the Python SDK for the Data Mesh by SyntropyNet! This SDK allows seamless integration with our data mesh solution, enabling you to leverage real-time data streams in your Python applications. With the Python SDK, you can unlock the power of the Data Mesh and harness real-time insights for your data-driven projects.

[pubsub-python ](https://github.com/SyntropyNet/pubsub-python)is a Python example illustrating the use of the Syntropy DataMesh project, which facilitates subscription to existing data streams or publishing new ones. This example employs the NATS messaging system and offers a simpler starting point for integrating Python applications with the Syntropy DataMesh platform.

# Installation

To install the Python SDK for Data Mesh, you can use pip, the Python package manager. Here's an example of how to install it:

```shell
pip install syntropynet-pubsub
```

# Getting Started

Before you begin using the Python SDK, make sure you have the necessary credentials and access tokens from the SyntropyNet platform. These credentials will allow you to connect to the Data Mesh and subscribe to or publish data streams.

## Usage

1. Import the SDK:

```python
from syntropynet_pubsub import DataMeshClient
```

2. Initialize the client:

```python
client = DataMeshClient(access_token="your-access-token", private_key="your-private-key")
```

3. Subscribe to a Data Stream:

```python
stream = client.subscribe("stream-name")
```

4. Receive Data Stream Events:

```python
for event in stream.events():
    # Handle the data stream event
    print("Received event:", event)
```

5. Publish Data to a Stream:

```python
client.publish("stream-name", b"Hello, Data Mesh!")
```

## Examples

For detailed usage examples, please refer to the [examples directory](https://github.com/SyntropyNet/pubsub-python/examples) in the repository. These examples cover various scenarios and demonstrate how to utilize the SDK's features effectively.

The preferred authentication method is using an access token from the developer portal.

```Text Python
import asyncio
import nats
import tempfile
import os

from helper import create_app_jwt

access_token = "SAAGNJOZTRPYYXG2NJX3ZNGXYUSDYX2BWO447W3SHG6XQ7U66RWHQ3JUXM"

async def main():
    jwt = create_app_jwt(access_token)

    # Write the JWT to a temporary file with correct format
    with tempfile.NamedTemporaryFile(mode='w+t', delete=False) as temp:
        temp.write(jwt)
        temp_path = temp.name

    nc = await nats.connect("nats://127.0.0.1", user_credentials=temp_path)

    async def message_handler(msg):
        subject = msg.subject
        data = msg.data.decode()
        print("Received a message on '{subject}: {data}".format(
            subject=subject, data=data))
        # await nc.publish("syntropy.test.subject", msg.data)

    await nc.subscribe("syntropy.bitcoin.tx", cb=message_handler)

    # run infinitely
    while True:
        await asyncio.sleep(1)

    # Terminate connection to NATS.
    await nc.drain()

    # Delete the temporary file
    os.unlink(temp_path)

if __name__ == '__main__':
    asyncio.run(main())
```

# Contributing
We welcome contributions from the community! If you find any issues or have suggestions for improvements, please open an issue or submit a pull request on the [GitHub repository](https://github.com/SyntropyNet/pubsub-python). We appreciate your feedback and collaboration in making this SDK even better. 

## Contribution Guidelines

To contribute to this project, please follow the guidelines outlined in the [Contribution.md](CONTRIBUTING.md) file. It covers important information about how to submit bug reports, suggest new features, and submit pull requests.

## Code of Conduct
This project adheres to a [Code of Conduct](CODE_OF_CONDUCT.md) to ensure a welcoming and inclusive environment for all contributors. Please review the guidelines and make sure to follow them in all interactions within the project.

## Commit Message Format
When making changes to the codebase, it's important to follow a consistent commit message format. Please refer to the [Commit Message Format](commit-template.md) for details on how to structure your commit messages.

## Pull Request Template
To streamline the pull request process, we have provided a [Pull Request Template](pull-request-template.md) that includes the necessary sections for describing your changes, related issues, proposed changes, and any additional information. Make sure to fill out the template when submitting a pull request.

We appreciate your contributions and thank you for your support in making this project better!


# Support

If you encounter any difficulties or have questions regarding the Python SDK for Data Mesh, please reach out to our support team at support@syntropynet.com. We are here to assist you and ensure a smooth experience with our SDK.

We hope this documentation provides you with a comprehensive understanding of the Python SDK for the Data Mesh. Enjoy leveraging real-time data streams and unlocking the power of the Data Mesh in your Python applications!