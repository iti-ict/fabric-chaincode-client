# Hyperledger Fabric Chaincode SDK for Node.js

Hyperledger Fabric is the operating system of an enterprise-strength permissioned blockchain network. For a high-level 
overview of the fabric, visit [http://hyperledger-fabric.readthedocs.io/en/latest/](http://hyperledger-fabric.readthedocs.io/en/latest/).

The Hyperledger Fabric Chaincode SDK for Node.js is an additional software component for the SDK already available for Node.js. The main goal
is to simplify the development of interactions with the network by adding an abstraction layer. Specifically this component provides these features:
    
* **Querying** a smart contract for the latest application state.
* **Submitting transactions** to a smart contract.
* Submitting transactions that include **private or sensitive data**.

## Installation

1. Download the [lastest release](https://gitlab.iti.upv.es/blockchain/blockchain-commons/fabric-chaincode-client/-/tags?utf8=%E2%9C%93&search=release-) rom our Github repository.
2. Install the downloaded release using the following format (replace X.Y.Z with the current release version)

```bash
npm install git+https://gitlab+deploy-token-58:aJqQG-gK3tJfMsHHngg3@gitlab.iti.upv.es/blockchain/blockchain-commons/fabric-chaincode-client.git#release-X.Y.Z
```

### Troubleshooting

If during installation there are some warnings related to `node-gyp' these are due to the 
`fabric-network` dependency. They are not harmful and can be ignored.

## Usage

The SDK consists of one class named `FabricChaincodeClient` that has to be instantiated. To instantiate the class is required the configuration of the network and the user that is going to enroll using a CA. An optional string representing the distinguished name attributes can 
also be used.

Once the class is instanciated, there are three methods  available: `query`, `querySecret` and `invoke`.

### API Reference

#### Constructor

```typescript
class FabricChaincodeClient {
    
    /**
     * @param config {IConfigOptions} - Specific configuration related to the user and the CA. This configuration is
     * used to enroll the user that is going to query/invoke the chaincode.
     * @param network {FabricClient | string | object} - Network configuration in the standard Hyperledger Fabric
     * format. See the docs of `fabric-client` or `fabric-network` for more information about this.
     * @param distinguishedNameAttributes {string} - Optional distinguished name attributes in string format to be used
     * in the Certificate Signing Request. By default `,C=US,ST=North Carolina,O=Hyperledger` is used.
     */
    constructor (
        config: IConfigOptions,
        network: FabricClient | string | object,
        distinguishedNameAttributes?: string
    ) { ... };
    
    ...

}
```

#### Available Methods

##### query

```typescript
    /**
     * Creates a wallet if it does not exist and proceeds to query a chaincode.
     *
     * The query itself is a wrapper for the queryInternal function. Arguments are forwarded.
     *
     * @param {string} channel - Name to identify the channel.SS
     * @param {string} smartContract - Smart contract name.
     * @param {string} transaction - Transaction name.
     * @param {string[]} args - Transaction function arguments.
     *
     * @return {Promise<string>} - Response or 200 OK if response is empty.
     */
    public async query(channel: string, smartContract: string, transaction: string,
                       ...args: string[]): Promise<string> {
        ...
    }
```

##### querySecret

```typescript
    /**
     * Creates a wallet if it does not exist and proceeds to query a chaincode.
     *
     * The query itself is a wrapper for the querySecretInternal function. Arguments are forwarded.
     *
     * @param {string} channel - Name to identify the channel.
     * @param {string} smartContract - Smart contract name.
     * @param {string} transaction - Transaction name.
     * @param {TransientMap} privateData - Object with String property names and Buffer property values.
     * @param {string[]} args - Transaction function arguments.
     *
     * @return {Promise<string>} - Response or 200 OK if response is empty.
     */
    public async querySecret(channel: string, smartContract: string, transaction: string, privateData: TransientMap,
                             ...args: string[]): Promise<string> {
        ...
    }
```

##### invoke

```typescript
    /**
     * Creates a wallet if it does not exist and proceeds to invoke a chaincode.
     *
     * The invoke itself is a wrapper for the invokeInternal function. Arguments are forwarded.
     *
     * @param {string} channel - Name to identify the channel.
     * @param {string} smartContract - Smart contract name.
     * @param {string} transaction - Transaction name.
     * @param {string[]} args - Transaction function arguments.
     *
     * @return {Promise<string>} - Response or 200 OK if response is empty.
     */
    public async invoke(channel: string, smartContract: string, transaction: string,
                        ...args: string[]): Promise<string> {
        ...
    }
```

### Configuration

As defined previously, the constructor of FabricChaincodeClient requires two JavaScript objects: one for the configuration of the network and a second one with the user that is going to enroll using a CA. An optional string representing the distinguished name attributes can 
also be used.

Examples of different configurations can be found in the folder [src/tests/](src/tests/).

#### Network Configuration

Network configuration in the standard Hyperledger Fabric format. See the docs of `fabric-client` or `fabric-network` for 
more information about this. An example can be found 
[here](https://hyperledger.github.io/fabric-sdk-node/release-1.4/tutorial-network-config.html).

It is common to find examples in the literacy of this configuration file written in YAML. In this project, the JSON
format has been selected as it works better with Node.js and involves less dependencies.

#### Enrollment Configuration

Specific configuration related to the user and the CA. This configuration is used to enroll the user that is going to 
query/invoke the chaincode.

Its definition can be found in [src/types/index.d.ts](src/types/index.d.ts).

#### Distinguished Name Attributes

Optional distinguished name attributes in string format to be used in the Certificate Signing Request. By default 
`,C=US,ST=North Carolina,O=Hyperledger` is used.


## Compatibility

The following table shows the tested versions of Fabric, Node and other dependencies and which are 
supported for use with version 1.4 of the Hyperledger Fabric Chaincode SDK for Node.js

|              | Tested       | Supported      |
|--------------|--------------|----------------|
| **Fabric**   | 1.4          | 1.4.x, 2.0.x   |
| **Node**     | 10, 12       | 10.13+, 12.13+ |
| **Platform** | Ubuntu 18.04 |                |
