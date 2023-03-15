# Secure Elections Technology

Texas needs a secure elections system.

Perhaps the simplest way for Texas to get a secure elections system to
get it is to commission it from the Texas' University of Texas public
higher education institutions.  Who would object to giving UT schools
small grants to develop secure elections systems in competition with
each other?  Surely no academic would risk their reputation in building
an insecure elections system.  If a winning system is produced, why
wouldn't the State of Texas adopt it?

Still, we should consider the issues and requirements for such a system,
and even some ideas for how to build a secure elections system.  That's
what I'll do in this document:

 - describe what a voter's experience should be with secure elections
   systems
 - discuss weaknesses of elections systems,
 - discuss what can and can't be done with technology,
 - discuss requirements for secure elections systems,
 - explain what blockchains are and how not to misuse them,
 - describe how to mix paper and digital cryptographic technology,
 - describe such a secure elections system,
 - present a threat model for secure elections systems,
 - analyze the proposed secure elections system.

It is essential to understand that technology cannot fully secure
elections.  At the end of the day we must have judicial and political
mechanisms for punishing cheating.  But even weak-willed courts and
legislatures today would be more likely to act if the evidence of
cheating was *automatic* and obvious to all.

To make cheating so obvious, and so automatically obvious, we need
technological solutions that are sound, reviewed, and accredited or
otherwise accepted by the established judicial and political bodies.  It
is absolutely essential that any such solutions be *sound* about all
else.

> NOTE: This file is written in a textual format known as Markdown.  If
> you're reading it as a GitHub page, then it will be rendered
> correctly.  If you're viewing it as a raw text file then you may need
> to understand a bit about Markdown, though it is fairly self-evident.
>       
> Readers of this file as a raw text file should know that text between
> backticks is meant to be rendered as code (specifically in a
> fixed-width font).  For example: `a + b`.
> 
> Section titles start with a `#`, `##`, or `###`, where `#` is a
> top-level section, `##` is a sub-section, and `###` is a
> sub-sub-section.  The rest shold be obvious.
>
> Asterisks are used for bold emphasis, while underscores are used for
> italics emphasis.

# Ideal Voter Experience

The process of voting in a secure elections system should be similar to
today's, with only additions that help the voter and the public confirm
the validity of an election's results.

Specifically:

 - voters should sign-in on a tamper-evident paper voter roll as well as
   on a tamper-evident digital cryptographic roll;

 - voters should take a picture of the page they signed on the voter
   roll;

 - voters should get a receipt from the digital voter roll that captures
   the "head" of a voter roll blockchain that can be verified is
   included in the final blockchain head;

 - voters should use a ballot marking machine and review the markings on
   their ballot match their selections

   (this step eliminates Florida, 2000, "hanging chad" style problems)

 - voters should feed their ballots to a ballot scanner that:

    - will drop the ballot in a physically-secured, tamper-evident
      ballot box

    - will print a receipt that *does not* violate the secrecy of the
      ballot but which _does_ commit to running elections results
      without revealing them and in a way that can be cryptographically
      verified with any number of apps

 - voters should be given the link to a document that describes how to
   validate the voter roll receipt and the ballot scanner receipt, both
   while still in the precinct, as well at home after the precinct's
   closing

 - voters should be able to visit a poll watcher to get help validating
   their receipts

 - when the election closes, voters should be able to:

    - validate the integrity of their precinct's voter roll,
    - search through all the voter rolls in their precinct/county/state
      for instances of others voting in their name
    - validate the integrity of the ballot scanning
    - rely on assurances by poll watchers that they have observed manual
      counts of randomly-selected ballot boxes match the cryptographic
      commitments made by the ballot scanners

In particular, voters should be able to quickly detect ballot box
stuffing, ballot scanner compromises, voter roll padding, and incorrect
aggregation of precinct results.

Denial of service problems ("machines are all down") can be resolved
with something akin to provisional voting in other precincts or even
counties.

# Weaknesses of Elections Systems

Let us list the different stages of holding an election, which are:

 - voter registration
 - voting
 - counting
 - aggregation (adding up all the totals from all the participating
   precincts)
 - certification (the very last step in aggregation)
 - legal challenges

Most election integrity issues historically arise at the counting stage,
though they can also happen during voting and during aggregation.

