##################################################################################
Name: Subhasis Chakraborty
Email: chsubhasis@gmail.com
B9Lab user name: chsubhasis
Essay of the final project on Regulated system of toll roads 
##################################################################################

### Objective:

 The objective is to ideate a regulated, system of toll roads. Vehicles pay to drive on them. It is regulated in the sense that there is an entity whose role it is to ensure some rules for participants. The end goal is to have all, part or nothing of such a somewhat decentralised system run on an Ethereum blockchain.

This system allows seamless toll collection process without stopping the vehicle at toll point. As an added benefits, it also enables seamless traffic management, reduce carbon footprint by reducing the vehicle queue at toll point, smart anti theft management and vehicle type classification, movement and audit.

### Why do we need a blockchain based system over here?

The manual toll collection plaza has several limitation and inherent issues. Couple of them are listed below
1. Long queue at toll gate/traffic congestion : The manual process rely on man power to collect the toll fee, process it, returns back the balance amount and then manually or electronically open the gate . This enforces a long queue at the toll point.
2. increase vehicle pollution / fuel consumption - As the long vehicle queue, there is pollution increase at the toll point which add up to over all carbon foor print.
3. Lost time - Since manual process takes time to process, its a lost opportunity time for the drivers.
4. lack of proper traffic management - Its hard to keep track of all the vehicle movements on road. Also , in case of theft or hit-run or robbery scenario, its hard to get the vehicle movement as they are not being tracked.

### Actors:

1. State : They holds the perpetual lease over toll road land.

2. Transport authority / Road Regulator : They have the comprehensive responsibilities for the planning, construction, operation and management of motorway and expressway network and also outline the policies for the Road operator to collect tolls for using this network. They sublease toll roads to road operator for the management and collection of toll fee on these roads.


In the current project, Road regulator has adopted 'distance based' tolling.

Below are couple of their responsibilities
- identify kind of vehicle and set toll rate for the same.
- Set maximum allowable toll price for the use of toll road for each type of vehicle
- management of road operator. Create or delete road operators.
- maximum toll road price updating : This can be done based on different index like Customer Price Index (CPI) or Average Weekly Earnings (EWE) or combination of both.

3. Road Operator : Road Regulator handovers the management and collection of toll road fee to Road operator under regulatory guideline. Road operator can then establish toll booths on the toll road to collect the toll fee from the motorist. Main responsibilities
- create and establish toll booth and its infrastructure.
- Define base toll fee price between toll booth. The final toll fee ( after multiplying with vehicle toll rate) must not be greater than the maximum toll fee set by the road regulator.
- If its electronic toll booth, then provides RFID enable tag and digital platform for the customer to manage their account and preferred payment mode. Customer in turn would purchase tag, create account with the Toll operator, link their preferred payment method for the road operator and stick the tag in their vehicle windscreen.
- Manage different toll fee collection scenario for the customer like ◦in case of missing tag or account balance deficit, generate pay invoice and post it to the customer address. - Levy penalty of not paying toll fee with in due time frame.
- provides an online medium to pay the toll fee.
- provides different aspect of managing individual online account such as 'Login issue', 'user name PIN and password', 'Account balance and payment', 'Account update'.

4. Toll booth : Toll booth workers collect money, disburse change, and issue receipts to motorists who use toll roads and private facilities. They also document how many people come by and watch out for toll evaders. Toll booth workers ensure that toll roads operate smoothly and efficiently.

In a cashless tolling system, toll booth is equipped with RFID tag reader, different sensors, surveillance cameras. These system work in a seamless way to detect approaching vehicle, determine its type, read the license plate, run check against possible theft, read tagId, fetch account details, check balance and block approx toll amount, manage account transaction and update the account and audit system.

### Implementation

As vehicle approaches the toll gate, IR sensor would sense it, identify the vehicle type and activates camera to capture number plate. It then runs through anti theft module to check against stolen or reported vehicle. If vehicle is stolen then information can rely to the police control centre and txt message delivered to car authorised owner. Toll gate would not allows the vehicle to pass through.

If not stolen, then RFID sensor would read the tag ID and fetch the payment mode detail. There are different payment mode available.
1. payment mode = etehreum address If payment mode is linked to ethereum address and proper balance is there then smart contract method would invokes with (tag id, car type, car registartion number, ethereum address, entry toll address) parameters and approx toll fee amount is blocked. Toll gate allows the vehicle to pass through.

