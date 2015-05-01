## Intro -- A few words on bitcoin

“Nerd gold."  
“If paypal and gold had a baby...”  

Libertarian catnip:  

* You can now buy drugs on the internet! 
* And degrade central gov ability to wage war by sneaky inflation tax!

Wild west phase -> Regulated phase

Like gold

* Scarce
* Useless.  
* *Almost* useles.
* Gold: Jewelry, electronics
* Bitcoin: It costs bitcoin to send bitcoin (transaction fees, typically 0.1-0.5 cents now) (but you can send without transaction fees for now, it just takes longer)

### And now a word from our sponsor

*Should I buy bitcoin*

bitcoin monetary base:   
Prelude> 14e6 * 235 — bitcoin mined * usd — ignores lost/burned bitcoin  
3.29e9 — 3.3 billion dollars

gold monetary base:  
8.2e12 — (8.2 trillion dollars) — http://en.wikipedia.org/wiki/Gold_reserve#Officially_Reported_Gold_Holdings

Worldwide m2: 6e13

Bitcoin maximalist moondream success scenario: 1e13ish.

Prelude> 1.0e13 / 3.29e9 * 100 -- (for percent)
303,951%

Buy buy buy!

(But extremely risky, past returns, future results, yadda yadda.)

## It's a jungle out there

Like gold:

* buy it
* earn it
* or steal it
* 7% of all bitcoin has been stolen.  (Credible estimate)
* or lose it
* (another 5-10% lost)

### Preventing theft

* use two factor auth (2fa) for web wallets
* don't use web wallets
* use deterministic bip32 paper wallets
* generate the paper wallets on a non internet computer with virgin live booted os (aka cold wallet)
* use farrady cage
* ......
* generate transaction on watching wallet / sign transactions on cold wallet / broadcast transactions on watching
* moving unsigned to cold wallet and signed to watching/broadcasting wallet is a pain point. (common practice is to use usb stick)


### Passphrase security

* Any phrase can be used to generate a private key (via sha256sum) 
* Aka brainwallet
* "Correct Horse Battery Staple" <<-- Don't do this.
* Correct Horse Battery Staple has around 40 bits of entropy (and it is splashed all over the internet because it is xkcd comic)
* Private keys actually have 256 bits of entropy, but that's overkill
* Gold standard for deterministic wallets is 128 bits of entropy for a passphrase. (Bip39)
* 128 bits is 12 words from the bip39 curated dictionary.  
* Paranoid practice: use dice to choose words from curated dictionary. (Avoids /dev/random backdoor)

### Preventing loss

How to lose treasure and get depressed

Gold: 

* Bury it and forget where you buried it (Or lose gps coordinates)

Bitcoin: 

* Forget the password that can be used to generate private key file (or lose private key file) 

Loss prevention

* write down passphrase and put it somewhere safe 

Succession

* have  a succession plan for die in a plane crash 
* deadmans switch
* give keys to someone you trust

Multi Sig -- helps with all of above

* use multi sig wallets 
* so your wife AND your mom AND your mistress all have to betray you...
* haskoin supports this so more on this later