There are or can be issues in voter registration processes as well.  A
registrar could issue voter IDs to fictitious persons, to dead persons
(or keep now-dead, previously-registered persons on the rolls), underage
persons, and other ineligible persons.

In the case of aggregation we could see county elections judges or state
secretaries of state report incorrect results even if all the precincts
reported correct results.

What we can do with technology, and what we can't:

 - We can bring technology to bear in making voting, counting, and
   aggregation *tamper-resistant* and *tamper-evident*.
 - We cannot use technology to prevent the whitewashing of cheating, but
   by making the cheating automatically obvious and undeniable we can
   make it harder for cheaters to get away with it.
 - Technology can't easily solve registration issues directly.

Ideally we can make detection of election fraud during voting, counting,
and aggregation, fully automatic.  That is what the focus of this
document will be.

# Goals

My main goal here is to:

 - make any cheating so obvious, and so *automatically obvious* that
   it is politically infeasible to allow it.

Implied by this goal are the following corollary goals

 - elections must be tamper-evident (and preferably tamper-resistant),
 - elections must be auditable,
 - all the technology involved must be reviewable and understandable so
   that people can believe it is secure.

In this document, too, I will

 - build a threat model for every piece of the system proposed,

 - then analyze the security of this architecture given that threat
   model.

A threat model is a set of attacks on a system that one can foresee and
attempt to defeat.

# Requirements for Election Integrity Technology

 - Tamper-evidence.  Ideally also tamper-resistance.

 - Paper and digital cryptography.

    - Ballots must be paper ballots, and all markings on them must be
      human _and_ machine readable.

    - There must be a paper voter sign-in roll.

    - There should be a digital and cryptographically secure system as
      well as two parallel systems will be harder to compromise than
      one.  As well, we are addicted to using technlogy to lower the
      costs of running elections, and that is unlikely to change, so we
      might as well make sure that that technology is secure.

 - Ballot marking and ballot counting have to be separate machines.
   This is necessary so that voters can see that their paper ballot is
   accurate.  Ballot marking machines are useful in avoiding Florida
   2000 "hanging chad" style failures.

 - Voter sign-in rolls.

 - It must be possible to audit elections immediately upon close of the
   precinct on the last day of the election.

 - It must be possible to secure the vote totals aggregation step.

 - Ballot secrecy must be maintained.  That is: it must not be possible
   to determine how any one voter voted.

    - It must be possible to publish ballot images without compromising
      vote secrecy.

    - Voters must not get to take home any "receipt" that shows how they
      voted.

 - Every effort has to be made to keep any _software_ honest.  Software
   will generally have bugs, and the larger the software, the more bugs
   it will have -- this is a fact of life that we need to deal with.

   (Spoiler: there are ways to do this.)

 - Digital systems should have public designs for their cryptographic
   protocols so that

   a) the public can validate the designs,

   b) the public can use apps to validate public election data,

   c) anyone can write such apps.

# Technological Solutions for Voter Roll Tamper Evidence

The easiest thing to make tamper-evident is the voter roll at each
precinct.  It will be easy because the voter roll is not secret (as
compared to the actual votes, which are).

"Voter roll" here refers to the record of who voted when and where in
any one election.

We need both, a pen-and-paper tamper-evident voter roll *and* a digital
tamper-evident voter roll.

With a tamper-evident voter roll the only way for a loser to cheat is to
alter the counts either in each precinct or as aggregated.  Because most
election fraud happens by stuffing the ballot boxes *after precincts
close*, preventing the padding of the voter roll means that stuffing the
ballot boxes must yield a larger number of votes than there were actual
voters (not registered, but actual).

A tamper-evident voter roll, however, cannot prevent fraud in counting
or reporting and aggregation.  A tamper-evident voter roll can only be
one of several technological measures to prevent election fraud.

# Explain It Like I'm 10: Tamper-Resistance/Evidence in Paper and Digital Systems

Tamper evident systems make any attempt at tampering obvious to anyone
examining the system.

Tamper resistant systems are meant to be difficult to tamper with at
all.

Tamper resistance is harder and more expensive to get than tamper-
evidence.  Tamper-evidence is generally sufficient, but when tampering
is evident one can have a hard time recovering -- it may not be possible
to hold a new election, for example.  So tamper-resistance is highly
desirable even if it is harder and more expensive to design than
tamper-evidence.

