## EOSJS Propose Example

This code can be used to create a ```propose.js``` file which can be used in create proposals in the test environment, and illustrates a full example of calling the ```propose``` action.

You can replace the ```data``` object below with the example data object provided in the details proposal type documentation.

```
const { Api, JsonRpc } = require("eosjs");
const { JsSignatureProvider } = require("eosjs/dist/eosjs-jssig");
const fetch = require("node-fetch");
const { TextEncoder, TextDecoder } = require("util");

const defaultPrivateKey = process.env.PRIVATE_KEY;
const signatureProvider = new JsSignatureProvider([defaultPrivateKey]);

const rpc = new JsonRpc("https://test.telos.kitchen", { fetch });

const api = new Api({
  rpc,
  signatureProvider,
  textDecoder: new TextDecoder(),
  textEncoder: new TextEncoder()
});

const sendTrx = async () => {

  try {
    const actions = [{
      account: "hyphadaomain",
      name: "propose",
      authorization: [
        {
          actor: "johnnyhypha1",
          permission: "active"
        }
      ],
      data: {
        proposer: "johnnyhypha1",
        proposal_type: "payouts",
        trx_action_name: "makepayout",
        names: [
          {
            "key": "recipient",
            "value": "johnnyhypha1"
          }
        ],
        strings: [
          {
            "key": "title",
            "value": "Payout for Beach Cleanup"
          },
          {
            "key": "description",
            "value": "we cleaned up a dirty beach"
          },
          {
            "key": "content",
            "value": "Cleaner beaches make better baskets"
          }
        ],
        assets: [
          {
            "key": "hypha_amount",
            "value": "2 HYPHA"
          },
          {
            "key": "seeds_amount",
            "value": "7.00000000 SEEDS"
          },
          {
            "key": "hvoice_amount",
            "value": "3 HVOICE"
          }
        ],
        time_points: [
          {
            "key": "contribution_date",
            "value": "2019-09-08T14:30:58.000"
          }
        ],
        ints: [],
        floats: [],
        trxs: []
      }
    }
    ]

    const result = await api
      .transact(
        {
          actions: actions
        },
        {
          blocksBehind: 3,
          expireSeconds: 30
        }
      )

    console.log("Successfull : ", result);
  } catch (e) {
    console.error(e)
    console.log('Please, fix an error and run script again')
    process.exit(1);
  }
}

const main = async () => {
  await sendTrx()
}

main()


```