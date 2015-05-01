## Pre-intro

We will (hopefully) be messing with haskoin later.  To decrease installation hassles, we plan to use docker.

### Goal

* docker run -it tphyahoo/haskoin-wallet:0.0.1 # a local haskoin-waloet v0.0.1 environment (minus install hassles & wait)
* originally created with https://github.com/tphyahoo/haskoin-wallet-docker/blob/master/Dockerfile

For a docker environment, do one of paths below (or both if you prefer)

### Path 1 -- Provision local environment

* install boot2docker 
* sanity check: docker --version
* docker run -it tphyahoo/haskoin-wallet:0.0.1 # should download what you need

### Path 2 -- Use already provisioned server 

* log into provided server with privkey below 
* you are sudo (required for docker, alas)
* server will disappear after talk after some indeterminate time
* sanity check (same as Path 1 above, but use sudo)
* docker run (same as Path 1 above, but use sudo) 
* ssh -i haskointalk@104.236.183.128 # password: letmein
* recommended to create user sandboxes to muck around here
* save your work somewhere off server if you do anything notable 

## Bored by my talk? Join my holy quest!

### Hard: 

Wanted: a dockerfile (or in extremis an image) for installing haskoin-faucet.  

* Requires wrangling haskoin-wallet 0.2.0 first
* yesod version skew
* probably requires latest ghc 7.10.1 and various library updates

Any install gurus around here?

### Less hard: 

Help me generate etag/ctag tag files with entry point in script/hw.hs, for easier code navigation. 
