## Supported Attributes
Assignment proposals require the following additional attributes.

Key                 | Description                                                           | Type      | Example Value
------------------- | ----------------------------------------------------------------------| --------- | ------------------
recipient    | The account to receieve the payment if approved         | name     | samanthahyph
role_id    | The ```role_id``` primary key from the ```roles``` table          | int     | 43
time_share  | The percentage of time (from 0-1) dedicated to this role  | float | 0.50


## Example EOSJS Code
The ```api.transact``` method in eosjs requires a ```data``` object.  Here is an example object for a payout proposal.

```
data: {
    proposer: "samanthahyph",
    proposal_type: "assignments", 
    trx_action_name: "assign",
    names: [
        {
            "key":"assigned_account",
            "value":"samanthahyph"
        }
    ],
    strings: [
            {
                "key": "title",
                "value": "Underwater Basketweaver - Carribean"
            },
            {
                "key": "description",
                "value": "Weave baskets at the bottom of the sea - Carribean Islands and surrounding area"
            },
            {
                "key": "content",
                "value": "The unique seaweed of the caribbean makes strong baskets."
            }
        ], 
    assets: [],
    time_points: [],
    ints: [
            {
                "key": "start_period",
                "value": "41"
            },
            {
                "key": "end_period",
                "value": "51"
            },
            {
                "key": "role_id",
                "value": "0"                        
            }
        ],
    floats: [
        {
            "key": "time_share",
            "value": "0.500000000000"
        }
    ],
    trxs: []
  }
```
