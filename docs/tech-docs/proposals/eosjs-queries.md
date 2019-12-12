## Query Examples

Below are two examples of how to query the proposals table from EOSJS.

```
const { JsonRpc } = require("eosjs");
const fetch = require("node-fetch"); 

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

async function printByProposer (proposalType, proposer) {
  let rpc;
  let options = {};

  rpc = new JsonRpc("https://test.telos.kitchen", { fetch });
  options.code = "hyphadaomain";
  options.scope = proposalType;

  options.json = true;
  options.table = "proposals";
  options.index_position = 4; // index #5 is "byproposer"
  options.key_type = 'i64';
  options.lower_bound = proposer;
  options.upper_bound = proposer;

  rpc.get_table_rows(options).then( result => {
    if (result.rows.length > 0) {
      console.log (JSON.stringify(result.rows, null, 2));
    } else {
      console.log ("There are no proposals for that type and proposer");
    }
  })
}

async function main () {
  try {

    // Print the last created role proposal
    await printLastCreatedProposal ("roles");

    // Print the role proposals proposed by this account
    await printByProposer ("roles", "johnnyhypha1");

  } catch (e) {
    console.log (e);
  }
}

main ();
```