# Capability <span style="float:right">![logo](monastery_logo_100.svg)<span>

Node capabilities are the core of any useful Monastery functionality. Under the assumption that we really cannot know what sort of distributed functionality are needed and what sorts of functionality will be invented in the future, Monastery makes no assumptions on what capabilities exist. Monastery allows to build up a custom made solution by composing multiple capabilities into a node, and then using those capabilities to make nodes perform useful work together.

The elementary lifecycle of a capability allows a capability to interact with its surrounding environment.

![Capability conceptual life cycle](capability_conceptual_life_cycle.svg)

* After the capability is added to a node it is bound to the node, so that it can maintain a reference to the node (if it wishes to). At this stage the final set of capabilities of the node is not yet known. If the capability needs other capabilities to function, it should wait until all capabilities have been added before attempting to resolve all of its dependencies.
* When all capabilities have been added to the node, but before the build is complete, the node or the builder will call onAllCapabilitiesBound() so that the capability may resolve its dependencies on other capabilities or on some aspect of the node implementation. At this stage the capability may know whether it can function or not, and may signal dissatisfaction via an exception.

