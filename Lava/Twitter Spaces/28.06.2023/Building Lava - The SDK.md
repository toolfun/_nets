28.06.2023    
Twitter Spaces https://twitter.com/i/spaces/1ZkJzXeLzgWKv?s=20
# Building Lava - The SDK


### Introduction to Lava and the SDK
The conversation begins with an introduction to Lava, an open-source, community-led, decentralized RPC for all of Web Three. The focus of the discussion is the Lava SDK, which provides plug-and-play access to the public decentralized RPC from a browser or backend. The SDK is fully open-source and supports multiple protocols, including JSON, Rest, gRPC, and others. The SDK is also chain-agnostic, supporting any chains available on Lava.

### Unique Features of Lava SDK
The Lava SDK is distinguished by its peer-to-peer decentralized access, automated pairing functionality, multi-chain and multi-protocol support, built-in concurrency and subscriptions, and integrations with popular libraries. The peer-to-peer security feature allows decentralized access for RPC across different chains and architectures, enhancing reliability, privacy, and censorship resistance.

### How Lava SDK Communicates with Lava
The Lava SDK implements the Lava protocol and wraps around user requests for various API interfaces within gRPC web. The SDK uses a solution inspired by seed nodes to get a list of possible accountable providers for the Lava pairing mechanism. Once synced with the Lava blockchain, the SDK can track the pairing the consumer is getting on the desired service. The SDK then communicates within the Lava protocol protobuffs all the communication supported in the chosen service.

### Decentralization in Lava SDK
The Lava SDK was designed with decentralization in mind. Instead of using a URL, the SDK gets a list of seed nodes, an address book format for possible peers. This list is updated very rarely, and most of them will still be accountable. This initial seed file allows everything from there to be automatic within the SDK, providing a built-in solution for decentralization.

### Challenges in Building the Lava SDK
The main challenges in building the SDK were:

- Finding a solution to eliminate the need for a server that has to run forever.
- Handling the fact that a frontend running an SDK is fully compromised and cannot hold a secret key.
- Adapting the SDK to run in a browser in a single thread, given its high concurrency.

### Integration with Existing Libraries
To make the transition to using the Lava SDK as smooth as possible, the team worked on integrating it with existing libraries like Etherjs. The goal is to provide an easy switch to the Lava SDK, where users can simply switch the provider and everything will work as before, but without using a centralized RPC.

### Future Functionalities and Improvements
The team is working on several functionalities and improvements for the SDK, including a provider picker for performance tracking and optimization, session handling for reducing overhead on the provider side, WebSocket subscriptions, and integration with badges.

### Conclusion
The Lava SDK is currently in its early alpha phase, and the team is looking for developers to contribute and help improve the developer experience and flow. The SDK is integrated with the Lava gateway, allowing for decentralized access with a simple command on the gateway. The team encourages developers to test the SDK, provide feedback, and contribute to its development.
