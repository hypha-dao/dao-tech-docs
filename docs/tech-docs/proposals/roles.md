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
    proposal_type: "roles", 
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

## Full Lifecycle

### Step 1: Propose a Role
See [propose.js](eosjs-propose.md) for exact eosjs code. Copy the data object above to the ```propose.js``` file and run ```node propose.js```

> You may need to run ```export PRIVATE_KEY=<insert key>```, and insert the key from the [test-net.md](Testnet Page)

### Step 2: Query the Proposal to get ballot_id

Once the proposal is created, it must be queried to find the Telos Decide ```ballot_id```.

You can query for the most recently created proposal using this query. 

```
cleos -u https://test.telos.kitchen get table -l 1 --index 2 --key-type i64 -r hyphadaomain roles proposals
```

Alternatively, using EOSJS, you can query using this:
```
async function printLastCreatedProposal (proposalType) {
  let rpc;
  let options = {};

  rpc = new JsonRpc("https://test.telos.kitchen", { fetch });
  options.code = "hyphadaomain";
  options.scope = proposalType; 

  options.json = true;
  options.table = "proposals";
  options.index_position = 2; // index #2 is "bycreated"
  options.key_type = 'i64';
  options.reverse = true;
  options.limit = 1;
  
  rpc.get_table_rows(options).then( result => {
    if (result.rows.length > 0) {
      console.log (JSON.stringify(result.rows, null, 2));
    } else {
      console.log ("There are no proposals of type: ", proposalType);
    }
  })
}
```
See [Proposal Queries](eosjs-queries.md) for a complete node script

The ```ballot_id``` attribute, which was system-generated when the proposal was created, is used to interact directly with Trail Decide. 

It will be saved in the ```names``` map.
```
"names": [
    {
        "key": "ballot_id",
        "value": "hypha1.....23"
    },
```


### Step 3: Vote for the Proposal
To vote for a proposal, use the ```castvote``` action on ```trailservice```.

```
cleos -u https://test.telos.kitchen push action trailservice castvote '["haydenhypha1", "hypha1.....1d", ["pass"]]' -p haydenhypha1
```

### Step 4: Close Proposal to Create the Role
To close the proposal, call the ```closeprop``` action.  If the proposal has enough votes, this action will create the role.

```
cleos -u https://test.telos.kitchen push action hyphadaomain closeprop '["roles", 26]' -p haydenhypha1
```