For example, many books are tamper-evident in that if one rips out a
page, that will be noticeable, even obvious.  Tear-resistant paper can
make a book tamper-resistant as well as tamper-evident.

What we want in voter sign-in paper rolls is the following:

 - ripping out pages should be difficult, and attempts (successful or
   otherwise) should be obvious;

 - adding pages should be difficult (because the binding should resist
   cutting) and obvious;

 - erasing or altering names, signatures, voter IDs, timestamps, etc.
   should be difficult and obvious;

 - there should be timestamps, and the timestamps should only get later
   as one moves forward through the book -- clocks should not run
   backwards or be adjusted backwards (even if they're running fast);

 - there should be tamper-resistant watermarks of serial numbers in the
   book such that replacing the whole book would require the cooperation
   of the manufacturer because the book serial numbers are recorded in
   some other book of books that is itself tamper-resistant;

 - there should be precinct open and precinct close entries.

A 'blockchain' is the digital and cryptographic analog of a tamper-
resistant and tamper-evident book of chronological entries.

## Replacement Attacks on Voter Sign-in Rolls

One attack against a tamper-resistant systems is wholesale replacement
of a unit.  For example, imagine a precinct closes and a bad actor
replaces the entire voter sign-in rolls with ones that show many more
voters than actually voted: now the bad actor can also stuff the ballot
boxes and have the ballot counts match the sign-in rolls.

To defeat this several things can be done:

 - There can be a book of sign-in rolls where every sign-in roll book's
   serial number and time of opening is recorded, and that parent book
   is itself tamper-resistant and tamper-evident.

   One problem then is that that book can itself be replaced.

 - Snapshots of the books can be recorded by the public and observers
   and even published online.  Especially the precinct close entries
   must be snapshot, observed, and published.

   This allows poll watchers and authorities to immediately audit the
   legitimacy of all voter sign-in rolls.

   For paper books a "snapshot" would be a photographic picture.

The critical take-away here is that real-time observation and recording
of the system by third parties (the voting public and poll watchers) is
essential to keeping the system secure.  Tamper resistance by itself is
not sufficient!

## Explain it Like I'm 10: Blockchains

A blockchain is nothing more than the digital cryptography equivalent of
a tamper-resistant paper book!

The way a blockchain is constructed is that it is a sequential log of
entries where each entry "cryptographically binds" the previous entry.

Let's talk about that "cryptographic binding".

This binding is based on something called a "hash function", which is a
mathematical function that takes an arbitrarily large bit of data and
outputs a small "hash" of that data -"small" here means, for example, 48
bytes-, and this function has certain characteristics:

 - it is hard to guess/compute the original data from the hash

   (this is known as 'first pre-image resistance')

 - it is hard to guess/compute a different data that gets the same hash
   value

   (this is known as 'second pre-image resistance')

 - it is hard to guess/compute a pair of data that have the same hash
   value

   (this is known as 'collision resistance')

In cryptography we write `H(x)` to refer to the hash value that results
from applying some hash function `H()` to some data `x`.

Another common bit of notation in cryptography is `||` as the data
concatenation operation.

And one more common bit of notation is `{ list, of, items}` to denote
structured data.

This trivial notation makes it easy to understand simple blockchain
constructions.

Here's one trivial blockchain construction that is actually used in the
real world:

 - the first entry is some constant data (for example: 48 zero-valued
   bytes); let's call this `e_0` (the first entry)

 - the second entry will be `e_1 = H(e_0 || data_1)`

 - the third entry will be `e_2 = H(e_1 || data_2)`

 - and so on, with the n-th entry being `e_N = H(e_N-1 || data_N)`

The idea here is that the data items `data_i` have to be logged
separately, but any modification to any of that data after it is
incorporated into the blockchain will be detectable because of the
second pre-image resistance and collision resistance properties of
cryptographic hash functions.

If the blockchain is recorded by many as it is produced, then it becomes
infeasible to make changes after the fact.  Note though that if a
blockchain is used but not actually observed or observable, then it
provides no protection -- it is essential that the blockchain be
observed by the public!

> NOTE: Cryptographic currencies are generally based on blockchains, and
> they depend on the public seeing the blockchain and validating it.
>
> Blockchains have many uses beyond cryptographic currencies.

> NOTE: The author does not support crypto-currencies ("crypto") as a
> matter of politics, but makes no recommendation on their use or
> non-use.

### Cryptographic Commitment Protocols

As noted above, the actual `data_i` items will be logged separately.

If the data can be published immediately, then they should be, otherwise
if the data must not be revealed until a later time, then the data can
be published at that time.

Delaying publication of the data yields something called a
'cryptographic commitment protocol', as the blockchain _commits_ to
values without revealing them at the time of the commitment.

Cryptographic commitment is useful for data that must be kept secret for
some time.

For example, vote scanners could maintain a blockchain of running vote
totals, and publish that data at the end of the election!

Now, to avoid compromising the secrecy of the ballot we can't have
running totals logged after each vote.  Otherwise one could correlate
voter sign-in roll entries with vote scanner entries and find out how
everyone voted.  In precincts with multiple sign-in stations and
multiple scanners there is some natural randomization to provide some
ballot secrecy, but we can do better by having the vote scanners only
log vote totals every N votes -- for example, every 5 or maybe every 50
votes.

Thus we scanner blockchain-secured data can have this form:

```
    scanner_data_i =
        if (i mod N == 0) then
            current_totals
        else
            scanner_data_i-1
        end
```

where `current_totals` is:

```
    current_totals =
        race_0_totals || race_1_totals || .. || race_N_totals
```

## Explain it Like I'm 10: Blockchain Security

If `H()` is a cryptographically-secure hash function then the above
blockchain design is tamper-evident to anyone who examines a blockchain,
as that person can evaluate the blockchain function at each step in
order to validate that the hash values they see match the ones recorded
in the blockchain.  The same things that make a blockchain tamper-
evident also make it tamper-resistant.

Imagine that each `data_N` is a voter sign-in roll entry bearing a
voter's name, voter ID, signature, timestamp, and the name and signature
of the poll worker who signed them in:

  `voter_roll_data_i = { voter_metadata, poll_worker_metadata, timestamp }`

Let's say now that we want to alter one of these that is not the _last_
entry in the blockchain: but this would cause the hash values that an
auditor would compute to differ from the ones computed at the time that
the subsequent voters were signed-in, and this would be _obvious_.

As with tamper-evident paper books there is a replacement attack on the
whole blockchain, and that attack is mitigated in the same way: by
having third parties (voters, poll watchers) take and publish snapshots
of the blockchain as voters are signed-in.

Note that both, tamper-evident paper sign-in books and blockchains can
be "broadcast" on the Internet as a way to increase the number of third
parties who observe and record the entries in the books so that they can
validate the final books at precinct close times.  A paper book can be
broadcast by having pictures or video of it served live on the Internet.

So as you can see, a blockchain is very similar to a tamper-evident
paper notebook.

Hopefully this demystifies blockchains for the reader.

But now let's discuss what one might call a "gaslighting" attack on
blockchains.

Imagine half the observers claim they saw a different blockchain than
the other half, now what?

Cryptography has an answer.  The machines constructing the blockchain
can _digitally sign_ the entries using a cryptographically secure
digital signature algorithm and a private key that only the machines
know (more about this later!), and then observers can validate the
signatures using what's know as the 'public key' that corresponds to the
machine's private key.

Now the gaslighting attack fails because everyone would be using
publically available software to validate the blockchain including the
signatures, and everyone can check that software by constructing test
blockchains.

So now the voter roll entry data look like this:

```
    voter_roll_data_i =
       Signed(machine_private_key,
              { voter_metadata, poll_worker_metadata, timestamp })
```

where `Signed(key, data)` looks like this:

```
    Signed(key, data) = Sign(key, data) || data
```

A digital signature system with this asymmetry between 'private key' and
'public key' is known as a 'public key cryptosystem'.  In such a
cryptosystem we have:

 - a "public key" for every "private key",
 - the public key can be shared with the world,
 - anyone knowing a given public key can validate a signature made with
   the corresponding private key,
 - but no one can compute the private key from the public key
 - and no one can forge signatures

A detailed explanation of public key (PK) cryptosystems is out of scope
for this document, but suffice it to say that public key cryptosystems
are at the core of all security on the Internet.  When one visits an
`https:` site there is a chain of public key operations involved that is
used to validate that one is talking to the correct server.

## A Word About 'Key Management'

To make the use of any cryptosystem be secure one must get 'key
management' right.

Key management is about answering questions like
"how do I know this public key is the correct public key for the entity
that claims to have made this signature?".

It is critical to get key management right, as even the most secure
cryptosystems can be rendered insecure if the key management used is
itself insecure.

The details of key management are also out of scope for an ELI10
discussion.  Suffice to say that machine manufacturers and elections
authorities need to be involved in ensuring that every elections device
(such as voter sign-in stations and ballot scanners) has a public key
that is recorded and published by those authorities ahead of an
election.  And the whole system's design needs to be public so that the
_public_ can validate it.

It is also important that software running on those machines cannot leak
the _private keys_, and we'll discuss how to do this (even when the
software is maliciously compromised!) in more detail later.

