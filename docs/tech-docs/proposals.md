## Creating a Proposal
The ```propose``` action is used to submit a proposal to Hypha. 

> The ```proposals``` table within the Hypha DAO contract uses the Telos Kitchen Sink design pattern, named such because it can store nearly any arbitrary data and it can adapt without upgrading the underlying EOSIO table structure.  ["Everything but the kitchen sink"](https://idioms.thefreedictionary.com/everything+but+the+kitchen+sink)

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

### Proposal Types
Hypha DAO *currently* supports *three* built in proposal types.

The ```proposal_type``` is an EOSIO ```name``` value. NOTE: proposal types are plural by practice.

Proposal Type   | Used For                                                   | Link
--------------- | ------------------------------------------------------------- | ------
roles           | Propose Creation of a New DAO Role                 | [View Details](proposals/roles.md)
assignments     | Propose Assignment of a Person to an Existing Role | [View Details](proposals/assignments.md)
payouts         | Propose a One-time Payout                 | [View Details](proposals/payouts.md)

### Required Values
Certain values are required to be within the ```map``` parameters for certain proposal types.   All proposals support/require ```title```, ```description```, and ```content```.  

Key         | Description                                                   | Type      | Example Value
----------- | ------------------------------------------------------------- | --------- | ------------------
title       | The title of the role, assignment, or payout                  | string    | "Community Manager"
description | A text-only field for summary info not suitable for the title | string    | "Energizing and engaging people to..."
content     | Any general purpose content field for text or markdown        | string    | "Full text of the role and additional materials..."


## Query a Proposal
To query for a proposal, the ```proposal_type``` is the scope and the primary key is the ```id```.  There are also indexes for ```proposer```, ```created_date```, and ```updated_date```.

You can query for the most recently created proposal using this query with ```cleos```.
```
cleos -u https://test.telos.kitchen get table -l 1 --index 2 --key-type i64 -r hyphadaomain roles proposals
```

Alternatively, you can query using eosjs. The below example, just as above, prints the most recently created roles proposal.
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
See [Proposal Queries](proposals/eosjs-queries.md) for a complete node script


## Voting on a Proposal
To vote for a proposal, use the ```castvote``` action on ```trailservice```.

```
cleos -u https://test.telos.kitchen push action trailservice castvote '["haydenhypha1", "hypha1.....1d", ["pass"]]' -p haydenhypha1
```

## Closing a Proposal
To close the proposal, call the ```closeprop``` action.  If the proposal has enough votes, this action will invoke the ```trx_action_name``` within the proposal and pass the ```id``` attribute.  At that point, the code within ```trx_action_name``` will be able to query and access all of the data stored within the ```proposal``` object. See example below. 

```
cleos -u https://test.telos.kitchen push action hyphadaomain closeprop '["roles", 26]' -p haydenhypha1
```

### Example ```newrole``` Action
When a role proposal type is closed, and it has enough votes to pass, it triggers the ```newrole``` action.  As you can see below, the ```newrole``` requires that authority of ```get_self()```, which will only be granted if the proposal was given enough votes to pass.  

And as you can see, if it passes, the new role is created within the ```holocracy``` class.

```
void hyphadao::newrole (const uint64_t& proposal_id) {

   	require_auth (get_self());

	name proposal_type { "roles" };
	proposal_table p_t (get_self(), proposal_type.value);
	auto p_itr = p_t.find (proposal_id);
	check (p_itr != p_t.end(), "Proposal Type: " + proposal_type.to_string() + "; ID: " + std::to_string(proposal_id) + " does not exist.");

	holocracy.newrole (p_itr->strings.at("title"),
						p_itr->strings.at("description"),
						p_itr->strings.at("content"),
						p_itr->assets.at("hypha_amount"),
						p_itr->assets.at("seeds_amount"),
						p_itr->assets.at("hvoice_amount"),
						p_itr->ints.at("start_period"),
						p_itr->ints.at("end_period"));
}
```