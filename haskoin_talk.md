# Haskoin (Bitcoin library for haskell)

## Scratchpad / party line(anyone can edit)

https://docs.google.com/document/d/1LWYoX9GEAl5V5uXyLoUXPr-xTSyPlCTi6OQWu6YJ5IE/edit?usp=sharing

## Sanity check / temperature / show of hands

* Who is comfortable programming in haskell? (show of hands)
* who feels like they basically understand bitcoin?
* Does anyone have basic newbie level questions about bitcoin, or should we skip that?
* who owns some bitcoin?
* who owns bitcoin that they can easily donate a buck or two later on when we do group exercise in a bit?
* you will probably want some bitcoin to play around with later.  google "bitcoin faucet" and see if you can beg some, or ask a bitcoin owner to donate some


## we will touch on

haskoin / haskoin wallet / haskoin faucet


## Why haskoin? 

* 1 word.  (3 words?)
* Floating point arithmetic
* Dominant bitcoin ecosystem is javascript (bitcoinjs / bitpay's bitcore)
* http://www.reddit.com/r/Bitcoin/comments/33u2id/help_losing_over_85_btc_because_of_bitgos_flawed/
* Bitgo transaction recovery bug was a javascript floating point bug
* Types are nice! (Especially when you're dealing with money and security matters)  
* personal goal: it would be cool to build stuff with bitcoin using haskell -- paying gigs / consulting / personal projects... we'll see.

## Ecosystem / discoverability:

* open source
* documentation, meh
* commercial software in sorta-stealth mode
* no mailing list
* has irc channgel but high latency

### haskoin (wrangled 0.2.0 install. but only ever used 0.1.0)

* 0.1.0 on hackage 
* 0.2.0 on github

### haskoin-wallet (wrangled 0.1.0 install)
 
* 0.1.0 on hackage 
* 0.2.0 on github
* 0.1.0 is fairly easy install, once you have right version of ghc
* Later versions are hard to install. I used docker with mixed results to wrangle the install efforts.  
* I was able to install haskoin 0.2.0 but not (yet) haskoin-wallet 0.2.0
* 0.2.0 has a lot more functionality, but is hard to install. 
* 0.2.0 has.....
* blockchain database, uses leveldb. arbitrary balance / transaction lookups.  
* aka a blockchain explorer, that lets you do blockchain queries
* testnet / testcoin
* zeromq (not sure why)
* watching only versus offline signing wallets? (not sure about this one)
* 0.1.0 is limited to...
* import transactions manually
* sqlite database for manually imported transactions
* the database only updates if the transactions you imported involve addresses/privkeys that the wallet knows about (that are living in sqlite)



### haskoin-faucet (unable to wrangle anything)

* on github
* not on hackage 
* had demo, now 404
* I originally hoped to do a teardown on haskoin-faucet, but I ran out of time trying to install it (but I still hope to)
* yesod front end over web faucet using haskoin-wallet

## Entry points to haskoin-wallet

* test suites
* hw, a command line tool.  similar to sx / bx (from darkwallet), but with less functionality

### Test suite

* cabal install --enable-tests # should already be enabled
* cabal test

### hw

* haskell@8e5176e39371:~/haskoin-wallet$ export PATH=~/.cabal/bin:$PATH
* hw --help # pretty much the only documentation there is, but it's enough to get started
* $ hw init 'correct horse battery staple' # this is the wallet we're using. Has about $1 in it.  Please don't steal it ;)
* more hw wrangling below

### sqlite3

haskoin (using Persistent) creates a wallet under  ~/.haskoin/walletdb

$ sqlite3  ~/.haskoin/walletdb

sqlite> .header ON

sqlite> . tables
db_account  db_address  db_coin     db_tx       db_tx_blob  db_wallet

sqlite> select * from db_wallet;
id|name|type|master|acc_index|created
1|main|full|xprv9s21ZrQH143K2mDJW8vDeFwbyDbFv868mM2Zr87rJSTj8q16Unkaq1pryiVYJ3gvoGtazbgKYs1v8rByDYg4LPpQPL6jHjwwhv7DWhWjyXo|1|2015-04-29 22:42:45.624478 UTC

By the way: 

There's our master bip32 key (this is not exposed via hw, only in the sqlite database)

Sanity check: http://bip32.org/ (paste xprv above into "derived private key" and play around, you will see other keys in wallet)  

Internal addresses are change addresses.

External addresses are addresses you give people to send you payments.  

# Preliminaries / Setup

Slight regret: I would have liked to use testnet / testcoins, but doesn't seem to be supported in 0.1.0, so used small amount of mainnet (real bitcoins).

Prediction: When we break up into groups and actually try to start spending the bitcoin, there will likely be orphans / side chains, and other kinds of book-keeping chaos.  Let's just see what happens :) 

* generate a bip32 wallet from a passphrase

```
hw init 'correct horse battery staple' # not sure what transform hw uses to get a bip32 master key.  would be interesting to know.
```

* you may want to generate additional wallets to send bitcoin to, eg 

```
hw init 'correct horse battery staple 1 ' (1 through n, or pick a better passphrase, or... be creative)
```

* generate some accounts 
* hw newacc testaccount1; hw newacc testaccount2 # later we will be sending money from testaccount1 to testaccoun2
* unlike satoshi/aka bitcoind and many other clients, these accounts can be specified as a source when sending bitcoin from the wallet, rather than all accounts mixed together.  Sometimes this notion of an account is called a "purse" (like in darkwallet)
* generate some addresses
* you can see the new addresses in sqlite if you want to, I think
* hw genaddr    testaccount1 ; hw genaddr testaccount2

```
haskell@8e5176e39371:~/haskoin-wallet$ hw balances # in satoshis
- Balance: 433490
  Account: testaccount1
- Balance: 10
  Account: testaccount2
- Balance: 0
  Account: testaccount3
- Balance: 0
  Account: testaccount4
- Balance: 0
  Account: testaccount5
```
* But the above won't show till the transactions have been imported so we need to chase down and import the transactions

```
haskell@8e5176e39371:~/haskoin-wallet$ hw coins testaccount1
- Script: 76a91402b833eaee3983fd86c0409db4eaf7e25f1b1d0388ac
  Address: 1FP226Khx3qNu2wrLM8QraKKiLMWu16tG
  TxID: 3b9c3510ba489a9086d18e955a1ebadb455e08038f116c0ef7ca305655a80457
  Index: 1
  Value: 433490
haskell@8e5176e39371:~/haskoin-wallet$ hw coins testaccount2
- Script: 76a9144ab68f1b58eede6fc5bfd860f7ba2a67114f58c688ac
  Address: 17p3hcqepcZ8kixLT21C451BnrLGbrN5YJ
  TxID: 3b9c3510ba489a9086d18e955a1ebadb455e08038f116c0ef7ca305655a80457
  Index: 0
  Value: 10
```

https://blockchain.info/tx/3b9c3510ba489a9086d18e955a1ebadb455e08038f116c0ef7ca305655a80457
https://blockchain.info/tx/3b9c3510ba489a9086d18e955a1ebadb455e08038f116c0ef7ca305655a80457?format=hex

haskell@8e5176e39371:~/haskoin-wallet$ hw importtx  `curl --silent https://blockchain.info/rawtx/3b9c3510ba489a9086d18e955a1ebadb455e08038f116c0ef7ca305655a80457?format=hex `

Disclaimer: not sure if above will actually work to get the bitcoin showing.

New Transactions can be broadcasted here:

https://blockchain.info/pushtx

# Group work period

Break up into small groups (2-4 people)

Pick one of these tasks to work on

## Task oriented:

* steal the "correct horse battery staple" wallet bitcoin (if it is still around)
* create a wallet with a different seed, populate it with accounts & addresses
* Try to get bitcoin showing inside this wallet (doesn't require broadcasting transaction, just requires importing transaction)
* Try to get bitcoin ACTUALLY in this wallet (requires broadcasting transaction)

Harder:

Generate a 2-of-3 (or 2-of-4 or whatever-of-whatever) account using hw, such that the consent of multiple trusted people is required to spend the bitcoin.

Disclaimer: I haven't actually done this and I'm not sure if it is currently supported, but hw --help seems to suggest it is possible.

starting point for this:

```
haskell@8e5176e39371:~/haskoin-wallet$ hw --help | grep multi
  newms      name M N [pubkey...]    Create a new multisig account
  addkeys    acc {pubkey...}         Add pubkeys to a multisig account
```

## Code oriented:

Easy-ish:

Programatically generate an arbitrary derived privkey (and/or pubkey) using 

```
xprv9s21ZrQH143K2mDJW8vDeFwbyDbFv868mM2Zr87rJSTj8q16Unkaq1pryiVYJ3gvoGtazbgKYs1v8rByDYg4LPpQPL6jHjwwhv7DWhWjyXo
```

as the master key. Check the result using tool at http://bip32.org/

EG, custom path (on bip32.org) m/0/2/0 should produce address 12XZzYUMEk5tbFmU2hKvBUruJ1x87H1iEh

Not so easy:

* Do a teardown on one of the transactions we have encountered so far.
* If it's showable, display it using Debug.trace

Generate an interesting transaction programatically using script primitives. For some value of interesting.  (Not even sure where I am going with this one to be honest!)
























