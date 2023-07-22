### IAP #

# CSP, Concurrency and Distributed Processing Model of Relay Engine

1. **Preamble**
```
1. Title: CSP, Concurrency and Distributed Processing Model of Relay Engine
2. Author: l0k18 <stalker.loki@protonmail.ch>
3. Comments-Summary: none
4. Comments-URI: none
5. Status: proposed
6. Type: Peer Application/Philosophy
7. Created: 2023-06-22
8. License: CC0
9. Replaces:
```

1. **Abstract**

   Modern hardware is increasingly built in a way that leverages parallelism to continue to improve the performance of computer systems, but the bulk of software development is largely still trapped inside the pre-Unix world of Von Neumann Machines with perfect synchrony, that doesn't exist at the limits of the capabilities of silicon. A core principle of Indra's design is to maximise the benefits of parallelism, concurrency and geographical distribution in ways that are not yet widely accepted or understood.

1. **Copyright**

   Creative Commons Zero v1.0 Universal - As far as possible, all rights of copyright are waived and no limitations are made upon the use of the work, derivation and redistribution, any use of it is the liability of the user as the Authors have no beneficial interest in it after the waiver is in force, with no effect upon the existing patents owned by the contributors.

1. **Specification**

   ## State Machines as Concurrent Communicating Networks of Pipelines

   In a quite famous presentation by Rob Pike in 2011, [available here](https://go.dev/talks/2011/lex.slide) he describes a way to write a state machine as a directed graph of atomic FIFO queues and coroutines. Specifically a lexical analyser, but when you look at what it is, it has a lot of similar features as a stream processor, and most especially, of an onion routing relay network.

   Indra adopts this model to devise and to enable a compartmentalised but highly programmable relay command set that we refer to as Onions.

   One of the core principles that goes along with this is the limitation we make in order to most optimally protect privacy, is to use the Source Routing model.

   What this means is that clients are able to acquire, largely anonymously, a snapshot of the state of the network in the form of peer information gossip, and construct layered messages that use encryption to allow the paths of traffic to be obscured from outside the view of the relays actually doing the work.

   ## Message Processing Architecture

   In order to simplify and make extension easier, the structure, encoding, handling and accounting for processed messages are all in one place, making the message layer the first class citizen of Indra's protocol design.

   These message types can be added easily with minimal reference to other types of messages, and provide for the notion of internal versus external processes.

   - Messages that are revealed by the decryption of a crypt layer to a relay are the messages the client intended them to see, and process.

   - Each layer the relay can see is processed in isolation, and subsequent layers of what a relay decodes are forwarded internally and each handler interacts with the engine interface to change the state of the peer's data, adding to pending processing, caching, registering sessions, and so forth.

   The construction of these layered messages is performed by code that accepts parameters that fill the fields of each message, and initially the cookie cutter message forms are defined, but ultimately the specification should be made so that plaintext descriptions and text encoded data for the fields and message payloads can be turned into a custom message.

   By isolating the engine's data handling and accounting systems inside the message types, we make it a lot easier to add new message types, as well as maintain the individual implementations.

   ### Facilitating Application Development

   In this way, we intend that it be as easy as possible for application developers to cover the edge cases for us, that are the meat of their application's workload.

   Indra's base templates for message construction are intended to cover the primary use cases related to relaying, tunnelling out to other systems, hidden service introduction, handshake and the transport system.

   Other ways to use it are possible, and especially the Delay message, which prompts a relay to cache the message until a given time passes, and then continue processing, enables all kinds of interesting and complicated uses for both asynchronous messaging and even anonymous, time locked data storage, a novel solution for the as yet not solved problem of reliable, private offsite storage where even the host does not know anything about the data or the users.

   Using Indra as a replacement for IP routing for remote control and privacy protected group interactions either via the multicast or relay pattern, and the possibilities for how to do this are literally endless and would overwhelm the core development process if they were not shifted to the developers of systems using Indra. 

1. **Motivation**

   A key core principle of a project needs to be understood by all who might get involved in implementing and augmenting it. For Indra, this relates to the constellation of design philosophies found in the literature of Unix and the Go language, and the works of the designers.

   Go is a language that is easy to approach, minimises wasted time in compilation, and recommends a very opinionated syntax and idiom, based on a set of principles that have been learned by long time software developers whose work founded the entire edifice of network computing.

   With these basic tools in hand, the capabilities that programmers can implement go beyond old ideas of what computer software does and enables a multiplicity of systems to cooperate in the same way as it does on the micro scale inside a computer device.

   And lastly, to also reduce the amount of edge cases handled by the system core, so that the core is simple, solid and easy to maintain, and easy to debug, while other cases are funded and developed by those who have a business model that needs custom message structures to implement.

1. **Rationale**

   Like all the best software projects, as the founders, we exercise our right to assert that we constrain the work of the core project to one programming language, and the programming language in this case is Go.

   In some cases, FFI importing of other language systems via CGO is ok in an early reference implementation, but if the foreign code touches on performance of the system too much, the lack of profiling and benchmarking tools is the main reason.

   Performance is critical in a low latency system, and this is also part of the reason why Go was chosen, in addition to the fact that both founders also choose it as their preferred language for development.

   At the time of writing, the large majority of cryptographic and coding systems needed for Indra are available in AVX2 or other similarly optimised Assembler kernels, and performance is expected to be the typical slightly under C++/Rust code due to the sometimes awkward and time consuming automated garbage collection, and above it, when the load passes 50% and the benefits of the sub microsecond switching of goroutines shines.

   Understanding the notion of a state machine running on compartmentalised, largely isolated systems connected over networks, is key to understanding the design decisions and rationale of Indra as a whole, and why these things are not negotiable requirements.

   A last note to add, is that as relates to this matter of parallelism and concurrency, that it is a known issue of a coroutine using system like Go does not optimally parallelise workloads. For these kinds of cases, we are recommending the use of stdio-based IPC APIs to deploy single threaded workers and leverage kernel level parallel processing.

1. **Backwards Compatibility**

   N/A

1. **Reference Implementation**

   Currently found at [github.com/indra-labs/indra/pkg/engine](https://github.com/indra-labs/indra/pkg/engine)

1. **Comments**

   



