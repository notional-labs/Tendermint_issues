# Table of Contents
1. Attack On Sentinel Network
2. Investigate The Attack
3. Conclustion
4. Solution

# 1.Attack On Sentinel Network ?
Sentinel has been suffering from intermittent network congestion.

### The damage is kinda bad : 
- high bandwith consumption
- validators missing blocks
- validators not proposing valid blocks

Jacob found out that some validators is proposing very large block which is the main cause to the congestion.
Those blocks is so fat that it never got commited to the chain. Let's call them evil blocks. The validators proposing evil blocks is hence asshole validators (asshole doesn't mean evil, maybe they're doing it unintentionally)
Validators ( but not all the validators ) will become asshole validators, once they're in contact with an asshole validator 
Evil block has size in Mb. For comparision, a "normal" block has around like 10Kb of data

### Weird things about the attack :

- some validators seems to be immune to the attack while others is suffering from it
- all evil block has 336 block parts. 

For more detail, read this report from Jacob Gakidian [link to the report](https://hackerone.com/bugs?subject=user&report_id=1395694&view=open&substates%5B%5D=new&substates%5B%5D=needs-more-info&substates%5B%5D=pending-program-review&substates%5B%5D=triaged&substates%5B%5D=pre-submission&substates%5B%5D=retesting&reported_to_team=&text_query=&program_states%5B%5D=2&program_states%5B%5D=3&program_states%5B%5D=4&program_states%5B%5D=5&sort_type=latest_activity&sort_direction=descending&limit=25&page=1)

# 2.Investigate The Attack
This is how I and Jacob find out about the attack. If u only want the conclusion, skip this

### Theory

#### 1. Validators using modified binary :
Their motive of modifying binary is profit maybe, and thus by modifying binary they unintentionally cause some bugs lead to proposing evil blocks
Back up this theory :
- People using modified binary validating osmosis

This theory is however improbable, here is why :

- We're able to point out some asshole validators using a modified binary. They swear they don't modify the binary. Since they swear to god, they're telling the truth
- They stops being an asshole validator by simply restart the node

#### 2. Outside attacker :
Xi jinping attack on a vpn related project

### What is inside the block
- Each blocks is splited into one many block parts for gossiping, evil block is no different. Think of block as an array of block parts
- I collected block parts of many evil blocks. Since all the evil blocks are always not fully received by a node due to networking reason, I only got some but not all the block parts for each evil blocks.
- What I got for each evil block is like block A : [non_nil_blockpart_0, nil , nil , non_nil_blockpart_3, nil, ..., non_nil_blockpart_336]
- I notice that there're many kinds of evilblocks (not all of them are has the same data)
- I combine the blocks with the most similarity ( for example block A and block B having the same non_nil_blockpart_x for every non_nil_blockpart_x they both have). Doing so, I get a fully completed evil block, I check the data hash of the completed evil block, it is correct
- I was able to get all txbytes from the completed evil block I made and decode them. It's a bunch of ibc client update tx ( about 800-900 of those tx)
- All of them txs is comming from a single signer [sent1v5amm7pkk0mwccsly0pv6ggf0tfa6x4x4x4jkr](https://www.mintscan.io/sentinel/account/sent1v5amm7pkk0mwccsly0pv6ggf0tfa6x4x4x4jkr)

# 3.Conclusion
- Some dude decide to flood the sentinel network with ibc client update txs
- Since client update txs are huge, they makes the block gigantic which in turn cause the network congestion

### Question remain :
- Why some validators are affected, while others are not
- Why after reseting the node, a validator stops being an asshole validator
- Motive of the attack
- Why ibc client update txs (may be because they're cheap in terms of data/fee)
- Why 336 block parts 
- Why is there's not any kind of sanity checking for block proposing so that validators won't propose huge block

# 4.Proposed Solution
I think this is not an ibc issue but a tendermint issue, attacker merely exploit ibc txs because they're cheap in terms of data/fee. Maybe we should make some changes to the tendermint code base.  
