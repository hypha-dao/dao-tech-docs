
## Step 1: Apply and Register
There are two actions that require approval from the applicant, ```regvoter``` on Telos Decide and ```apply``` on ```hyphadaomain```.

### Telos Decide ```regvoter```
To register a user for Hypha DAO, the user must sign a transaction to register.

```
cleos -u https://test.telos.kitchen push action trailservice regvoter '["hyphalondon2", "0,HVOICE", null]' -p hyphalondon2
```

### Hypha DAO Apply
The user must also approve the action call to ```apply```.

```
cleos -u https://test.telos.kitchen push action hyphadaomain apply '["hyphalondon2", "I met with Debbie at the regen conference and we talked about Hypha. I would like to join."]' -p hyphalondon2
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

cleos -u https://test.telos.kitchen push action trailservice mint '["johnnyhypha1", "1 HVOICE", "original mint"]' -p hyphadaomain
