
## Supported Attributes
Role proposals require the following additional attributes.

Key             | Description                                               | Type      | Example Value
--------------- | ----------------------------------------------------------| --------- | ------------------
hypha_amount    | Per period payment amount in HYPHA for this role          | asset     | "50 HYPHA"
seeds_amount    | Per period payment amount in SEEDS for this role          | asset     | "45.50000000 SEEDS"
hvoice_amount   | Per period payment amount in HVOICE for this role         | asset     | "50 HVOICE"
start_period    | First period_id for which this role is active/eligible    | int       | 32
end_period      | Last period_id for which this role is active/eligible     | int       | 62


## Example EOSJS Code
The ```api.transact``` method in eosjs requires a ```data``` object.  Here is an example object for a role proposal.

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
    trxs: []
}
```