# Dealing with Various Attacks

Note that I've already covered above some the attacks described in this
section.

## Replacement Attacks

It is critical to understand that while a voter roll can be tamper-
evident, the device itself -paper or digital- alone is insufficient to
prevent padding of the voter roll because the voter roll could always be
replaced wholesale after the precinct closes!

To prevent replacement attacks we can provide several things that will make
any replacement glaringly obvious:

 - voter roll book serial numbers (for pen-and-paper rolls),
    - digital signatures made by 'hardware keys' for blockchains
 - signatures by the relevant election judge and the state secretary of
   state,
 - a receipt for every voter of their having voted that strongly binds
   the state of the voter roll at the time they voted,
 - broadcast of the voter roll in real time or near real time.

For a digital voter roll we can have receipts for voters that contain QR
codes with digital, cryptographic proof of the inclusion of their voter
entry in the voter roll, including the proof for the preceding voter (or
precinct open record in the case of the first voter, or commissioning
record for the book in the case of the precinct open record), and
digital signatures of the entries made by the machines using keys kept
in in hardware and only indirectly accessible to [approved] software.

## Failure-to-Close-the-Roll Attacks

Another attack on tamper-evident voter rolls would be to not write a
precinct close entry in the roll at precinct close time.

Two things help here:

 - poll watchers
 - voter roll record timestamps

