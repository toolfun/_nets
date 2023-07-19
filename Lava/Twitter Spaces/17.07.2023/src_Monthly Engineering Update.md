0:00
80s You. 2s Hello? 12s Hey, what's up? 1s Hey, can you hear me? Hi. Yeah, loud and clear. Good. Can 
you hear me, guys? Yes, sir. As I'm a listener, that's why. 1s Oh, it's updated now. 3s Can 8s how's 
everyone 1s doing? Well, doing excellent. How are you? Doing pretty good. Flying out to Paris tonight. I 
was going to say, you got a big flight today, right? Yeah, I'm flying tonight. 2s What airline are you 
flying? 2s Air France. 2s Okay, nice. 2s I just imagine that there's, like, stereotypical French food served. 
4s Let's hope it's not, like, United food. 5s No. 2s I hear an echo. Can you hear me? It yes. There's an 
echo of me coming through your 10s I was going to say some of the best food I had was on the Indian 
airlines. I think I'm biased, though, because they had, like, lots of vegetarian stuff. Indian food is the best. 
When I lived in Singapore, I would abuse it. 2s It's amazing. I would eat it 1s and cry. 1s Because it was 
both too hot and too yummy. 2s In Singapore, they have amazing food. It's, like, insane. They have hoker 
markets. You can buy anything for $5. It's incredible. 1s I really hope to fly, like, Ethiopian Airlines one 
day, because I think that just, like, pound for pound, they may have the greatest food if you come to 
Israel. There's a lot of Ethiopian food in Israel. Oh, yeah. 1s There's a few restaurants, yeah. Have to 
check that out. 7s So I will 1s play a little music for a second here while we wait. Please let me know if 
the volume levels are appropriate. 51s All right. 2s Just want to give a shout out to some of the people 
who we see have joined the call, some of our community members, and Lavaites, some of the various 
developer champions. So we appreciate you and say welcome to the rest of our community for joining 
the engineering call number nine. This engineering call is hosted on Twitter Spaces as you see, which has 
been a change of pace for us. We ordinarily were hosting our engineering updates on Discord, so we're 
glad for those who have made the transition with us and for the newcomers, we say welcome to you. If 
you're interested in speaking, there is a quirk of Twitter Spaces in that you have to join from your phone. 
You won't be able to become a speaker from your computer. I don't know exactly why that is the case. 
I'm just aware that that is one of the quirks of Twitter Spaces. So if there's a question or something that 
you'd like to interject during this call, please be aware that you need to do so from your phone. 3s Just 
want to say welcome to everybody 1s who has recently joined and say that this engineering call is an 
opportunity to provide sneak peek into our development process for Lava. Lava is the open standard for 
Blockchain RPC and APIs, and this is a crucial way for us to interface with the community. We're 
encouraging our communities to ask questions, to provide feedback, to contribute, and this gives you first 
row access to our awesome engineering team. We have some superstars on the call today, as always, so I 
want to welcome them. First, I want to say that we're joined by Gil Binder, our chief technology officer. 
Gil, welcome to the call. 1s Hey, thanks, man. Great to be here as always. Happy to provide updates on 
what's happening on our engineering. 1s Absolutely. As well, we're joined by Omar Michelle, who's our 
chief architect officer. Hello, guys. Happy to be here. If you have any questions, feel free. 3s Great. And 
we're also joined by Jacob AVADAR, who is additionally our senior VP engineering. Welcome, Jacob. 
Welcome. Great to be here. We have very important updates for today. 2s Absolutely. So, as we've said, 
some of our tweets we've mentioned that some of the topics that we want to cover today are bad badges. 
The upcoming fork that we have planned, as well as the split in our binaries that is coming. It's pretty 
exciting news. It's big news. This is a big engineering call, so we want to jump right into it. With that 
said, I want to open by talking a little bit about badges. We'll have Gil answer some questions about 
badges, but to give an intro to those who are less familiar. Badges are an object that's used by lava in our 
SDK on the front end in order to, you could say, obscure the private key. It's one of the ways that you can 
authenticate, and it's a temporary access pass to allow someone to make calls, RPC calls on the SDK. It's 
a very cursory explanation, and we're going to get a much more clear, clean one from Gil in a moment 
here. But for those who have not checked out our gateway and our SDK badges are now live, you can go 
to Gateway Lavanet XYZ and check them out. Go ahead and use a badge in your front end application. 
But with that said, Gil, can you tell us a little bit about badges? Just a general overview of what they are 
and what they do. 2s Yeah, I would love to do that. I think badges, it's one of the coolest innovations that 
we have made in Lava. It's basically a way for you to use the full decentralized network in the front end. 
So in your DAP, directly in your DAP. Let's think about badges as like a WeWork where you need to get 
a badge and enter the building in. So you go to WeWork, they give you a badge and you can use that 
badge to enter and exit the building and enter the offices, et cetera. So this is exactly very similar to what 
U1
we have in Lava for front end users. But to take a small step back on why badges are necessary, we have 
to understand that a lot of other decentralized networks struggled with allowing access to decentralized 
networks via the front end because they did not have a mechanism like badges. Because, think about it, 
you have your public and private key. How do you communicate with somebody else and authenticate 
them without going through the blockchain? So, to build a solution for that, this is why we built a badges 
solution. So it allows you on the user front end to get access to all the providers on Lava from your front 
end. So for developers who are out there listening for a long time, we were saying that 1s the SDK was 
mostly useful towards the back end because there was not a real way to secure the private key. And 
badges are the solution for that. So we can now use use private keys. It's obscured through the use of 
badges. How do I use badges? Let's say as a developer? Or how are badges useful? 2s I think they're 
super useful because you can immediately implement them in your front end. So instead of using your 
private key, placing it on the front end, which would obviously compromise your private key, it's not 
something that you want to do. We have the badges solution. So imagine going to uniswap and then you 
basically generate your own private key pair on the fly and and you take the public key from the 
generated key pair, you send it to the badge server. The badge server signs it with its own private key, 
giving you basically a badge which you can enter lava with. This badge you can send now queries and 
get any RPC, any API access from the front end. So again, you go to uniswap, you generate your own 
key and now you can talk to the providers on lava directly with your own key and the badge provided by 
the badge server. 2s So you said the badge server the badge server is something that is run by lava. 2s I 
think that's a great question. We also run it so you can use our gateway and you can use our bed server, 
but you can also spin up your own. Super easy. We're actually working on some docs for that right now. 
But yeah, anybody can run it with their private key and basically sign on their behalf access to the 
network. 1s Awesome. So we have a new way for developers to essentially create front end applications 
using Lava SDK. More secure, allows for a person to even spin up their own badge server if they don't 
want to use the one that Lava has provided. Gil, thank you for that. 4s Another feature, or really a major 
development that we need to discuss is the coming fork in the testnet. Jacob, can I ask you what is the 
cause of the fork? 1s Yes, 1s the cause of the fork is actually quite a few features that triggered it. First of 
all, we are already running for over six months in the existing testnet since the beginning of January. And 
we decided that it's the right time in order to do a fork and to create testnet number two. So there are a 
few changes. We are changing the way that the specs are going to appear, so the format is going to be 
changed. Also, we are adding TLS support in order that the communication will be more secured. So any 
consumer will connect now to the provider with a certification. And the last thing is timestamps we had 
some issues with the existing chain, with the validators, which were external to Lava due to some 
configuration issue. Sometimes the blocks were faster. And we want now all the blocks to appear exactly 
on the same time in the new fork. In the new chain, we will have all the blocks every 30 seconds. 1s
These are the main triggers for doing this fork. 1s Understood. So what are some of the more direct 
benefits that the fork may confer to our users, to our developers, to our validators or providers? 1s Great 
question. So let's start mentioning them one by one. First of all, as I mentioned before, we are changing 
the specs. So the format of the specs will be changed. There will be a new engine for the specs that will 
allow now to define add ons in the spec. For example, a provider will be able to declare whether it's an 
archive node or a full node. A provider will be able to define whether it supports Debug EVM for all the 
ethereum. He will be able to define if he supports the Debug API. So we created a new structure using a 
collection that will allow us. 2s To provide all these new features. And in order to do it, there was a 
breaking change in the way that the spec behave. So in order to avoid it with a regular upgrade, we 
decided to do this fork. 1s All of these changes, some of them will be done immediately. Some of them 
will just give us the ability to use them in the future. So every two weeks, we will continue to do 
upgrades in the network, and the new engine of the specs will allow us these changes. 1s In addition to 
that, some of the APIs from now on will be optional. Right now, as you probably know, provider must 
support all the API in the Spec from now on. It will be allowed to declare some of them as optional 
again, as written in the spec. And the provider can decide whether he wants to support optional APIs or 
not. 3s We mentioned the TLS support. So TLS support is also really important. 2s It requires from all 
providers to change their endpoints. To be ones that support TLS, the providers will need to come up 
with a certificate with a domain and there will be no more IP only in order to connect to them. So that 
way it impacts also the consumers. Obviously anyone who is working with the gateway will not have any 
impact. But if you are using the SDK, so you will need to upgrade to the new version that supports it. 3s
There's a lot of really great improvements for our community I think excited to see how the improve 
security and features actually boost our use in how our community responds to it. A question for you is 
do you foresee any future forks during this testnet? 2s Yes, it can definitely happen. I don't see it 
occurring very often but still once a quarter or once every few months it can happen that we will do a 
fork. The importance here is our agility to do that, that it will have almost no impact on the team inside 
lava and on our community. So it will be something which is yet another tool in our toolbox that we can 
use whenever it's needed. 2s And when speaking about the security 2s of this fork and the new features 
that are being offered is one of those it will allow the SDK to connect from https 1s yes, exactly. This is 
what we talked about. The security and the ability to work with TLS so it will be more secured. I can 
even add to that. Regarding protection and security that we added also the ability to do filtering. Using 
headers and in case the header specs that are defined only predefined headers will be able to pass and if 
there are headers which are not known to the provider and are not declared in the spec itself so the 
provider will be able to filter them and not send them to the node itself. That way it will give better 
protection to any node runner. 2s Got it. So how will the fork be administered? How will it be 
implemented? 1s Yes, thank you very much. Really important. So, the way that we decide that this fork 
will happen, we will declare in our discord later this week about the exact date that it will happen. 1s
Once we will decide about the exact block in the existing chain, that the folk will happen. Let's assume 
it's block 1 million. So from that point, the old chain will continue to walk 1,000,001 1,000,002. The 
blocks will continue to progress with the existing Validators and providers. However, we will open a new 
chain about one to 2 hours later, after the last block, before the fork was signed. And then the new chain 
will run with new Validators and with new providers on our side, on Lava side, and we will declare and 
publish it in our discord so all the existing Validators can join together with us to the new chain. So we 
will publish, they will see the address book and they can join. So they will stay validators also in the new 
chain. Providers the same, they will be able to join and continue to work with the new chain. We will 
leave the old chain continuing to run for a few hours, maybe up to 24 hours. Once everything is stable, 
we will shift our gateway as well and our internal consumers will be shifted to the new chain. And that's 
it. Everybody can start working with the new chain. So, for clarity, will the Validators be chosen? How 
will the Validator set be chosen for testnet two? The testnet one Validators keep their vote power and 
tokens. 2s Yes, exactly. We will keep that. We will create a new Genesis file based on the last block that 
we mentioned. So assuming it's block 1 million. So the new Genesis file for testnet lava testnet two will 
be this 1s Genesis file, which will contain all the old Validator. So all the Validators can join. Obviously, 
if they will decide not to join or they will not do it within an hour or two from the time that we publish, 
they will be jailed, and it will open place for new Validators to join. But the expectations is that all 
Validators will continue with us to testnet number two. 1s All right, so it's very important that our 
community hears this, that testnet two is forthcoming, and the switch will be something that you'll want 
to participate in. Appreciate you sharing, Jacob. All the various benefits that we're going to get from this. 
And I'm excited to see this fork. 2s Yeah, thank you very much. Just one sentence maybe to add to that. 
All the onboarding from now on for the new chain will be faster and easier for Validators. 1s They will 
not need to run all the history anymore of our versions, 2s no need to do any manual upgrades like they 
did so far. 1s So I see it as a big advancement for the Validators, same for providers. All of them will be 
removed from the old chain and they will need to re register in the new chain with the new flags and the 
new parameters and from that point they will be able to enjoy from the new add ons that we are 
supporting from that point and on. Thank you very much. Yeah, thank you. Thank you, Jacob. Another 
big upcoming thing is the split in our binaries that's happening Lava D that we know we always 
affectionately use to make our key pairs and a few other actually all of the tasks of Lava really is going to 
be breaking into Lava D and Lava protocol. So some of the functions will be broken out into Lava D, and 
some of the functions will be broken out into Lava protocol. But we're going to have Omer speak a little 
bit more clarity on what's happening here and why it's happening. But before we get into the specifics of 
that, I just want to know, in general, what are the advantages of splitting repositories like this, or mono 
repos splitting binaries? I just wanted a little bit of information about what's some of the advantages of 
doing this. Hi. Thank you, gemny. So before we talk about splitting the binaries, we were thinking about 
how to handle the protocol code versus the consensus code. Because on Lava, we have two separate, you 
could call it source repositories. One for implementing the off chain protocol of consumers and providers 
and the RPCs and handling all those API interfaces that we always discussed gRPC, WebSocket, JSON, 
RPC, et cetera. And we have the Cosmos chain that implements the Lava consensus that is based on 
Cosmos SDK and is implementing our pairing mechanism and provider staking and consumer 
subscriptions and projects and all of that. So we could either split the two repositories or create a 
monorepo that handles everything together. So a bit about the advantages of splitting the repositories is 
that it's easier to onboard on a certain piece of code, but combining them in a monoreppo helps you sync 
up upgrades for the protocol with the consensus together. And the testing environment is much easier to 
run both when you're forking the repositories or when you're developing. So, to make it short, we decided 
to go with monorepo for stability and improvement on the pace of changes that we're making. 2s It. But 
there was a decision to split the binaries. What were the advantages of that? So once we decided to go 
with a monoreppo, we needed to decide if we're still releasing one piece of software or we're splitting our 
releases into two. Splitting the binaries is actually splitting the release process and splitting the release 
process. 1s It has these advantages and these advantages as well. And between the advantages is that we 
could deploy two providers fixes and features without our validators having to vote on an upgrade, which 
is a heavier vote to maintain during Mainet. An upgrade could actually affect how tokens are distributed 
and stuff that you really have to take into account and actually just upgrading the side protocol. The P, 
two P of providers and consumers might have a faster cycle because if you have change in an API that is 
coming and it's crucial and you can't wait a week or two or three for it to be deployed. So separating the 
deploy process for providers and consumers allows us to deliver faster and improve 1s the functionality 
in a way that doesn't affect the entire ecosystem, but only those that are participating in the relays 
themselves, which are our providers and the consumer 1s parties that are running either the Gateway or 
the SDK or back end users. The server kit, it. It. So from a protocol perspective, it sounds like we're 
saying that there are patches that can be more quickly deployed that affect maybe the consensus but don't 
affect consensus but are crucial to the protocol. Exactly. And there will be still some upgrades that need 
to be synced. And this is why we're going to have a mechanism for suggesting the latest protocol version. 
This is going to be handled on chain while we have a recommended version and the minimal version. 
And for such breaking changes that we decide to do as an ecosystem and we say okay, let's upgrade, for 
example, the protocol that consumers providers are communicating with, which affects the proofs on 
chain for payments. Then we will do an upgrade for the minimal version and we'll say it in advance and 
try to minimize breaking changes as much as possible because we understand how painful it could be. 
And for those patches, for example, exposing 2s prometheus metrics, we're going to just allow providers 
to upgrade at their own time and get the extra features. They do not affect consumers, they do not affect 
anybody else and it changes the binary. So previously we would be super careful with those kind of 
things because it could break the consensus code by changing the binary, even if it doesn't supposed to. 
It's not supposed to because it doesn't change consensus state transitions. It could be affecting it and it's 
something that we don't want risk. But while separating the binaries then the risk of somebody 
mistakenly changing the consensus.
