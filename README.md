# AcademyCompetition

Welcome on this second plutus competition for Cardano, so I have heard many times people saying that Cardano smart contracts are centralized, that they rely on the frontend and so are not able to keep the status
let's see if this is true...how?
Let's buy an NFT from jpeg.store using cardano-cli: without frontend

##The Plutus script

In the repository you will find the plutus script of jpeg.store, how do I know is right? let's test what address we get using the script:

`cardano-cli address build \
--payment-script-file jpeg.plutus \
--mainnet \
--out-file jpg.addr

cat jpg.addr
`

We get *addr1w999n67e86jn6xal07pzxtrmqynspgx0fwmcmpua4wc6yzsxpljz3* and if we check this address in pool.pm we see it contains the NFTs listed on jpeg.store

Time to unlock one NFT, in this case we decided to go for https://www.jpg.store/asset/a706fc87764cde4ac018c38bf61630c1065932db49e6f495be3b29f8436f636f4c6f636f477265656e50616c6d53314e4654373532
An amazing CocoLoco (please understand I am ironic)
`
{"fields":[{"bytes":"918d481f35e7e0c0cc056977f4f08575af7cb172574a7d17bdd1ddf5"},{"int":100000000},{"bytes":"a706fc87764cde4ac018c38bf61630c1065932db49e6f495be3b29f8"},{"bytes":"436f636f4c6f636f477265656e50616c6d53314e4654373532"},{"bytes":"68b82dc4fb2d515501728de5bfcb1fd0a47eb3dd628d3340e10afa28"},{"int":0}],"constructor":0}
`

You may wonder how to build the datum? 
* The first key is the payment hash of the owner, in this way he is the only one to update the listing or cancel it
* The second key is the price in lovelace
* the third key is the policyID
* the fourth key is the assetname hex encoded
* the last one is the hash of the jpeg fee address

Perfect now we need an address built with cardano-cli with enough ADA to pay and the transaction code will be something like:
`
cardano-cli transaction build --alonzo-era --mainnet --required-signer-hash HASHOFYOURWALLET --tx-in TRANSACTION_HASH_WITH_ADA_TO_PAY#INDEX --tx-in 4c6e3f5f17974933e52ee422c0a8be84cd1f3c2300379eb2be8448d3cccba582#0 --tx-in-script-file jpg.plutus --tx-in-datum-file datumLoco.json --tx-in-redeemer-file redeemerJpg.json  --tx-in-collateral CollateralTx_in_yourWallet#0 --tx-out addr1q9cwvremt6n320s2e3agq0jyq82yhrk3htsu0w426xnz5us70z4w0jgvcdkkynmm8wmds66jd9kusnjfpu6raw5fqp0sr07p5w+2000000 --tx-out addr1q9catuzusthj3wkp74m47cpxvvd0v756efcyxtdrz07hd8yvn7xg0n0fnx3ut0wxcsyqh6nufwyw60y6ehlxa29842tqczpcpd+98000000  --tx-out $(cat payment.addr)+1593068+"1 a706fc87764cde4ac018c38bf61630c1065932db49e6f495be3b29f8.436f636f4c6f636f477265656e50616c6d53314e4654373532" --change-address Your_address_for_change --protocol-params-file pparams.json --out-file alonzo.tx --invalid-before BLOCK_BEFORE --invalid-hereafter BLOCK_FINAL
`

Find the redeemerJpg.json that makes everything compile succesfully and win the competition
