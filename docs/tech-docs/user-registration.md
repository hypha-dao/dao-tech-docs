

## User Registration

To register a user for Hypha DAO, the user must sign a transaction to register, and the ```hyphdaomain``` account must approve the mint action to actually issue the token.

```
cleos -u https://test.telos.kitchen push action trailservice regvoter '["johnnyhypha1", "0,HVOICE", null]' -p johnnyhypha1
cleos -u https://test.telos.kitchen push action trailservice mint '["johnnyhypha1", "1 HVOICE", "original mint"]' -p hyphadaomain
```

### Check HVOICE Balance
Check HVOICE balance using the voters table, scoped by the voter's account name.
```
cleos -u https://test.telos.kitchen get table trailservice johnnyhypha1 voters
{
  "rows": [{
      "liquid": "1 HVOICE",
      "staked": "0 HVOICE",
      "staked_time": "2019-12-10T18:05:41",
      "delegated": "0 HVOICE",
      "delegated_to": "",
      "delegation_time": "2019-12-10T18:05:41"
    }
  ],
  "more": false
}
```
