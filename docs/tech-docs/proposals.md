
## Creating a Proposal
The ```propose``` action is used to submit a proposal to Hypha. 

> The ```proposals``` table within the Hypha DAO contract uses the Telos Kitchen Sink design pattern (link coming). This pattern is called the "Telos Kitchen Sink" pattern because it can store nearly any arbitrary data and it can adapt without upgrading the underlying EOSIO table structure.  ["Everything but the kitchen sink"](https://idioms.thefreedictionary.com/everything+but+the+kitchen+sink)

Here's the method signature for the ```propose``` action.
```
ACTION propose (const name&                		 proposer, 
                const name&                      proposal_type,
                const std::optional<name>&       trx_action_name,
                const map<string, name> 		 names,
                const map<string, string>        strings,
                const map<string, asset>         assets,
                const map<string, time_point>    time_points,
                const map<string, uint64_t>      ints,
                const map<string, float>         floats,
                const map<string, transaction>   trxs);

```

#### Proposal Types
Hypha DAO *currently* supports 3 three built in proposal types.

1 - Propose Creation of a New Role  (```proposal_type="roles"```)
1 - Propose Assignment of a Person to an Existing Role  (```proposal_type="assignments"```)
1 - Propose a One-time Payout  (```proposal_type="payouts"```)

#### Required Values
Certain values are required to be within the ```map``` parameters for certain proposal types.   All proposals support/require ```title```, ```description```, and ```content```.  

Key         | Description                                                   | Type      | Example Value
----------- | ------------------------------------------------------------- | --------- | ------------------
title       | The title of the role, assignment, or payout                  | string    | "Community Manager"
description | A text-only field for summary info not suitable for the title | string    | "Energizing and engaging people to..."
content     | Any general purpose content field for text or markdown        | string    | "Full text of the role and additional materials..."


### Role Proposals
Role proposals require the following additional attributes.

Key             | Description                                               | Type      | Example Value
--------------- | ----------------------------------------------------------| --------- | ------------------
hypha_amount    | Per period payment amount in HYPHA for this role          | asset     | "50 HYPHA"
seeds_amount    | Per period payment amount in SEEDS for this role          | asset     | "45.50000000 SEEDS"
hvoice_amount   | Per period payment amount in HVOICE for this role         | asset     | "50 HVOICE"
start_period    | First period_id for which this role is active/eligible    | int       | 32
end_period      | Last period_id for which this role is active/eligible     | int       | 62

### Assignment Proposals
Assignment proposals require the following additional attributes.

Key                 | Description                                                           | Type      | Example Value
------------------- | ----------------------------------------------------------------------| --------- | ------------------
recipient    | The account to receieve the payment if approved         | name     | samanthahyph
role_id    | The ```role_id``` primary key from the ```roles``` table          | int     | 43
time_share  | The percentage of time (from 0-1) dedicated to this role  | float | 0.50

### Payout Proposals
Assignment proposals require the following additional attributes.

Key                 | Description                                                           | Type      | Example Value
------------------- | ----------------------------------------------------------------------| --------- | ------------------
assigned_account    | The account assigned to the role (also account to receive payment)          | name     | jameshypha11
hypha_amount    | One time HYPHA payment         | asset     | "50 HYPHA"
seeds_amount    | One time SEEDS payment          | asset     | "45.50000000 SEEDS"
hvoice_amount   | One time HVOICE payment       | asset     | "50 HVOICE"
contribution_date  | The date that the corresponding contribution (funds, labor, etc) was provided  | time_point | 2019-09-08T14:30:58.000

## Example EOSJS Code
The ```api.transact``` method in eosjs requires a ```data``` object.  Here are example objects for each proposal type.

### Role Proposal
```
data: {
    proposer: "johnnyhypha1",
    proposal_type: "role", 
    trx_action_name: "newrole",
    names: [],
    strings: [
            {
                "key": "title",
                "value": "Underwater Basketweaver"
            },
            {
                "key": "description",
                "value": "Weave baskets at the bottom of the sea"
            },
            {
                "key": "content",
                "value": "We make *great* baskets."
            }
        ], 
    assets: [
            {
                "key": "hypha_amount",
                "value": "11 HYPHA"
            },
            {
                "key": "seeds_amount",
                "value": "11.00000000 SEEDS"
            },
            {
                "key": "hvoice_amount",
                "value": "11 HVOICE"
            }
        ],
    time_points: [],
    ints: [
            {
                "key": "start_period",
                "value": "41"
            },
            {
                "key": "end_period",
                "value": "51"
            }
        ],
    floats: [],
    trxs: [
            {
                "key": "tp1",
                "value": "2019-09-08T14:30:58.000"
            }
        ]
    }
```
