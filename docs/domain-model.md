# The domain model <span style="float:right">![logo](monastery_logo_100.svg)<span>

[TOC]

This page discusses the domain model [loosely] in terms of Domain Driven Design. The intent is to create a common "language" to understand the domain objects, their roles, and how they interact in specific contexts. The details are listed in separate pages. This page is dedicated to a high level decomposition of the domain and some basic domain mapping, independently from any specific implementation of Monastery.

Terms in the domain ubiquitous language are highlighted in **bold** type.

<span id="domains"/>
# Domains and subdomains

Monastery is a generalized framework for abstractly representing a cluster as viewed from a member node. In some respects the relationship between Monastery and your code is similar to the relationship of an J2EE application server and your code. You can have different server implementations with varying features, but as long as you stick to the J2EE set of interfaces your code is independent from the specific container implementation you choose. As such, Monastery expresses several main domains:

* **Cluster**
	* The cluster as a whole. There can be several clusters in a full system.
* **Node**
	* A node is a member of a specific cluster. It can only belong to one cluster.
	* A process can have more than one node in it. Those can be several nodes of the same cluster or can be members of several clusters.
	* A node must have an identity, which identifies it in the cluster and can be known to other nodes. See more in the [node page](node.md)
* **Process**
	* A process is normally a single Java process, but the concept is not mapped into any programming entity in Monastery. It can be just a thread. It does not really matter.
* **Capability**
	* A [capability](Capabilities.md) represents things a node can do.
	* Capabilities are the core abstraction in Monastery and are what makes Monastery useful. They are expressed in an implementation-independent way and by restricting all cluster coding to be expressed solely through Monastery capabilities makes the code portable among different Monastery implementations that can be based on entirely different underlying enabling technologies.
	* A node is [mostly] a composition (aggregate) of capabilities.
	* Each capability represents some service that is accessible to code in the process that contains it. This maybe something that is executed locally, or remotely in another node, or perhaps by some centralized cluster component that is not itself a node. 
	* A capability generally can have several related pieces of functionality, and in some sense can be thought of as a subdomain.
	* Capabilities can come from several sources and do not all have to be implemented by the same implementation.
	* Some capabilities are very specific pieces of functionality.
		* Some are considered **core capabilities** as in most cases without you can't have distributed nodes that are members of a cluster without them. For instance, capabilities having to do with cluster membership. In principle, it is possible to create a functioning Monastery implementation that does not have these "core" capabilities. But at least the first implementations certainly do have them as a starting point for making a cluster function.
		* Some capabilities are defined in the Monastery API, which thus provides a common vocabulary for commonly needed functionality you may need a clustering solution to provide. All of those capabilities are pure interfaces (fully abstracted)
		* Monastery itself is extensible. One can easily add new capabilities that are not packaged as part of the Monastery APIs, and as long as they are kept abstract then portability is preserved in the sense that one can have multiple implementations. Useful capabilities (intrerfaces, not implementations) should be contributed back to the API so as to make them part of the common capability "vocabulary". There is no obligation to do so.
		* There is a rather lengthy list of "todo" capabilities to model and implement and they will gradually be added to the Monastery API.
* **Monastery implementations**
	* A Monastery implementation provides a concrete implementation for some subset of capabilities.
	* A **core implementation** it typically coupled to a specific code enabling technology provides some substantial enough subset of capabilities such that additional capabilities can be readily constructed as "portable implementations"
	* A **portable implementation** is a concrete implementation of some subset of capabilities, which are all composed by only depending on other purely abstract capabilities and do not introduce any dependencies on a particular enabling technology.
		* This portability is achievable by means of using core enabling capabilities, typically provided by a core implementation. See more in [Mixing and matching](#mixing-and-matching).
		* This is analogous to using a J2EE container and **composing** it with some JDBC driver and some JMS provider, neither of which come from the implementation of the container. So you may get a core implementation using Zookeeper, with many capabilities that may be provided directly by Curator and others using custom code, then composing it with a pure implementation of service discovery and load balancing that only depend on Monastery API capabilities. Later you might decide to switch the code implementation to one based on Hazelcast, and none of your application clustering code would be affected.

<span id="context-mapping"/>
# Context mapping

<span id="mixing-and-matching"/>
# Mixing and matching

----
![todo](construction.png)

**todo: describe Monastery as an anti-corruption layer**
**todo: describe capability bundles as bounded contexts**
**todo: describe mixing multiple implementations and composing portable capabilities**
**todo: describe mixing and matching when there are overlaps between multiple implementations**
**todo: Nodes are aggregate roots. So capabilities must be immutable. Capability mutation must be done by replacement. Replacement must be done by the node. But then what is a node supervisor? A factory?**