if not enough balance, then these details get passed to autonomous subsystem to prepare the toll fee demand letter which then will sent to the vehicle registered owner address. Toll gate allows the vehicle to pass through.
2. payment mode = credit/debit/pre-paid card If payment mode is linked to card and proper balance is there then smart contract method would invokes with (tag id, car registration number and card details) parameter and approx toll fee amount is blocked.

if not enough balance, then these details get passed to autonomous subsystem to prepare the toll fee demand letter which then will sent to the vehicle registered owner address. Toll gate allows the vehicle to pass through.
3. If no tag attached with the vehicle Then car registration number passed to autonomous subsystem to prepare the toll fee demand letter which then will sent to the vehicle registered owner address. Toll gate allows the vehicle to pass through.

============================================================================================================

### Detail Design

Our project describes a road system that will be represent by 2 overarching smart contracts:

* `Regulator`
* `TollBoothOperator`

These other elements of the system are represented by externally owned accounts:

* owner of `Regulator`
* owner of `TollBoothOperator`
* individual vehicles
* individual toll booths

## Route price

The price will be decided by 3 variables:

* the entry toll booth
* the exit toll booth
* the vehicle type

There variables will be used thus:

* the `TollBoothOperator` defines a base route price from booth A to booth B. The base route price from booth B to booth A maybe non-existent, equal or different.
* the `TollBoothOperator` defines a multiplier for each vehicle type. This is the number by which the base route price is multiplied with to get a specific vehicles route price or deposit.

## External accounts

There are a certain number of off-chain actions, such as proof of identity or secret exchange, that we will address only with a wave of the hand in this project. Smart contracts should make these actions possible via public functions, though.

## Vehicles

We only care about vehicles, not about drivers. It means it can be driven by the vehicle's rightful owner, another driver, or no human driver at all.

* Only vehicles registered with the regulator are allowed to enter the road system.
* Before entering the road system, registered vehicles must make a deposit of at least the amount required by the operator for their vehicle type.
* Upon exit, the exit toll booth will trigger the payment off the deposit and refund the difference to the vehicle.

* This deposit must be accompanied by the address of the toll booth by which they will enter.
* When exiting the road system, the vehicle gives, off-chain, a secret to the exit toll booth.
* The exit toll booth sends this secret to the road operator contract, which is used to unlock the deposit, then pay the road operator the proper fee for the route taken, then refunds the difference to the vehicle.

The off-chain exchange of the secret is handled by a wave of the hand in this project. You are not asked to work on this massive "detail".

## Toll booths

* When faced with a vehicle that wants to enter the road system, a toll booth will confirm, off-chain, the address of the incoming vehicle.
* If the vehicle has made the proper deposit for its type, the booth opens the gate. This part will be simulated in tests.

Here too, you need not work on the off-chain exchange of address of the incoming vehicles.

## Owner of `Regulator`

This account will:

* update the vehicle types in the smart contract.
* ask the smart contract to deploy new `TollBoothOperator`s.
* inform whether an address is a registered `TollBoothOperator`.
* unregister `ToolBoothOperator`s when needed.

## Owner of `TollBoothOperator`

This account will:

* update the base price of routes for all toll booth pairs it sees fit.
* update the multiplier of all vehicle types it sees fit.

In particular:

* if a vehicle just completed a route for which there is no base price, the owner should put a price to it before it can unlock the deposit.
* if a vehicle type has no multiplier, then the road system is closed to this vehicle type.

We will simplify the system by overlooking the fact that the owner may disallow a vehicle type _after_ such a vehicle has entered the system.

## Smart contracts

## `Regulator`

This contract does 2 main things (studiously avoiding the word _functions_ here):

* keep track of the vehicle type for each vehicle address
* deploy new `TollBoothOperator`

This ensures that:

* because the regulator deploys the toll booth operator, road users are confident they are exchanging with a vetted smart contract.
* an address that has no vehicle type is not registered and should not be allowed onto the road system.

Additionally:

* the regulator collects no fees.
* a type of `0` denotes an unregistered vehicle.
* for mnemonics only, you can assign type `1` for motorbikes, `2` for cars and `3` for lorries.

## `TollBoothOperator`

This contract has many functions which have been parcelled out to its inheritance tree, see its interfaces. The things it does:

* can be paused / resumed to pause the vehicle-facing operations.
* keeps track of the regulator.
* keeps track of the base deposit.
* keeps track of the multipliers of vehicle types.
* keeps track of the toll booths under its purview.
* keeps track of the route base prices.
* accepts deposits from vehicles.
* accepts exit calls from toll booths.
* accepts messages to clear the exit backlog.

For simplicity's sake:

