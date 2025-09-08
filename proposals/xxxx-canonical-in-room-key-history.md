# MSC0000: Canonical in-room key history

## Proposal

In this proposal we refer to the resident server of a room creator to
be a _room creator server_.

### The `m.server_key` event

Room creator servers maintain a history of the room's keys.

### Room creation

When a room is created, the same key that is used to sign the create
event is also used to publish a key. This is the root key for the room.

When additional creators join a room, the room creator server which is
the sole creator of the room publishes the key used in the additional
creator server's join event to its own chain.

This event becomes the root of the additional creator server's own key
chain. Which it must maintain if it wants to create

Authorization of join events becomes dependent on referencing the an
authorised `m.server_key` event that references the singing key of the
join event. This is to ensure that the key history is maintained by
room creator servers.

### Maintaining the history

Each room creator server maintains a linear chain of server keys.

Each room creator server must keep a maximum of one forward extremity
for the set of `m.server_key` events they maintain.

### Forcing room creator servers to maintain their keychain

In order to force room creator servers to maintain their keychain,
they must be forced to acknowledge the forward extremities of their



### The role of notaries

They just need to provide the forward extermity that they know about
when queried on a specific room.



### I don't think there's a way to force additional creators to maintain a consistent history

This presents a problem because it means that an additional creator can almost always trick
a new joiner into thinking there is another history to the room.

We might get away with it if we just accept that people shouldn't join the room if a notary
disagrees with the additional creator.




## Potential issues

### Enforcing keychain maintainership is network level not DAG level

Could be .. hmm actuall no i know how to fix this, just make sure
that the only event that they can append to the DAG, is one that references
the forward extremity.

Hm but what if i join with a server that will only allow one room
creator to verify? and no one else? will that softlock that creator server?
Yes until it retracts the key.. not good.

So we will have to allow additional creators to say whether they have verified
each key... but we need to make sure that doing so doesn't allow them
to just selectively never verify things and lie later...

Instead, can we split by

So we need another proposal that ties identity in the same way.

### Tombstone escape hatch

### Participaiting in a room with a new server becomes dependent on successfully handshaking with a room creator



## Alternatives

*This is where alternative solutions could be listed. There's almost always another way to do things
and this section gives you the opportunity to highlight why those ways are not as desirable. The
argument made in this example is that all of the text provided by the template could be integrated
into the proposals introduction, although with some risk of losing clarity.*

Instead of adding a template to the repository, the assistance it provides could be integrated into
the proposal process itself. There is an argument to be had that the proposal process should be as
descriptive as possible, although having even more detail in the proposals introduction could lead to
some confusion or lack of understanding. Not to mention if the document is too large then potential
authors could be scared off as the process suddenly looks a lot more complicated than it is. For those
reasons, this proposal does not consider integrating the template in the proposals introduction a good
idea.


## Security considerations

**All proposals must now have this section, even if it is to say there are no security issues.**

*Think about how to attack your proposal, using lists from sources like
[OWASP Top Ten](https://owasp.org/www-project-top-ten/) for inspiration.*

*Some proposals may have some security aspect to them that was addressed in the proposed solution. This
section is a great place to outline some of the security-sensitive components of your proposal, such as
why a particular approach was (or wasn't) taken. The example here is a bit of a stretch and unlikely to
actually be worthwhile of including in a proposal, but it is generally a good idea to list these kinds
of concerns where possible.*

MSCs can drastically affect the protocol. The authors of MSCs may not have a security background. If they
do not consider vulnerabilities with their design, we rely on reviewers to consider vulnerabilities. This
is easy to forget, so having a mandatory 'Security Considerations' section serves to nudge reviewers
into thinking like an attacker.

## Unstable prefix

*If a proposal is implemented before it is included in the spec, then implementers must ensure that the
implementation is compatible with the final version that lands in the spec. This generally means that
experimental implementations should use `/unstable` endpoints, and use vendor prefixes where necessary.
For more information, see [MSC2324](https://github.com/matrix-org/matrix-doc/pull/2324). This section
should be used to document things such as what endpoints and names are being used while the feature is
in development, the name of the unstable feature flag to use to detect support for the feature, or what
migration steps are needed to switch to newer versions of the proposal.*

## Dependencies

This MSC builds on MSCxxxx, MSCyyyy and MSCzzzz (which at the time of writing have not yet been accepted
into the spec).
