0:00
25s You. 2s Testing. Testing. 3s Loud and clear. 30s Hey, guys. 3s Hey. Welcome. 3s Thank you. 1s
Exciting day. 3s Long time coming. 8s We'll give a few minutes for our people to join up. 81s Somebody 
just produced a lot of white noise. Yeah. What was that? 2s Sorry. Curious what these buttons do. 3s
Don't press the red button. 51s We need some elevator music or something while we wait. 4s Think Lava 
needs to hire a DJ. 6s I can't, of course, find a decent lo fi track for us to listen to. 13s Oh, man. Ads. 117s
All right, briefly interrupt our music session to welcome everybody to this. We appreciate the patience 
for those who arrived on time. We're so excited for today. Today is the announcement that our SDK is 
live in Alpha. And we're very excited to be here today, sharing our hard work on Lava SDK for the first 
time publicly. I'm Kagemni karimu. I'm Lava's Devrel. We have some amazing guests today, Gil and 
Omar, who really need no introduction, but why don't we do a short roundtable introduction with Gil and 
Omer? 2s Yes, sounds good. Yeah. So I'm gil. I'm the CTO and co founder of Lava. Super excited to be 
here and share with you all of our hard work, the team's hard work in the last few months launching the 
SDK Alpha. So super excited to be here. 2s Hi, guys. I'm Omar. I'm Lava's, chief architect officer. 
Excited to be sharing this product with you and our innovations, and feel free to ask questions to hear 
more. 3s Well, welcome to you both. Excited to have you both here. For those who are new, joiners, 
newcomers and less familiar. We'll do a quick recap on some of the basics behind Lava. The first thing is 
that Lava allows for decentralized public APIs and RPC. And you may be asking, what is RPC? Well, 
RPC is the standard communication protocol used to interact with blockchain nodes. RPC is used 
thousands of times whenever you use any web Three app or service. And most apps today rely on a 
single provider or public RPC, which is unreliable, lacks privacy, and pretty much unverifiable. Lava 
solves that problem. Lava is an open source, community led standard for accessing any blockchain. It 
connects consumers, developers, apps, services with multiple providers instead of one, and hence it 
provides a much more reliable, private and accurate experience for users. It also incentivizes RPC 
providers to provide the best possible service and disincentivizes bad service and malicious and 
fraudulent activity. Today we are celebrating and announcing the launch of the Lava SDK in its public 
alpha. Lava SDK is a TypeScript JavaScript implementations of the Lava protocol, and it enables direct 
peer to peer communications between developers and RPC providers. On Lava, I've used it quite a bit 
myself, and I can say it's a great solution for web Three builders that are starting to build more and more 
crosschain apps. And it works from both back and front end, the latter via badges, which we'll soon 
explain. We're releasing the SDK and alpha so that builders can use it and help us improve by sharing the 
feedback. Thank. With that said, I would like to ask Gil of the Lava SDK, how is it different? We see all 
these different offerings. How is it different from other RPC offerings? 2s Thank you for that, Kagemny. 
This is a great think. You know, before I dive into what makes Lava and our SDK different from other 
RPC offerings, I would like just to first say as Omar and repeat what Omar said, feel free to post any 
messages, any questions, we'd love to answer those. And that we are super excited to launch the alpha of 
our think. You know, what makes Lava unique is the fact that we are basically building a way for node 
runners and for contributors to build different RPCs for different chains and different APIs for different 
chains, offer them on Lava and then have our consumers basically adapt a bridge, a wallet. Anyone who 
wants to get access to this data, basically buy a subscription on chain and get peer to peer access to all of 
these providers. So. 2s Think about one stop shop where you can just from the command line, get access 
to all the APIs and all the RPCs that are available on all the chains. And this is really our vision. And 
what makes it unique is that we've built this protocol around reliability, privacy, being censorship 
resistant, and allowing for multi chain, multi API support at scale. You so not only you're not going 
through the lava network, when you're accessing these providers run nodes and give you access to data, 
you're talking to them directly. And this is I think the most meaningful part is that it basically allows the 
communications to scale because there's no middleman that you have to go through. There's no proxy 
that you have to go through to get the data. You can just talk to them directly, get the data, and there's 
economic certainty that the data is accurate. I think this is one of the biggest innovations 2s that's pretty 
phenomenal. I want to offer to Omar opportunity to explain a little bit about how all of these various 
features work. 2s Thank you, Gemini. So the Lava SDK is library that is as you said, integrable from a 
backend or front end using TypeScript or JavaScript and it's an underlying library that we are also 
U1
working to integrate with common libraries such as Etherjs. Custom JS was also recently integrated by 
our great community members. 1s Kudos to you Ruslan and 1s other libraries as an infrastructure library 
that handles the communication so you as a developer don't have to really change anything in your DAP 
in order to adapt to a fully decentralized and authenticated access. You don't need to enter a URL or 
change any code, you just use the modified underlying technology within the library that you already like 
and use and behind the scene in a decentralized manner. Getting the list of peers based on seed node 
inspiration kind of solution and it takes that in order to bootstrap to your list of providers on the chain 
you're interested in, for example Ethereum, Mainet and then directly peer to peer connects with a list of 
providers. So you get your own sea of nodes access from your DAP and you don't have to worry about 
scale and you don't have to worry about a backup. And also because it's simultaneous, you get out of the 
box data reliability which compares answers for you to identify fraud and quality of service between 
providers to get the best ones, so you can adaptively get the best provider out of a list of providers based 
on your user. So you have a user using your DAP and it gets the best possible solution for him, not for a 
benchmark you did with your back end and development environment, something that runs Live in his 
DAP while the user is using. And this goes also on chain so you can get feedback on it both for the 
provider and the consumer, while the provider gets paid in the testnet tokens or Mainet tokens when we 
launch. 5s Awesome. I'm sorry, I was stuck on mute there. Awesome. 1s I really appreciated the note that 
you made about the developer experience, having that simple process to initiate multiple chains. It's 
something that a lot of our developers who have tested it have celebrated and something that I hope 
developers who are just becoming aware of it actually appreciate. About the Lava SDK, how much it 
simplifies your interfaces. Exactly. 1s I'm hoping that now we would see more multi chain SDKs to allow 
you to do stuff with multiple chains instead of focusing on one single chain at a time. Because of the 
RPC problem that we are aiming to solve with one SDK, one initialization that's decentralized. And also 
suddenly you get access to tons of APIs and not limited to just the set that is limited. Per that. And then 
you could do some very complicated stuff as part of a higher level SDKs for developers, lowering the 
barriers of entry for developers to do that. So I'm hoping we'll see more open source projects developing 
libraries on a MultiChain access base, which is something we are unlocking right now. 2s I'm hopeful as 
well. I would like to ask Gil if you could speak a little bit about what some of the main features that we're 
seeing with Lava SDK currently. 1s Yeah, of course, I would love to. I just would like to say, Omar, I 
like what you said about sea of nodes, your own personal sea of nodes. First time I'm hearing it. And I 
love 1s yeah, you know, I think it's important to explain the main features of what we're basically 
offering in the SDK. And it's that our SDK basically allows you, as we've explained before, an easy 
access to many, many chains using it. Right now we have something we call the gateway integration. It's 
a way for you to use our UI to basically set up everything that is necessary to create the communications 
and the settings for setting it up. And we support both backend and frontend. And I think why is this an 
important distinction? 1s I would like to make a point. When you're building a decentralized network and 
1s you want the consumer and the provider to communicate in a peer to peer way, meaning without going 
through any middlemen, you have to design a protocol around that. And this is one of the biggest 
innovations that we are launching today in the SDK. And the question is, how do you allow your users, 
let's say your uniswap, how do you allow your DAP users to get access on your behalf to all of these 
amazing sea of providers? How do you allow them to do that? So we've built a custom system that is is 
super innovative and we call badges. 2s Think about when you go to WeWork. You have your badge, 
you go, you enter the building, you have to use a badge that was given to you by the company. So this is 
very similar to that. We basically allow the user of the DAP, for example, you go to uniswap, you want to 
make a swap. We allow the user directly and by the way, seamlessly to them, create their own key that 
they can use to communicate with providers. And then they get a badge from uniswap. They attach the 
badge to the key, and now they can communicate with the sea of nodes that Omar was talking about. So 
basically, anyone can run a badge server and provide badges to their users. And in that respect, we're 
keeping it completely decentralized. I hope that's clear. 2s Yeah, I love what you said, and I want to add 
on top of that why the badges is such an exciting feature, because those keys are cryptographically signed 
and can be used by the providers to get payments, but it cannot be abused. So it's also safe in terms of 
protocol and accountable, meaning that later what your user is doing is also 1s being accountable by the 
providers servicing that on chain on his quality of service metrics. So we have this accountability 
mechanism due to the badges and it allows you to use it. Now, one more important thing about those 
badges is we are assuming a front end user could be compromised. And in terms of security, those 
badges are a limited time based impact on your usage. So as a developer, you know, they cannot be 
abused, they cannot bypass what you're allocating for a specific developer, even if it's peer to peer and 
not controlled by you because otherwise you would say, hey, but somebody gets access to my RPC and I 
cannot see this transport, right? So you can know that this transport is limited. Buy the batch you 
provided and it's protected by on chain mechanisms to limit that. You cannot be overcharged more than 
each specific badge. 2s I love this concept, and I think it's important to highlight the gradual migration in 
Web Three away from just throwing private keys everywhere. I think we've seen how that can end badly. 
So I think Badges is an excellent example of a solution to that problem. 2s Another thing I wanted to ask 
about was we've had some questions from a lot of our developers about they maybe use popular Web 
Three libraries like Web Three JS, Ether JS, and Cosm JS. Can you all speak a little bit about the 
integrations that exist with the libraries? 5s Yeah, of course. So I think to make it super easy to use, it's 
important to work with existing frameworks. So we have Ether JS, we have Cosmjs, and in the pipeline 
we have Wagme and Wallet Connect. And I might be forgetting other libraries, but for us it's extremely 
important that Lava SDK is drop and play. You just copy import your NPM, install it, you drop in two 
lines of code and you have to change nothing else about your environment. We hope that making it 
seamless easy to use will help with the adoption of the library because of all the benefits you're getting 
versus just connecting to a single endpoint, which is obviously a single point of failure. So in Lava, you 
get access again to a sea of nodes through the SDK, and you can provide these sea of nodes also to your 
end users through the front end. 2s Great points and that the question was a bit self serving and that I 
wanted to take an opportunity to shout out one of our developer champions who used Lava SDK and one 
of his own projects, which was pretty extensive DAP connecting telegram to Cosmos Validators and then 
decided to help with the integration of Cosm JS into Lava SDK. So also on the lookout for those who are 
interested in helping Lava SDK to interface with other ecosystems. Which brings me to another point 
about why right now we're releasing this in alpha. I just want to reiterate that Lava is in its testnet phase 
and so as such, we're always looking for devs to contribute and help build. The SDK is open source, 
anyone can propose new specs and our community can move faster than many centralized providers. 
Because of that, you have Lava SDK, which you can test out, you can contribute to the open source code, 
you can look at our repos and everything that you do in this phase of alpha helps us move faster to 
Mainet and brings decentralized blockchain access to the entire web three space faster. So 2s we know 
that centralized providers are not the problem, but they are not enough. And Lava gives an opportunity 
for anyone, from the smallest developer to someone who has a very large team, the capacity to 
decentralize and find reliable RPC. So definitely get your hands on that and we can talk more about 
where you can do that there. But I wanted to say a few words about why a dev should join the SDK alpha 
testing movement. It's a very important thing for us to have as much feedback from as many parties as 
possible to make sure we're building the best thing. 1s And with that said, what are some of the future 
improvements or functionalities that developers can expect? 1s I leave this open ended to both of you. 3s
So I'm really looking forward to one of our exciting features called Optimizer the picker optimizer. It's 
add on on the SDK that allows the SDK to track the quality of service and the latest block of each of its 
providers. And depending on the information that is queried, it knows to pick the right provider. So let's 
say you need some historical data, then you would go to the provider with the highest availability and if 
you need the freshest data, you would go to the provider with the latest block. Statistically, it was the first 
one to provide you the latest block. So this innovation allows you to give the best quality of service to 
your users. And also you can define a strategy. Let's say you always want the latest and freshest data or 
you want the fastest loading time, so you want latency to be the scoring factor so you can differentiate 
how the optimizer calculates the best provider. And these features allows you to benefit from access to 
many providers at once. So it's an exciting feature. It's already integrated into our gateway solution for 
access through our managed solution and bring it to the SDK thing I'm really excited about. 3s Yeah, I 
agree. That's a super exciting feature and I think there are many more features. As we've said, Lava SDK 
is launching, is launched already today. You can go and check it out, try it out. It's in an alpha state, so 
that means that there are some bugs, there are some features that are missing, like Omar said, one of 
those features. But we are planning on pushing strong and bringing these features to the SDK as soon as 
possible. You can also track the development on our GitHub and obviously, if you're interested, we'd 
love to have your help hands on trying the SDK and joining our Discord community, providing feedback 
and even providing contributions to the code. 2s I want to reiterate that those things are really important 
to us. We do take feedback very seriously. I myself am constantly mining our community for input on 
how we can improve what we're doing. So definitely if you're someone who likes the exploration, you 
like to try the cutting edge Web three stuff. This is a great time to become involved with our SDK 
because it's very formative stages and this alpha has shown us a lot, even in terms of what developers are 
looking for. I do want to say if you want to get started, you can definitely check out our docs and visit 
our gateway, gateway Lavanet XYZ. Or you can visit our docs at docs lavanet XYZ. Just take a look 
around. There should be some pages that hint you in the correct direction. The major thing is we now 
have it where you can go directly through the gateway and get your badge there and get started. Or you 
can also build some back end, which the docs can give you a little bit of guidance on how to build a 
backend environment that will be safe with your private keys. But either way that you go, it's a great time 
to get involved with Lava SDK alpha. We look forward to your feedback. Definitely join us on the 
Discord where we're really lively. We love to answer questions and interface with the community, and 
we have upcoming community and engineering calls dates forthcoming that you can of course join us on 
and speak with us further. But this is an opportunity for us to really just announce and introduce this 
Lava SDK alpha. I hope that it has been informational and enjoyable to you. I wanted to offer one more 
time Gil and Omar if you had any closing thoughts or words I don't think we have. I want to invite also 
anybody from the community if they have any questions or input that they'd like to share here now. But 
otherwise I'd like to invite Gil and Omer. 2s It. Appreciate you taking the time to listen to this. We have 
just released our gateway badges integration. It's something that was just feature opened in the last 30 
minutes. So please check it out. I myself used recently before hacker house in Solana, the SDK 
playground repository within the Lava Net system on GitHub. So you can play with a repository in 
playground that can help you do multi chain access. So I encourage you guys to try it out either through 
the gateway or directly from GitHub. And if you have any questions, feel free to pop up by Discord. 
Kagemi is super active. There we have our developer Champion program that you can hear more about 
on Discord and appreciate you. 2s Yeah. On the same note, 2s we are super excited to launch this SDK 
Alpha and we couldn't have done it without the amazing team at Lava in the community. So thank you all 
for contributing and really putting an effort and your sweat into this project. I feel like it's amazing and I 
want to shout out to the entire team for pushing this forward. And yeah, guys, join us for the evolution, 
the revolution of data on Blockchains. Join us and help us push the message to everyone. Appreciate you 
coming here. Thank you. Yes, I want to echo our shout out to the community who's been very 
instrumental in finding bugs already and assisting in this. And just want to say thank you everybody who 
has joined this call, who has taken the time to listen and who will try out the SDK. We look forward to 
your feedback. We look forward to getting to know you a little bit more. This has been introducing Lava 
SDK Alpha. Thank you for attending. I'm Kagemny Karimu. Lava's Devrel. Engineer signing out. Take 
care everybody. Thank you everyone.
