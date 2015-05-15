# The Node <span style="float:right">![logo](monastery_logo_100.svg)<span>

The `Node` interface is the main interface between the application and the cluster. It is a representation of the local node's runtime cluster identity, and the interface via which the application obtains access to the cluster's capabilities.

The application obtains a reference to a `Node` from the implementing library or framework, and after that it uses it to communicate with the cluster by getting references to appropriate capabilities and using those.

![node model](node_and_application_relationships.svg)

The `Node` itself is very minimal. All it has is:

1. An ID
2. A method to get [`Capability`](Capabilities.md) object references of any implemented `Capability` interface.
3. An elementary state of the node in the cluster (non-functional)

## Node ID

The node ID represents the runtime identity of the local node in the cluster.

* The ID must be unique cluster-wide. This is enforced by the implementation.
* The ID may be `null` until the node is in a `JOINED` state
* The ID is provided by the implementation and the application may not have control over it (read-only)
* The ID may not change for at least as long as the node is joined to the cluster

Note that the ID may or may not be persistent, and it may or may not exist before the node is joined to the cluster.

## Node life cycle

The local node's states are very simple, and are not intended for complex state management. That is left for appropriate capabilities. The node has a very linear lifecycle:


![states](base_node_states.svg)

1. When the node starts out, it is in a `DISCONNECTED` state. At this stage, no node in the cluster knows about the local node, and it does not communicate with the cluster.
2. To join the cluster the `Node` is 'announced' to the cluster. Once announced, the node switches to an `ANNOUNCED` state. In this state, the node is still not usable as a cluster node, but the implementation is "working on it"
3. When the process of joining the cluster is complete, the node enters its final stage, which is `JOINED`. At this state, we know that the node's existence is now been known to the cluster and it's runtime ID has been established.

Note that importantly, being in a `JOINED` state says nothing about whether the node is usable, whether it can connect with the cluster, or whether it is in any way healthy or functional. What you *do know* is that *until* it is in a `JOINED` state, there is no point in trying to obtain or use any capabilities.

So joining the cluster, is basically a "green light" to start using its capabilities.

> **Note**
> 
> The node lifecycle described here is actually not built-in to the node. 
> It is a `Capability`, and like all capabilities it is both pluggable ad optional.
> Having said this, many capabilities rely on the node ID, and may need to know that the node is actually joined to the cluster and that the ID is valid in the cluster. So in practice, this is quite central. 
> 
> It is possible, however for an implementation to forgo implementing the node announcement capability and ignore this state model, or introduce a different one.

## Building a node

## Capabilities
Capabilities are discussed in more detail in the [capabilities page](Capabilities.md).

In short, the node itself provides no real functionality. It is just a container for pluggable capabilities, which are all entirely optional. Monastery is unopinionated about what capabilities a cluster should have, nor has it any opinion on what algorithms should be used or what their attributes should be.

So what is the point?

Monastery does provide a set of predefined capabilities, defined as interfaces, and their documented contracts. Those provide a template of sorts for Monastery implementations to provide a common view of the cluster with interchangeable implementations. This is similar to some of the javax APIs, which provide standard interfaces but no implementations, leaving it to the community to provide implementations.

While Monastery provides a set of standard capability interfaces, anyone can add additional capability interfaces without changing or breaking the Monastery API (implementation behavior can be broken, naturally).

In a separate project, a Monastery base implementation of some of the capabilities is provided to ease the development of implementations, but that is also not mandatory to use.

## Working with the `Node`