As well, voters could make a note of how many voters are left in line
when the precinct is closed, thus leaving evidence that is hard to
erase of impending precinct close.  If the precinct is not closed soon
after closing the doors and instead the voter roll gets padded, then
that will be quite obvious.

## Pen-and-Paper Tamper Evident Voter Rolls

A pen-and-paper voter roll is just a book that voters are asked to sign
when they show up to vote.  To make a pen-and-paper voter roll tamper-
evident we just need books with:

 - tear-resistant pages that leave a visible mark if a page is ripped
   out

 - non-erasable ink pens

 - places in which to write, in chronological order:

    - book commission record (signed by the state secretary of state and
      the election judge)
    - precinct open time
    - voter metadata:
       - voter name
       - voter ID
       - a timestamp (every entry must have a timestamp in the future of
         the preceding entry!)
       - any notes
    - precinct close time

## Digital Tamper Evident Voter Rolls

To make a digital tamper-evident voter roll all we need is what is a
blockchain with entries signed using a public key cryptosystem with
private keys held in hardware (and only indirectly accessible to
approved software).

An earlier section explains what a blockchain is.

# Tamper Evident Vote Counting

While the voter roll is *public*, _how_ each voter votes is meant to be
*secret*.

The secrecy of the ballot is essential to free elections, for otherwise
people can be browbeat into voting for specific candidates and ballot
measures, or they can be punished for voting for the "wrong" candidates
and ballot measures.  The secrecy of the ballot also protects against
vote buying: you can pay someone to vote in a particular way, but if
there is no way for them to prove that they did then the vote buyer does
not know if they are getting their money's worth.

The secrecy of the ballot is also a hinderance, for the most obvious
step we could take to make vote counts secure is to apply the same
techniques to voting as to the voter rolls, but to do that would make
voting non-secret.

Yet we must have voter secrecy.