* there is only 1 (or 0) toll booth at a given kilometre on the road. So one booth may have as many gates as there are lanes on the road.
* a base route price of `0` denotes an absence of information.
* the entry and exit booths of a route cannot be the same address.
* the `TollBoothOperator` has total control over the deposit, the base route prices, and the multipliers.
* there is no congestion or pollution charges.
* the latest route base price is the valid route base price, even if the road operator changed the price after the vehicle entered the road system.

## Fee mechanics

For a vehicle to be accepted on the road system and the operator to be paid at the end of the route, a little dance is engineered:

* before entering the system, the vehicle deposits the required amount into the `TollBoothOperator`, and passes along the address of the entry booth and a unique hashed secret of its own choice. The vehicle keeps the secret until the end of the trip.
* when faced with the toll booth it mentioned when depositing, it proves its identity off-chain (we talk about this identity proof but you need not implement it), after which the booth opens the gate.
* when exiting the road system at a booth, the vehicle gives the exit toll booth its unhashed secret off-chain. Again, we talk about this off-chain exchange but you do not need to implement it.
* the exit toll booth then submits this secret to the `TollBoothOperator`, which unlocks the deposit for payment and refund of the difference, if applicable.
* if the fee is equal to or higher than the deposit, then the whole deposit is used and no more is asked of the vehicle, now or before any future trip.
* if the fee is smaller than the deposit, then the difference is returned to the vehicle.
* if the fee is not known at the time of exit, the pending payment is recorded, and "base route price required" event is emitted and listened to by the operator's oracle.
* when the oracle receives a new base route price request, it submits the base fee, which also clears one pending payment.
* if there are more than 1 pending payments, an additional function is there to progressively clear the backlog a set number of pending payments at a time in a FIFO (of the exit) manner.

For simplicity's sake, we have not implemented a deadline after which the deposit is returned to the vehicle. Also, the function to clear pending more than 1 payment is an ugly one, because it implies a loop and multiple transfers. We actually use it to see how many pending payments you can cram in a single transaction.

## Contract overview

All interfaces are found in the `contracts/interfaces/` folder and are suffixed with `I`, a capital `i`. Here is the list of them:

* `OwnedI`, which keeps track of the owner of a contract.
* `PausableI`, which keeps track of the _paused_ status of a contract.
* `RegulatorI`, which describes methods required of the regulator.
* `RegulatedI`, which keeps track of who the regulator is.
* `MultiplierHolderI`, which keeps track of the multipliers to attach to vehicle types.
* `DepositHolderI`, which keeps track of the base deposit required of vehicles.
* `TollBoothHolderI`, which keeps track of addresses that represent toll booths in the system.
* `RoutePriceHolderI`, which keeps track of the base route prices between entry and exit toll booths.
* `TollBoothOperatorI`, which describes methods required of the toll booth operator for interaction with vehicles.

The contracts you need to create in their individual files in the `contracts/` folder are as follows. Note that we will use your implementations against our battery of Truffle unit tests. You can see a sample of them in the test folder. So it is important that you stick to the naming and the parameters order.

When we say that `One` inherits from `OneI` and `Two` inherits from both `OneI` and `TwoI`, we leave it to your best judgement as to how to use `One` when coding `Two`. Remember that inheritance is transitive.

## `Owned`

It extends:

* `OwnedI`

and has:

* a modifier named `fromOwner` that rolls back the transaction if the transaction sender is not the owner.
* a constructor that takes no parameter.

## `Pausable`

It extends:

* `OwnedI`
* `PausableI`

and has:

* a modifier named `whenPaused` that rolls back the transaction if the contract is in the `false` paused state.
* a modifier named `whenNotPaused` that rolls back the transaction if the contract is in the `true` paused state
* a constructor that takes one `bool` parameter, the initial paused state.

## `Regulator`

It extends:

* `OwnedI`
* `RegulatorI`

and has:

* a constructor that takes no parameter.

### `Regulated`

It extends:

* `RegulatedI`

and has:

* a constructor that takes one `address` parameter, the initial regulator; it should roll back the transaction if the regulator argument is `0`.

### `MultiplierHolder`

It extends:

* `OwnedI`
* `MultiplierHolderI`

and has:

* a constructor that takes no parameter.

## `DepositHolder`

It extends:

* `OwnedI`
* `DepositHolderI`

and has:

* a constructor that takes one `uint` parameter, the initial deposit wei value; it should roll back the transaction if the deposit argument is `0`.

## `TollBoothHolder`

It extends:

* `OwnedI`
* `TollBoothHolderI`

and has:

* a constructor that takes no parameter.

## `RoutePriceHolder`

It extends:

