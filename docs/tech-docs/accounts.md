Account and balance information can list in a few places. 

## Check a User's HVOICE balance
HVOICE is the token registered with Telos Decide. You can query the staked, liquid, and delegated balances. If the user ```johnnyhypha1``` were registered in multiple treasuries, those would show here. 


```JavaScript tab=

  let rpc = new JsonRpc(host || "https://test.telos.kitchen", { fetch });
  let options = {
      code: "trailservice",
      scope: "johnnyhypha1",
      table: "voters"
  };
  
  console.log (await rpc.get_table_rows(options));
```

```bash tab=
cleos -u https://test.telos.kitchen get table trailservice johnnyhypha1 voters
```

Result
``` json
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

## Check a User's HYPHA balance
```JavaScript tab=

  let rpc = new JsonRpc(host || "https://test.telos.kitchen", { fetch });
  let options = {
      code: "hyphatokens1",
      scope: "johnnyhypha1",
      table: "accounts"
  };
  
  console.log (await rpc.get_table_rows(options));
```

```bash tab=
cleos -u https://test.telos.kitchen get table hyphatokens1 johnnyhypha1 accounts
```

Result
``` json
{
  "rows": [{
      "balance": "21 HYPHA"
    }
  ],
  "more": false
}
```