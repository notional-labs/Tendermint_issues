# Tendermint issues

This Repository was created in 2021 to document issues with invalid block parts on Sentinel.  

More recently similar issues occured on a cosmos mainnet.  There is a possible fix:

* https://github.com/cometbft/cometbft/pull/904

The HackerOne report for this, which got no response for 18 days, and has never been paid a bounty on is here:

* https://acrobat.adobe.com/link/review?uri=urn:aaid:scds:US:c14b2605-3497-381a-870e-510b92e33f75

Details in this repository are here:

* [Attack On Sentinel](./Attack_on_sentinel.md)


## Proposed Mitigation

**upstream**
* Reduce the default maximum block size to 5mb.

**downstream**
* Do a migration to reduce the default maximum block size to 5mb.