Thus while we can be inspired by approaches to securing the voter roll,
we must seek somewhat different solutions for actual voting and vote
counting, as we must maintain voter secrecy.

## Problems in Securing Voting and Vote Counting

What can we do about vote secrecy to make vote counts secure?

For one, we can do the things we've long done:

 - paper ballots that can be hand-counted
 - locked, tamper-evident ballot boxes

But in this day and age hand counting is... mostly not done ("it's too
expensive") and it's not secure either.  We've all seen video of hand
counting where fraud clearly took place.  Note that such video evidence
is deniable by the legal and political processes we have.

Many people are wary of voting machines, but machines are probably here
to stay.  We've seen that older technologies too were easy to subvert
even though they weren't computer based.

We may have to make sure that we can build machines that we can trust.
There is a great deal of literature in that space, though building
secure and trust-worthy systems is not a solved problem in Computer
Science.  In spite of this not being a solved problem, we can do a great
deal better than the current, pitiful "state of the art" in elections
machines!

Fortunately there is some good news on the front of how to build secure
elections computers.

## Securing Vote Counting

The simplest thing to do is to have ballot scanners use cryptographic
commitment and blockchains to securely log running vote totals in such a
way that:

a) the public can validate the integrity of the blockchain at any time

b) the running vote totals are only published at the end of the election
and the public can then validate that the vote totals match the
previously-committed values,

c) only the running vote totals of every N ballots are logged so as to
provide acceptable ballot secrecy (e.g., N could be 50).

To make sure that the ballot machines aren't lying about the vote
totals, some randomly-selected ballot boxes must be opened and counted
in public (in the presence of poll watchers and elections judges), and
the resulting counts must be checked against the scanner's log.  If
there are discrepancies then all ballot boxes will have to be
hand-counted, and the hand count correction to the scanner's results
will have to be published and reported.

# Securing Vote Aggregation

Given secure ballot scanner vote totals blockchains it is
straightforward to add up the totals of all vote scanners in an
election, and the public can validate these sums themselves.

Ballot scanner totals can be sanity checked against voter sign-in rolls:

 - there must not be more votes in any race in a ballot scanner's output
   than there were voters at the same precinct

 - the voter sign-in rolls must be properly formed, with precinct open
   and close entries, ever-increasing timestamps, timestamps that are
   within the date(s) of the election, etc.

It is important too that the public be able to validate that every
ballot scanner's public signing keys are indeed the public signing keys
of ballot scanners authorized by elections officials.  It is also
important that the public be able to see and validate manufacturer
certificates of related public keys for the machines' Trusted Platform
Modules (TPM) (about which more below).

# "Trust" in Computing

In Computer Science and the computer industry "trusted" is a term of
art.  It refers to one's ability to "trust" hardware, firmware, and
software components of a larger system, and the entirety of the system
itself, to... be the system that one wanted it to be.  For example, a
trusted computing platform will only run trusted operating systems and
applications.  Compare to "trusting that the system is implemented
correctly and bug-free", which is *not* a definition of "trusted" in
this context.

Though it is generally not possible to build computer systems we can
trust are correctly implemented and free of bugs (including security
vulnerabilities), we have some tools we can use in the construction of
secure elections systems.  Specifically, we have an off-the-shelf tool
that lets build a system whose function can be verified even if the
system itself cannot be.  That tool is called a Trusted Platform Module
(TPM).

A TPM is a *chip*[\*] that contains a very small computer, running a very
small and general-purpose cryptographic system that has many uses
including helping to ensure that larger operating systems and
applications running on a computer are "trusted".  But because TPMs
implement blockchains and other cryptographic features that we'll be
using in the design of an election system, we can tolerate bugs in the
rest of the election system as long as the TPM itself isn't compromised.

> [\*]  The Trusted Computing Group's (TCG) Trusted Platform Module
> standards define interfaces and behaviors.  TPMs can be emulated in
> software and firmware, and need not be discrete chips.  However for
> our purposes TPMs are chips.

Because TPMs are so small (in terms of code size), and because they are
tested and certified by specialty laboratories, if we can build all
critical elections systems functionality largely using TPMs, then we can
reduce the scope of what attackers can do by compromising the rest of
the elections system.