* `OwnedI`
* `TollBoothHolderI`
* `RoutePriceHolderI`

and has:

* a constructor that takes no parameter.

## `TollBoothOperator`

It extends:

* `OwnedI`
* `PausableI`
* `RegulatedI`
* `MultiplierHolderI`
* `DepositHolderI`
* `TollBoothHolderI`
* `RoutePriceHolderI`
* `TollBoothOperatorI`

and has:

* a constructor that takes in this order:
  * one `bool` parameter, the initial paused state.
  * one `uint` parameter, the initial deposit wei value; it should roll back the transaction if the deposit argument is `0`.
  * one `address` parameter, the initial regulator; it should roll back the transaction if the regulator argument is `0`.
* a fallback function that rejects all incoming calls.

## Migrations

You need to create one migration script `2_...js` that will:

* deploy a regulator,
* then call `createNewOperator` on it.
* then resumes the newly created operator, which should be paused before the resume step.

## Tests

A quick note on wording:

* "a / the deposit" refers to the value that the contract requires from entering vehicles.
* "what is deposited" means what was actually sent by the vehicle upon entering.

You may create as many tests as you want but we still want you to create tests for the following scenarios. 

* Scenario 1:
  * `vehicle1` enters at `booth1` and deposits required amount (say 10).
  * `vehicle1` exits at `booth2`, which route price happens to equal the deposit amount (so 10).
  * `vehicle1` gets no refund.
* Scenario 2:
  * `vehicle1` enters at `booth1` and deposits required amount (say 10).
  * `vehicle1` exits at `booth2`, which route price happens to be more than the deposit amount (say 15).
  * `vehicle1` gets no refund.
* Scenario 3:
  * `vehicle1` enters at `booth1` and deposits required amount (say 10).
  * `vehicle1` exits at `booth2`, which route price happens to be less than the deposit amount (say 6).
  * `vehicle1` gets refunded the difference (so 4).
* Scenario 4:
  * `vehicle1` enters at `booth1` and deposits (say 14) more than the required amount (say 10).
  * `vehicle1` exits at `booth2`, which route price happens to equal the deposit amount (so 10).
  * `vehicle1` gets refunded the difference (so 4).
* Scenario 5:
  * `vehicle1` enters at `booth1` and deposits (say 14) more than the required amount (say 10).
  * `vehicle1` exits at `booth2`, which route price happens to be unknown.
  * the operator's owner updates the route price, which happens to be less than the deposited amount (say 11).
  * `vehicle1` gets refunded the difference (so 3).
* Scenario 6:
  * `vehicle1` enters at `booth1` and deposits more (say 14) than the required amount (say 10).
  * `vehicle1` exits at `booth2`, which route price happens to be unknown.
  * `vehicle2` enters at `booth1` and deposits the exact required amount (so 10).
  * `vehicle2` exits at `booth2`, which route price happens to be unknown.
  * the operator's owner updates the route price, which happens to be less than the required deposit (so 6).
  * `vehicle1` gets refunded the difference (so 8).
  * someone (anyone) calls to clear one pending payment.
  * `vehicle2` gets refunded the difference (so 4).

We will run your tests against our hopefully-correct implementations, for which we expect your tests to pass. We will also run your tests against our purposefully-incorrect implementations, for which we expect your tests to fail.

## GUI

We want you to create a simple UI with 4 pages. You do not need to make it look fancy. But it has to be functional. It is ok if it requires copy / paste of addresses and secrets from the human user.

You can use the framework of your choice, and it is good manner to add some instructions on how to make it run within the VM. If there is any NodeJs package that you would like to use make sure they appear in `package.json` and you committed it too.

So:

* a page for the deployed `Regulator`'s owner, which allows it to:
  * set vehicle types.
  * create a new `TollBoothOperator`.
  * no need to make it possible to change the regulator on the individual `TollBoothOperator`s.
* a page for the `TollBoothOperator`'s owner, which allows it to:
  * add toll booths.
  * add base route prices.
  * set multipliers.
  * no need to make it possible to change owners of the contract.
  * no need to make it possible to pause the contract.
  * no need to make it possible to remove toll booths.
  * no need to make it possible to change the required deposit.
  * no need to make it possible to clear one pending payment.
* a page for individual vehicles, which allows it to:
  * see its basic Ether balance.
  * make an entry deposit.
  * see its history of entry / exit.
  * no need to see its pending payments.
* a page for individual toll booths, which allows it to:
  * report a vehicle exit.
  * be informed on the status of the refund or of the pending payment of the vehicle reported above.
  * no need to see its history of entry / exit.