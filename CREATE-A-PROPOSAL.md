## GenesisX Governance powered by the blockchain

The GenesisX Community Governance will allow anyone with a GenesisX wallet to submit project proposals for integrations, promoting, new exchanges, marketing, dev, etc and get the requested XGS coins if more than a net of 10% of MasterNode owners choose to support it with **Yes** votes.
### Create a proposal

All the commands assume a CLI wallet.

You can run them in the debug console of a Qt wallet as well, just remove the `$ genesisx-cli ` part from the commands.

### Identify the next budget superblock to pin the proposal to:

```
$ genesisx-cli getnextsuperblock
259200
```

### Create an address in the wallet that will receive the funds if the proposal is voted Yes by the MasterNodes:
```
$ genesisx-cli getaccountaddress "proposal1"
GWqEhP6R7veHPjW3Crdm3cz8dHf4HQ7WMB
```

### Create the proposal using the superblock and address from above. 

In this example we are asking for 2000 XGS coins. Min amount that can be requested is `10`. Value `1` after the URL indicates that this is a one-off proposal, targeted to be paid on block 259200 if more than 10% net masternodes support it. Proposal name, `test1` in this example, is limited at 20 characters and URL at 64. The URL is there to provide additional details about the proposal so that MN owners can decide if they should vote it yes or no.
```
$ genesisx-cli preparebudget "test1" "http://forum.example.com/proposal1" 1 259200 "GWqEhP6R7veHPjW3Crdm3cz8dHf4HQ7WMB" 2000
2e21cae5c69209d648734653e3dcca85c67ac96e08a48d99923de58e9ba222d3
```

Use the help subcommand if you want more details about each parameter:
```
genesisx-cli help preparebudget
```

### Wait for the 50 XGS proposal fee transaction to gain 6 confirmations
```
$ genesisx-cli gettransaction 2e21cae5c69209d648734653e3dcca85c67ac96e08a48d99923de58e9ba222d3
```

### Once we have 6 confirmations for the proposal fee, we can submit the proposal
```
$ genesisx-cli submitbudget "test1" "http://forum.example.com/proposal1" 1 259200 "GWqEhP6R7veHPjW3Crdm3cz8dHf4HQ7WMB" 2000 "2e21cae5c69209d648734653e3dcca85c67ac96e08a48d99923de58e9ba222d3"
```

### See all proposals for this budget cycle
```json
$ genesisx-cli getbudgetinfo
[
    {
        "Name" : "test1",
        "URL" : "http://forum.example.com/proposal1",
        "Hash" : "1c4c744a3b684b535b0eafe30a9c366e189554a53e9c76b542b9cbba8887f886",
        "FeeHash" : "2e21cae5c69209d648734653e3dcca85c67ac96e08a48d99923de58e9ba222d3",
        "BlockStart" : 259200,
        "BlockEnd" : 302400,
        "TotalPaymentCount" : 1,
        "RemainingPaymentCount" : 1,
        "PaymentAddress" : "GWqEhP6R7veHPjW3Crdm3cz8dHf4HQ7WMB",
        "Ratio" : 0.00000000,
        "Yeas" : 0,
        "Nays" : 0,
        "Abstains" : 0,
        "TotalPayment" : 2000.00000000,
        "MonthlyPayment" : 2000.00000000,
        "IsEstablished" : true,
        "IsValid" : true,
        "IsValidReason" : "",
        "fValid" : true
    }
]
```

### Vote yes on the above proposal from the masternode CLI
```
$ genesisx-cli mnbudgetvote local "1c4c744a3b684b535b0eafe30a9c366e189554a53e9c76b542b9cbba8887f886" yes
```

### Alternatively you can vote from the Cold wallet as well on behalf of multiple masternodes
```
$ genesisx-cli mnbudgetvote many "1c4c744a3b684b535b0eafe30a9c366e189554a53e9c76b542b9cbba8887f886" yes
```
