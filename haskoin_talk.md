# Haskoin (Bitcoin library for haskell)

haskoin / haskoin wallet / haskoin faucet

## Why haskoin? 

* 1 word.  (3 words?)
* Floating point arithmetic
* Dominant bitcoin ecosystem is javascript (bitcoinjs / bitpay's bitcore)
* http://www.reddit.com/r/Bitcoin/comments/33u2id/help_losing_over_85_btc_because_of_bitgos_flawed/
* Bitgo transaction recovery bug was a javascript floating point bug
* Types are nice! (Especially when you're dealing with money and security matters)  

## Ecosystem / discoverability:

* open source
* documentation, meh
* commercial software in sorta-stealth mode
* no mailing list
* has irc channgel but high latency
* I'd like to make haskoin more mainstream... we'll see.

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
* hw, command line tool.  similar to sx / bx (from darkwallet), but with less functionality

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

# Let's move some bitcoin around

* generate a bip32 wallet from a passphrase
* hw init 'correct horse battery staple' # not sure what transform hw uses to get a bip32 master key.  would be interesting to know.
* generate some accounts 
* hw newacc testaccount1; hw newacc testaccount2 # later we will be sending money from testaccount1 to testaccoun2
* 
* unlike satoshi/aka bitcoind and many other clients, these accounts can be specified as a source when sending bitcoin from the wallet, rather than all accounts mixed together.  Sometimes this notion of an account is called a "purse" (like in darkwallet)
* generate some addresses for the account
* send some bitcoin to your wallet from pre-existing bitcoin source (don't have to actually send bitcoin -- can just generate signed transaction and not broadcast it!) 
* get hex transaction, eg
* https://blockchain.info/tx/b134d7b90b95923fcc4565fa26ec08063732c738214f1997ba1b6c5e18f4114b (not hex)
* https://blockchain.info/tx/b134d7b90b95923fcc4565fa26ec08063732c738214f1997ba1b6c5e18f4114b?format=hex (hex)


Create wallet



I would have liked to use testnet / testcoins, but doesn't seem to be supported in 0.1.0, so used small amount of mainnet (real bitcoins).














