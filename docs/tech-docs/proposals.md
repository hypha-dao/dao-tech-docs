
## Creating a Role Proposal
The ```propose``` action is used to submit a proposal to Hypha. 

The ```proposals``` table within the Hypha DAO contract uses the Telos Kitchen Sink design pattern (link coming).  

> This pattern is called the "Telos Kitchen Sink" pattern because it can store nearly any arbitrary data and it can adapt without upgrading the underlying EOSIO table structure.  ["Everything but the kitchen sink"](https://idioms.thefreedictionary.com/everything+but+the+kitchen+sink)


Each record in the table contains:

- proposer (name)                                   -- must sign the ```propose``` transaction
- proposal_type (name)                              -- must be ```roles```, ```assignments```, or ```payouts``` 
- trx_action_name (name)                            -- must be ```newrole```, ```assign```, or ```makepayment```
- map of strings to arbitrary strings               -- length? should be pretty big
- map of strings to arbitrary name values           -- [a-z]|[1-5]$1-12
- map of strings to arbitrary asset values
- map of strings to arbitrary time_point values
- map of strings to arbitrary int values
- map of strings to arbitrary transaction values
- map of strings to arbitrary float values


The propose action 