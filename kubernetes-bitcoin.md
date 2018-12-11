# Bitcoin and Kubernetes

This blog is the culmination of my efforts to run
[Bitcoin](https://bitcoin.org/en/bitcoin-paper) on
[Kubernetes](https://kubernetes.io/). My professional work involves using
[Ansible](https://www.ansible.com/) to deploy applications to Kubernetes, and I
was interested in using Ansible to deploy a containerized version of `bitcoind`
on Kubernetes. I have been thinking a lot lately about how the Bitcoin network
will evolve and change over time and I believe the future of Bitcoin
applications will live on Kubernetes.

## The Bitcoin Network

For awhile now I have been fascinated by the nature of Bitcoin's network.
Bitcoin forms what is called a "Small World Network" as miners are heavily
invested in maintaining connections to as many miners as possible. The miners
in the network form a massive "supernode" where a user can realistically assume
that if their transaction is propagated to a miner that is part of this
"supernode" then the transaction will be successful and confirmed. This is the
foundation for 0-confirmation security in Bitcoin.

But as Bitcoin grows, not all "nodes" in the network need to be miners. In
fact, the next layer of nodes are heavily incentivized to form dense
connections with mining nodes (and maybe even smart contracts) in order to
maintain zero-confirmation security in the network and providing API interfaces
for clients/users to interact with the highly-connected first layer.

This has been recently discussed with the release of nChain's
[Metanet](http://squiremining.com/category/metanet/) project which intends to
replace the internet with a value-network that is built entirely on the Bitcoin
blockchain. The idea is that "Merchant nodes" are nodes that expose useful APIs
for developers and users that will maintain high connectivity (due to economic
incentive) to the densely connected miner nodes.

For enterprise to adopt Bitcoin, we must adapt to the existing DevOps model
that has enabled enterprise to grow and evolve at the rate that only Open
Source Software can provide. As the DevOps world moves to container
orchestration inside of Kubernetes, it is only natural that Bitcoin evolves
using the same infrastructure. 

## What is Kubernetes?

In layman's terms, Kubernetes is a cloud-platform that allows big business to
innovate and develop their applications faster and in a sustainable and
scalable manner. Kubernetes helps to automate application deployments and
provides tools that give enterprise more flexibility than a traditional
Platform-as-a-Service system.

In even simpler terms for those who only care about Bitcoin, Kubernetes is a
cloud system that power complex applications.

Look at the Kubernetes [Capital One case study](https://kubernetes.io/case-studies/capital-one/):
>Capital One has applications that handle millions of transactions a day. Big-data decisioning—for fraud detection, credit approvals and beyond—is core to the business. To support the teams that build applications with those functions for the bank, the cloud team led by Senior Director Software Engineering John Swift embraced Kubernetes for its provisioning platform. "Kubernetes and its entire ecosystem are very strategic for us," says Swift. "We use Kubernetes as a substrate or an operating system, if you will. There’s a degree of affinity in our product development.

Businesses like Capital One can use Bitcoin to reduce operating costs and
decrease settlement times. The technology is a no-brainer for them to adopt at
scale and they will expect to be able to innovate on top of their existing
infrastructure.

## Bitcoin powered by Kubernetes

Object management inside of Kubernetes is declarative. This means that the user
can simply say "I want a `bitcoind` service" to Kubernetes, and Kubernetes
handles the magic behind the scenes to spin up the needed resources to allow
the user to have a fully functioning Bitcoin node inside of their cluster.

Since interacting with the base layer of the network is cumbersome, I believe
that Bitcoin development will evolve to a point where projects like
[BitDB](https://bitdb.network/) provide an abstraction for developers to work
in environments/languages they are familiar with. Just like how Kubernetes
allows a developer who knows nothing about database configuration to say "Give
me a database instance" and they receive an IP+Port that connects to the
database, we can provide the same level of abstraction for Bitcoin.

Currently, BitDB provides a public API interface that will need to run on a
cloud like Kubernetes in order to scale to meet demand. For cloud-native
Bitcoin applications to thrive, we desperately need a microservice architecture
for Bitcoin.

## Microservice Architecture Needed

There is a project in development called [TeraNode](https://terab.lokad.com/)
which is supposed to reinvent the Bitcoin node software into a microservice
architecture. This is incredibly important moving forward as Metanet will be
powered by Bitcoin nodes that are *not* "full nodes".

I have been a constant fighter against the "full node" disinformation that
exists in Bitcoin. A "full node" in Bitcoin is a miner full-stop. Anyone who is
*not* participating in the Proof-of-Work competition is *NOT* a "full node".
However, it is certainly in a merchant's best-interest to maintain a connection
to the network for monitoring it, yet they have no use for the mining software
that exists inside of the client. The merchants are also at the mercy of miners
as they are the ones solely responsible for producing blocks and maintaining
the security of the network.

As Bitcoin scales, the use-cases and incentives for running a node in the
network will expand. A basic example of this would be providing a token
authentication service that developers can be the consumer of. Currently,
Google provides developers with a basic token service exposed via a REST API.
Google could rewrite the token service to use the underlying security of the
blockchain to provide an even better service and actually monetize the service
itself. For Google to do this, it does not need all of the features the client
currently supports. Ideally Google asks for a specific component of the client
software declaratively and their application can just plug and play.

As a developer, I am not interested in validating every block in the network
everytime I spin up a Bitcoin client... I simply want to be able to query the
network and expose data to my service trusting that the network is controlled
by honest hashrate. While there are some projects that allow a user to do this,
a true SPV client doesn't exist and I would prefer to see all of this software
under a single project source that is tried and tested much like the Linux
Kernel.

We need to follow the Unix philosophy of breaking out the Bitcoin client
software into components that "do one thing and do it well".

## Running Bitcoin inside of Kubernetes
**NOTE:** All of the `bitcoind` deployments referenced below refer to the
Bitcoin SV client.

[Operators](https://coreos.com/operators/) allow a developer to package,
deploy, and manage a Kubernetes application.

I have previously written a [blog
post](https://github.com/dymurray/ao-blogs/blob/master/status-example.md) about
creating a Kubernetes Operator that deploys a containerized `bitcoind` service.
I have a maintained version of what I'm calling the Bitcoin Operator
[here](https://github.com/dymurray/bitcoin-operator).  This operator allows
anyone to deploy an RPC-enabled `bitcoind` service that exposes some
information at the Kubernetes resource level so that other applications running
on Kubernetes can automate infrastructure rollouts depending on the status of
the Bitcoin Operator.

### The Bitcoin Operator

To learn more about running the Bitcoin Operator, see my [Bitcoin Operator
blog](https://dymurray/ao-blogs/blob/master/bitcoin-operator.md). The Bitcoin
Operator allows a developer to easily run Bitcoin inside of Kubernetes.

This attempt was just a proof of concept as I do not believe it is feasible for
application developers to run a "full node" inside of Kubernetes unless they
are running a cluster on mining hardware. The deployment also requires a beefy
cluster that can handle the entire blockchain sync as well as enough CPU cores
to validate the blocks.

# Conclusion

As more and more businesses choose a hybrid-cloud deployment model for their
applications, Bitcoin cannot be an exception. For Bitcoin to scale to meet
enterprise business needs then we must create better software that can be
consumed for big business use cases. The growth and scaling of Bitcoin must
force us to meet this problem head-on with solutions such as TeraNode that
allow big business to plug Bitcoin into their existing infrastructure.
