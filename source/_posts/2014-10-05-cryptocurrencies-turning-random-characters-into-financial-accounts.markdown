---
layout: post
title: "Cryptocurrencies: Turning Random Characters Into Financial Accounts"
date: 2014-10-05 04:44:09 -0400
comments: true
categories: 
---

For the past month I have been spending lots of my time researching cryptocurrencies, most notably Bitcoin and Stellar. After having read multiple whitepapers, manually generating and executing bitcoin transactions on the blockchain and speaking with many of the thought leaders in the space including: [Fred Ehrsam](http://www.coinbase.com/about), [Joyce Kim](https://www.stellar.org/about/) and [Richard Gendal Brown](http://gendal.wordpress.com/about/), I am more convinced then ever before that cryptocurrencies are here to stay and will revolutionize the way our world works. In this post, I want to share with you two of the fundamental realizations that have lead me to this conclusion and also some hurdles that are already slowing down the expansion of cryptocurrencies.

## Permissionless Financial Participation

In the traditional financial system, in order to transfer money without the use of physical cash, you need to open a bank account. <!--more--> This process can only be done by human beings within commuting distance of a banking institution willing and eager to open accounts. This is something we take for granted in the developed world, however there are over 2.5 billion individuals who do not have this luxury. In contrast to this cumbersome and physical method of joining the financial system, cryptocurrencies allow anyone with access to an internet enabled device to open a financial wallet, from which they can receive and send payments worldwide with near-zero fees. What makes this possible? The fact that cryptocurrency accounts are nothing more then randomly generated strings of letters and numbers that can be uniquely identified. 

{% img center /../images/farmer-phone.jpg 400 300 %}

To make this clearer, lets go ahead and generate a bitcoin address that can store, send and received bitcoins (special thanks to [Key Shirriff's blog post on the raw bitcoin protocol](http://www.righto.com/2014/02/bitcoins-hard-way-using-raw-bitcoin.html)). The first step is to literally generate a 256-bit string of letters and numbers which will act as the bitcoin addresses private key (analogous to a password). 

	# Warning: this random function is not cryptographically strong and is just for example
	private_key = ''.join(['%x' % random.randrange(16) for x in range(0, 64)])

This will result in a private key that looks like this:

	private_key = '3f42e34aebc34aff098c901a4ddf658afb29f80d1d158ed2b01fd982fe3aa230'

From this private key, we can now generate a public address we can send to whoever we would like to receive funds from. In order to do this, we will use the Elliptic Curve DSA algorithm followed by SHA-256 hashing, RIPEM-160 hashing, and then Base58 encoding with checksum to end up with the public bitcoin address:

	signing_key = ecdsa.SigningKey.from_string(private_key.decode('hex'), curve=ecdsa.SECP256k1)
	public_key = ('\04' + signing_key.verifying_key.to_string()).encode('hex')
	
	ripemd160 = hashlib.new('ripemd160')
	ripemd160.update(hashlib.sha256(public_key.decode('hex')).digest())
	bitcoin_address = utils.base58CheckEncode(0, ripemd160.digest())
	
With just a few lines of code that could run on even the simplest of computing devices we already have the equivalent of a bank account in this new financial system. 

## The Alternative (ACH) is a Poorly Designed, Closed System

The more I learned about the way cryptocurrencies work, the more I wanted to compare it to how the existing financial system works. [Richard Gendal Brown's blog](http://gendal.wordpress.com/) was very helpful in this regard, as well as [Edward Kim's post](http://engineering.zenpayroll.com/how-ach-works-a-developer-perspective-part-1/) on Zen Payroll's blog about how ACH (Automatic Clearing House) payments work from a developers perspective. It turns out ACH was developed in the 1970's and is entirely a file based system which uses secure FTP to transfer debit or credit files detailing transactions between two parties (Yikes!). All one needs to execute such a transaction is the bank account numbers of the two parties. There is no password, or other form of consent to send a charge across ACH.

{% img center /../images/ach.png 450 257 %}

So how does this encrypted but otherwise credential-less protocol protect against unauthorised charges on accounts within its system? It doesn't! It has merely implemented the equivalent of "ctrl-z" where any institution in the network can undo a transaction by applying the opposite transaction back to the network on behalf of a complaining customer. The reason this does not happen much more frequently is simply because they limit access to the ACH network to chartered banks who act as Originating Depository Financial Institution's for other companies needing access to the system. 

In comparison with ACH, Bitcoin is clearly more secure in that no one in the network can credit your account without access to your private key. With this control returned to the end customer, we can now exist in an open system without gatekeepers to safeguard our accounts. In the future, companies such as Zen Payroll will not have to go through the enormous barrier of having to convince a bank to become their ODFI in order to start accepting business. They could simply join the global payments network that is bitcoin and begin making people's lives easier through exceptional payroll services. This begs the question, how many other financial services companies would exist today if the barrier to joining a payments and settlement network was less cumbersome?

## The Hurdle of The Narrow Bridge

Many of the exact reasons why people are getting excited about cryptocurrencies are the very reasons financial regulators are very nervous about their proliferation. The existing financial system has evolved within a walled garden of sorts where the government has tremendous power and oversight into financial institutions and their regulation. Although cryptocurrencies are not illegal, any US business dealing in their transmission must register with FinCen (The Financial Crimes Enforcement Network), comply with AML (Anti-money Laundering Laws) and KYC (Know Your Customer practices). In addition to this, they also need to register for a MTL (Money Transmitter License) in 48 states, each with different requirements and governing bodies. This is an absurd amount of regulation which will impede all but the most well funded of startups. 

If you do get through these hurdles, finding a bank willing to do business with you will be your next challenge. Bank have received a stern warning from FinCen that if they do accept cryptocurrency business, they will need to brace for stricter audits. This has made many banks unwilling to take on these types of businesses, and those who do (Silicon Valley Bank) are being very selective on their clientele, usually requiring a chief compliance officer on the team and backing from a premier VC firm. This list of barriers is creating a narrow bridge between the fiat and crypto worlds as only a handful of companies are successfully acting as intermediaries between the old financial system and the new. 

In order to secure the success of cryptocurrencies we must work on widening the bridge between fiat and crypto. We need to make it easier for people to move their funds into cryptocurrencies and make it easier for entrepreneurs to start companies that deal with their transmission without legal repercussions. Financial regulation has already stopped many entrepreneurs from starting businesses in this space and this deterrence will continue. As pointed out by [Josh Whiton on his blog](http://joshwhiton.com/startups/the-next-silicon-valley/), which ever country succeeds in passing and maintaining crypto-friendly regulation will unleash a tremendous amount of financial innovation.




