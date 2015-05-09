
[TOC]


# About the Monastery project

Monastery is a model for clustered computing.

It is made up of a set of interfaces, contracts, and documentation for putting together a useful cluster of working nodes. It is intended to simplify clustered computing in much the same way that the servlet specification helps putting together application servers. Much like it, Monastery separates the specification from the implementation. The Monastery project includes some full implementations of the interfaces, but those are not intended to be "reference implementations", but only "implementations".

Clustering technology and needs are very diverse and are still fast changing in 2015. Therefore, Monastery does not attempt to be authoritative with respect to the set of capabilities exposed by the interfaces and implementations. It is open to change and augmentation. While its core includes some key capabilities, the specification itself is extensible, and additional capabilities can be added without breaking the model. In this its nature differs from the servlet specification, which is a product of an authoritative committee.



## Motivation
The Monastery project came about after an unsatisfying search for a clustering solution that would solve some basic problems simply and without technology lock-in.
Some of the problems that I wanted to address are:

* Self-organization
* Service discovery
	* with a *good* load balancing solution. At best, solutions I found for service discovery had weak local load balancing, such as round-robin. None had a way to load balance based on global cluster knowledge of service node total behavior.
	* no central dependency, such as on a central database, and central load balancer
* Service configuration
	* with configuration changing in run time
	* and no centralized dependency
* Some ability to have some replicated data and state for the control plane
* Maybe some basic broadcasting capability, again for the control plane
* **Simplicity**
	* be able to use only the features and capabilities you need (not forced to use something bigger than you need)
	* no need to add specialized systems that your ops teams must learn and support
	* very easy learning curve, focused on using only the capabilities you need. If you don't need it: you don't really need to know about it.
* No technology lock-in (minimal opinion)
	* technology is moving fast in this area. New projects come up all the time. Don't get locked in to something that becomes quickly obsolete.
	* some organizations like particular technologies or have issues with some technologies. One should be able to easily swap out underlying implementations of capabilities without affecting the applications.

In short, I was not able to find anything coming close to even a small subset of this set of attributes. There are some fantastic projects out there, but none actually seems very practical when looked at closely. In real-world examination, all seemed to leave a good deal of development to get something we could adopt at my work. 

On examining the problem more closely, it appears that what is missing is not so much core, difficult technologies, but mostly a framework that stiches everything together in a sensible way.

## Principles
### Minimalism
#### Minimal opinion
* pretty much everything is optional
* minimal or no assumptions about underlying implementation capabilities
* anyone can add capabilities. No need to coordinate or update the framework.
* contracts of underlying implementations are not assumed beyond what they need to expose via the defined capabilities.
	* if they don't like the "standard" capabilities, they can always add their own. Although this may limit code portability and may even lead to a technology lock-in.
	* *importantly*, the framework makes no assumptions about the CAP approach of the underlying implementation. It does allow the implementation to make formal claims about it's CAP properties, so that application code can decide whether it's compatible with the implementation, or adapt to the CAP assumptions indicated by the implementation.
### Clarity and understandability
### Build on a foundation of pure interfaces and contracts
### What, not how
### Orthogonality
### Clear separation between the control plane and the data plane
form, technologies, solutions
### Separation of responsibilities and capabilities
### Extensibility

## Process
Aspiring to [C4](http://zguide.zeromq.org/page:all#toc141)

* It seems to work
* It makes sense
* I am too lazy to invent a new one and document it

## Authoring
The documentation project is written in Markdown, and it is built with [mkdocs](http://www.mkdocs.org/). It is organized as a separate [GitHub project](https://github.com/arnonmoscona/monastery-docs) from the [API](https://github.com/arnonmoscona/monastery-api) itself.

As per the [C4 process](http://zguide.zeromq.org/page:all#toc141), contributions should be made via forking and GitHub pull requests.

### Diagrams
At this stage, Diagrams should be authored using [Mermaid CLI](http://knsv.github.io/mermaid/mermaidCLI.html), exported to SVG or PNG and added as images. Ideally, should be able to embed mermaid diagrams directly into pages, but some hosting sites do not allow JS, and require completely static Markdown.