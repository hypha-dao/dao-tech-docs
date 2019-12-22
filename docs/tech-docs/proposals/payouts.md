## Supported Attributes
Payout proposals require the following additional attributes.

Key                 | Description                                                           | Type      | Example Value
------------------- | ----------------------------------------------------------------------| --------- | ------------------
recipient    | The account that will receive the payout if approved      | name     | jameshypha11
hypha_amount    | One time HYPHA payment         | asset     | "50 HYPHA"
seeds_amount    | One time SEEDS payment          | asset     | "45.50000000 SEEDS"
hvoice_amount   | One time HVOICE payment       | asset     | "50 HVOICE"
contribution_date  | The date that the corresponding contribution (funds, labor, etc) was provided  | time_point | 2019-09-08T14:30:58.000

## Example JSON Payout Proposal
Here's an example payout proposal. See here for live file: https://gitlab.com/hypha/hypha-dao-smart-contracts/blob/migration/tests/proposals/payout.json
``` JSON
{
  "data": {
    "names": [
      {
        "key": "proposer",
        "value": "johnnyhypha1"
      },
      {
        "key": "recipient",
        "value": "johnnyhypha1"
      },
      {
        "key": "proposal_type",
        "value": "payouts"
      },
      {
        "key": "trx_action_name",
        "value": "makepayout"
      }
    ],
    "strings": [
      {
        "key": "title",
        "value": "Payout for Beach Cleanup"
      },
      {
        "key": "description",
        "value": "We cleaned up a dirty beach"
      },
      {
        "key": "content",
        "value": "Cleaner beaches make better baskets"
      }
    ],
    "assets": [
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
    "time_points": [
      {
        "key": "contribution_date",
        "value": "2019-09-08T14:30:58.000"
      }
    ],
    "ints": [],
    "floats": [],
    "trxs": []
  }
}
```