TPMs are also commoditized, which means that they are cheap and easy to
source.  This can also help with supply chain security because it should
be possible to obtain TPM chips from many sources, and we can source
newer ones and older ones, all of which makes it more difficult to
compromise all the TPMs used in a secure elections system.  Moreover, we
can construct a system where it is easy to verify that precinct voter
sign-in rolls and vote counting are done correctly even with compromised
TPMs.

Essentially the idea is to verify:

 - the authenticity of elections machines and their TPM chips (at any
   time, before, during, and after the election),

 - the integrity of signed blockchains built and maintained by TPM-using
   elections machines as the election progresses.

The best entities for doing this are the voter themselves as well as
poll watchers, using applications that they can run on any computer
(including smartphones) to validate a receipt printed by the election
system.  When a precinct closes the election judge and observers should
also -together and independently- check the entire blockchain, checking
that each time they have checked it earier is still included in the
current chain, while the public can follow along.

The only tasks that must be performed with physical access to physical
ballot boxes and ballots are:

 - sampling to verify that randomly sampled ballots in randomly sampled
   ballot boxes are incorporated in the block chain

 - audits

 - recounts

# Using Trusted Platform Modules (TPMs) to Protect Against Buggy and Even Malicious Election Software

TPMs provide a number of "shielded" cryptographic functions:

 - head-of-blockchain registers,

 - shielded cryptographic key objects (secret/private keys),

 - a rich authorization policy facility for determining whether software
   running on the machin with the TPM can use a given key object,

 - use of key objects by software on the machine with the TPM (e.g., to
   make digital signatures).

"Shielded" means that the cryptographic objects (such as secret/private
keys) are held in the TPM chip in a way that either they cannot ever be
read out of the chip, or they can be read only by authorized personnel.
Note that it is always possible to create keys that can never be read
out, and it is also possible to prove this property of a key!

We call keys that can only be read by the TPM -and never by any hardware
or software outside the TPM- 'hardware keys'.

The authorization policy facility of the TPM can be used to construct
arbitrarily complex policies.  For example, we can say that only a
certain version of each bit of firmware and software running on the
machine can use a given key object.  We can require that some external
party (e.g., a State Secretary of State, an elections judge, etc.)
authorize certain uses of keys.  We can make it so keys can no longer be
used past a given time (e.g., at the end of the election) via a key
revocation facility.  We can require the use of a smartcard device by
poll workers to start an election or to end an election.  And much much
more.  If you can dream up a policy chances are that it can be
implemented using a TPM.

TPMs naturally implement blockchains using something calle Platform
Configuration Registers (PCRs).  PCRs are misnamed because originally
they were intended to be a blockchain of firmware and software loaded by
a machine so that that blockchain's state can be used to authorize
access to hardware keys by approved software only.  Thus the set of
loaded firmware and software would be the "configuration of the
platform" and thus the name of the feature.  However, PCRs really are
just blockchains, and they can be used for any purpose.

To be more specific, a PCR records the current-most entry of a
blockchain.

There is a great deal more to say about TPMs, but it's mostly all out of
scope for this document at this time.

# Design Recap

[To be expanded on later.]

 - Signed blockchains for voter sign-in rolls.
 - Committed and signed blockchains of running vote totals for ballot
   scanners.
 - All on machines that use TPMs to secure digital signature keys and
   validate that only approved software is running on the machine.
 - The integrity of the blockchains can be checked in real-time as well
   as after the end of the election, and final (and running, as they
   ran) vote totals are revealed only at the end of the election.
 - Running vote totals revealed at the end are recorded and reported
   with granularity that preserves ballot secrecy.
 - Appropriate key management so that machines' and TPMs'
   manufacturer-signed certificates can be validated by the public, and
   enrollment of machines by elections officials can also be validated
   by the public.

# Operational / Administrative Matters

[TBD.  This section will discuss everything from how machines are
commissioned, decommissioned, have ballots loaded, have counters read a
precinct close time, etc.  Of particular interest will be recovery from
reboots, power failures, and any attempts at denial of service attacks
on the machines (e.g., attempts to destroy them).]

# Thread Models

[TBD.  This section will describe the kinds of malicious actors and
attacks that we wish to defend against.]

# Analysis

[TBD.  This section will analyze whether and to what extent the systems
proposed here satisfy the thread model described in the previous
section.]
