* `interface` [OwnerSupport](#interface-ownersupport)    
    defines a owner for a device    
    
* `interface` [MetaSupport](#interface-metasupport)    
    returns metadata for a device    
    
* `interface` [RentingSupport](#interface-rentingsupport)    
    defines a contract able to rent and return    
    
* `interface` [AccessSupport](#interface-accesssupport)    
    control the access for a device.    
    
* `interface` [RemoteFeatureSupport](#interface-remotefeaturesupport)    
    support controlling acces for only identifieable users, which can be based on keybase or other whitleists    
    
* `interface` [TimeRangeSupport](#interface-timerangesupport)    
    supports checks for a minimum and maximum time to rent    
    
* `interface` [OffChainSupport](#interface-offchainsupport)    
    defines Rules, which can be verified, even if the state is held on chain.    
    
* `interface` [RentForSupport](#interface-rentforsupport)    
    renting from a different account then the controller    
    
* `interface` [DepositSupport](#interface-depositsupport)    
    supports saving a deposit and returning it afterwards    
    
* `interface` [BlackListSupport](#interface-blacklistsupport)    
    defines a blacklist for users to be rejected    
    
* `interface` [DependendAccessSupport](#interface-dependendaccesssupport)    
    supports acces control for depending devices.    
    
* `interface` [DiscountSupport](#interface-discountsupport)    
    support controlling acces for only identifieable users, which can be based on keybase or other whitleists    
    
* `interface` [GroupedSupport](#interface-groupedsupport)    
* `interface` [IdentitySupport](#interface-identitysupport)    
    support controlling acces for only identifieable users, which can be based on keybase or other whitleists    
    
* `class` [MultisigSupport](#class-multisigsupport)    
    support mutlisigs as controller of a device. In this case all keyholders have access when rented.    
    
* `interface` [RemoteFeatureProvider](#interface-remotefeatureprovider)    
    interface for a remote feature-contract    
    
* `interface` [StateChannelSupport](#interface-statechannelsupport)    
    support  StateChannels    
    
* `interface` [TokenSupport](#interface-tokensupport)    
    supports exchanges between different tokens    
    
* `interface` [VerifiedDeviceSupport](#interface-verifieddevicesupport)    
    supports for signing messages by the device.    
    
* `interface` [VerifiedDeviceSupportPerContractImpl](#interface-verifieddevicesupportpercontractimpl)    
    the Implementation for a whitelist for all devices in this contract.    
    
* `interface` [VerifiedDeviceSupportPerDeviceImpl](#interface-verifieddevicesupportperdeviceimpl)    
    supports for signing messages by the device with one signature per device.    
    
* `interface` [WeekCalendarSupport](#interface-weekcalendarsupport)    
    supports controlling acces by setting opening times per weekday    
    
* `interface` [WhitelistSupport](#interface-whitelistsupport)    
    Defines additional access for certain users, (like the cleaning lady in an appartment)    
    
## `interface` OwnerSupport

defines a owner for a device
See [features/OwnerSupport.sol](https://github.com/slockit/usn-mvp/blob/develop/contracts/features/OwnerSupport.sol)

```javascript
[{"constant":true,"inputs":[{"name":"id","type":"bytes32"}],"name":"deviceOwner","outputs":[{"name":"","type":"address"}],"payable":false,"stateMutability":"view","type":"function"}]
```

* **constructor** ()

* function **deviceOwner** (`bytes32 id`)     
    constant returns (`address`)    
    the owner of the device
    * id : the deviceid 

## `interface` MetaSupport

returns metadata for a device
See [features/MetaSupport.sol](https://github.com/slockit/usn-mvp/blob/develop/contracts/features/MetaSupport.sol)

```javascript
[{"constant":true,"inputs":[{"name":"id","type":"bytes32"}],"name":"meta","outputs":[{"name":"","type":"bytes"}],"payable":false,"stateMutability":"view","type":"function"},{"constant":true,"inputs":[{"name":"id","type":"bytes32"}],"name":"properties","outputs":[{"name":"props","type":"uint128"},{"name":"extra","type":"uint64"},{"name":"subnode","type":"bytes32"}],"payable":false,"stateMutability":"view","type":"function"},{"anonymous":false,"inputs":[{"indexed":true,"name":"fixFilter","type":"bytes32"},{"indexed":true,"name":"id","type":"bytes32"},{"indexed":false,"name":"endId","type":"bytes32"}],"name":"LogDeviceChanged","type":"event"}]
```

* **constructor** ()
* event **LogDeviceChanged** (`bytes32indexed fixFilter`, `bytes32indexed id`, `bytes32 endId`)     
    this event will be triggered for each object changed. In case we have endId, we can use the same values for all ids between the id and endId. the fix-filter is just the same id so we can create a bloom-filter to detect all changes.

* function **meta** (`bytes32 id`)     
    constant returns (`bytes`)    
    created after the rent-function was executed the link to the metadata, which may also support ipfs:///...
    * id : the deviceid 
* function **properties** (`bytes32 id`)     
    constant returns (`uint128 props`, `uint64 extra`, `bytes32 subnode`)    
    the properties or behavior defined per device, which is a bitmask with well defined values. (See https://github.com/slockit/usn-mvp/wiki/Types#deviceproperties)
    * id : the deviceid 

## `interface` RentingSupport

defines a contract able to rent and return
See [features/RentingSupport.sol](https://github.com/slockit/usn-mvp/blob/develop/contracts/features/RentingSupport.sol)

```javascript
[{"constant":true,"inputs":[{"name":"id","type":"bytes32"},{"name":"index","type":"uint256"}],"name":"getState","outputs":[{"name":"controller","type":"address"},{"name":"rentedFrom","type":"uint64"},{"name":"rentedUntil","type":"uint64"},{"name":"properties","type":"uint128"}],"payable":false,"stateMutability":"view","type":"function"},{"constant":true,"inputs":[{"name":"id","type":"bytes32"}],"name":"getStateCount","outputs":[{"name":"","type":"uint256"}],"payable":false,"stateMutability":"view","type":"function"},{"constant":false,"inputs":[{"name":"id","type":"bytes32"},{"name":"secondsToRent","type":"uint32"},{"name":"token","type":"address"}],"name":"rent","outputs":[],"payable":true,"stateMutability":"payable","type":"function"},{"constant":true,"inputs":[{"name":"id","type":"bytes32"},{"name":"user","type":"address"}],"name":"getRentingState","outputs":[{"name":"rentable","type":"bool"},{"name":"free","type":"bool"},{"name":"open","type":"bool"},{"name":"controller","type":"address"},{"name":"rentedUntil","type":"uint64"},{"name":"rentedFrom","type":"uint64"},{"name":"props","type":"uint128"}],"payable":false,"stateMutability":"view","type":"function"},{"constant":false,"inputs":[{"name":"id","type":"bytes32"}],"name":"returnObject","outputs":[],"payable":false,"stateMutability":"nonpayable","type":"function"},{"constant":true,"inputs":[{"name":"id","type":"bytes32"}],"name":"supportedTokens","outputs":[{"name":"addresses","type":"address[]"}],"payable":false,"stateMutability":"view","type":"function"},{"constant":true,"inputs":[{"name":"id","type":"bytes32"},{"name":"token","type":"address"}],"name":"tokenReceiver","outputs":[{"name":"","type":"bytes32"}],"payable":false,"stateMutability":"view","type":"function"},{"constant":true,"inputs":[{"name":"id","type":"bytes32"},{"name":"user","type":"address"},{"name":"secondsToRent","type":"uint32"},{"name":"token","type":"address"}],"name":"price","outputs":[{"name":"","type":"uint128"}],"payable":false,"stateMutability":"view","type":"function"},{"anonymous":false,"inputs":[{"indexed":true,"name":"fixFilter","type":"bytes32"},{"indexed":true,"name":"id","type":"bytes32"},{"indexed":false,"name":"controller","type":"address"},{"indexed":false,"name":"rentedFrom","type":"uint64"},{"indexed":false,"name":"rentedUntil","type":"uint64"},{"indexed":false,"name":"noReturn","type":"bool"},{"indexed":false,"name":"amount","type":"uint128"},{"indexed":false,"name":"token","type":"address"},{"indexed":false,"name":"properties","type":"uint128"}],"name":"LogRented","type":"event"},{"anonymous":false,"inputs":[{"indexed":true,"name":"fixFilter","type":"bytes32"},{"indexed":true,"name":"id","type":"bytes32"},{"indexed":false,"name":"controller","type":"address"},{"indexed":false,"name":"rentedFrom","type":"uint64"},{"indexed":false,"name":"rentedUntil","type":"uint64"},{"indexed":false,"name":"paidBack","type":"uint128"}],"name":"LogReturned","type":"event"}]
```

* **constructor** ()
* event **LogRented** (`bytes32indexed fixFilter`, `bytes32indexed id`, `address controller`, `uint64 rentedFrom`, `uint64 rentedUntil`, `bool noReturn`, `uint128 amount`, `address token`, `uint128 properties`)     
    created after the rent-function was executed
* event **LogReturned** (`bytes32indexed fixFilter`, `bytes32indexed id`, `address controller`, `uint64 rentedFrom`, `uint64 rentedUntil`, `uint128 paidBack`)     
    whenever the device was returned.

* function **getRentingState** (`bytes32 id`, `address user`)     
    constant returns (`bool rentable`, `bool free`, `bool open`, `address controller`, `uint64 rentedUntil`, `uint64 rentedFrom`, `uint128 props`)    
    returns the current renting state
    * id : the deviceid
    * user : the user on which this may depend. 
* function **getState** (`bytes32 id`, `uint256 index`)     
    constant returns (`address controller`, `uint64 rentedFrom`, `uint64 rentedUntil`, `uint128 properties`)    
    returns the important informations of a state 
    * id : deviceid
    * index : stateindex 
* function **getStateCount** (`bytes32 id`)     
    constant returns (`uint256`)    
    returns the current amount of renting states
    * id : deviceid 
* function **price** (`bytes32 id`, `address user`, `uint32 secondsToRent`, `address token`)     
    constant returns (`uint128`)    
    the price for rentinng the device
    * id : the deviceid
    * secondsToRent : the time of rental in seconds
    * token : the address of the token to pay (See token-addreesses for details)
    * user : the user because prices may depend on the user (whitelisted or discount) 
* function **rent** (`bytes32 id`, `uint32 secondsToRent`, `address token`)     
    payable    
    rents a device, which means it will change the state by setting the sender as controller.
    * id : the deviceid
    * secondsToRent : the time of rental in seconds
    * token : the address of the token to pay (See token-addreesses for details) 
* function **returnObject** (`bytes32 id`)     
        
    returns the Object or Device and also the funds are returned in case he returns it earlier than rentedUntil.
    * id : the deviceid 
* function **supportedTokens** (`bytes32 id`)     
    constant returns (`address[] addresses`)    
    a list of supported tokens for the given device
    * id : the deviceid 
* function **tokenReceiver** (`bytes32 id`, `address token`)     
    constant returns (`bytes32`)    
    the receiver of the token, which may be different than the owner or even a Bitcoin-address. For fiat this may be hash of payment-data.
    * id : the deviceid
    * token : the paid token 

## `interface` AccessSupport

control the access for a device.
See [features/AccessSupport.sol](https://github.com/slockit/usn-mvp/blob/develop/contracts/features/AccessSupport.sol)

```javascript
[{"constant":true,"inputs":[{"name":"id","type":"bytes32"},{"name":"user","type":"address"},{"name":"action","type":"bytes4"}],"name":"canAccess","outputs":[{"name":"","type":"bool"}],"payable":false,"stateMutability":"view","type":"function"}]
```

* **constructor** ()

* function **canAccess** (`bytes32 id`, `address user`, `bytes4 action`)     
    constant returns (`bool`)    
    verifies whther the user is in control or has access to the device.
    * action : constant which the user wants to execute
    * id : the deviceid
    * user : the user on which this depend. 

## `interface` RemoteFeatureSupport

support controlling acces for only identifieable users, which can be based on keybase or other whitleists
See [features/RemoteFeatureSupport.sol](https://github.com/slockit/usn-mvp/blob/develop/contracts/features/RemoteFeatureSupport.sol)

```javascript
[{"constant":true,"inputs":[],"name":"featureCount","outputs":[{"name":"","type":"uint256"}],"payable":false,"stateMutability":"view","type":"function"},{"constant":true,"inputs":[{"name":"","type":"uint256"}],"name":"features","outputs":[{"name":"feature","type":"address"},{"name":"iface","type":"uint64"}],"payable":false,"stateMutability":"view","type":"function"},{"constant":false,"inputs":[{"name":"_addr","type":"address"},{"name":"_interface","type":"uint64"}],"name":"setRemoteFeature","outputs":[],"payable":false,"stateMutability":"nonpayable","type":"function"},{"constant":false,"inputs":[{"name":"_addr","type":"address"}],"name":"removeRemoteFeature","outputs":[{"name":"found","type":"bool"}],"payable":false,"stateMutability":"nonpayable","type":"function"}]
```

* **constructor** ()

* function **featureCount** ()     
    constant returns (`uint256`) 
* function **features** (`uint256`)     
    constant returns (`address feature`, `uint64 iface`) 
* function **removeRemoteFeature** (`address _addr`)     
     returns (`bool found`) 
* function **setRemoteFeature** (`address _addr`, `uint64 _interface`)     
     

## `interface` TimeRangeSupport

supports checks for a minimum and maximum time to rent
See [features/TimeRangeSupport.sol](https://github.com/slockit/usn-mvp/blob/develop/contracts/features/TimeRangeSupport.sol)

```javascript
[{"constant":false,"inputs":[{"name":"id","type":"bytes32"},{"name":"min","type":"uint32"},{"name":"max","type":"uint32"}],"name":"setRangeSeconds","outputs":[],"payable":false,"stateMutability":"nonpayable","type":"function"},{"constant":true,"inputs":[{"name":"id","type":"bytes32"}],"name":"getRangeSeconds","outputs":[{"name":"min","type":"uint32"},{"name":"max","type":"uint32"}],"payable":false,"stateMutability":"view","type":"function"}]
```

* **constructor** ()

* function **getRangeSeconds** (`bytes32 id`)     
    constant returns (`uint32 min`, `uint32 max`)    
    returns the minimum and maximun time this device can be rented
    * id : the device id 
* function **setRangeSeconds** (`bytes32 id`, `uint32 min`, `uint32 max`)     
        
    sets the minimum and maximun time this device can be rented
    * id : the device id
    * max : the maximun time in seconds or 0 if there is no limit
    * min : the minimum time in seconds or 0 if there is no limit 

## `interface` OffChainSupport

defines Rules, which can be verified, even if the state is held on chain.
See [features/OffChainSupport.sol](https://github.com/slockit/usn-mvp/blob/develop/contracts/features/OffChainSupport.sol)

```javascript
[{"constant":true,"inputs":[{"name":"id","type":"bytes32"},{"name":"secondsToRent","type":"uint32"},{"name":"amount","type":"uint128"},{"name":"token","type":"address"},{"name":"user","type":"address"},{"name":"tokenReceiver","type":"bytes32"},{"name":"controllerBefore","type":"address"},{"name":"rentedUntilBefore","type":"uint64"},{"name":"depositBefore","type":"uint128"}],"name":"rentIf","outputs":[{"name":"error","type":"uint16"}],"payable":false,"stateMutability":"view","type":"function"},{"constant":true,"inputs":[{"name":"id","type":"bytes32"},{"name":"user","type":"address"},{"name":"action","type":"bytes4"},{"name":"controller","type":"address"},{"name":"rentedUntil","type":"uint64"},{"name":"properties","type":"uint128"}],"name":"canAccessIf","outputs":[{"name":"","type":"bool"}],"payable":false,"stateMutability":"view","type":"function"}]
```

* **constructor** ()

* function **canAccessIf** (`bytes32 id`, `address user`, `bytes4 action`, `address controller`, `uint64 rentedUntil`, `uint128 properties`)     
    constant returns (`bool`)    
    checks if the user would have access if under the given state
    * action : intended action
    * controller : current controller of the device
    * id : deviceid
    * properties : properties of the device
    * rentedUntil : planned end of the rent
    * user : user trying to rent the device 
* function **rentIf** (`bytes32 id`, `uint32 secondsToRent`, `uint128 amount`, `address token`, `address user`, `bytes32 tokenReceiver`, `address controllerBefore`, `uint64 rentedUntilBefore`, `uint128 depositBefore`)     
    constant returns (`uint16 error`)    
    checks if the renting would be valid given the previous state and passed parameters
    * amount : amount the user will pay
    * controllerBefore : current controller of the device
    * depositBefore : current deposit of the user
    * id : deviceid
    * rentedUntilBefore : current timestamp for the end of a rent
    * secondsToRent : timespan for the rent
    * token : token the user will use for payment
    * tokenReceiver : receiver of the tokens / payments
    * user : user trying to rent the device 

## `interface` RentForSupport

renting from a different account then the controller
See [features/RentForSupport.sol](https://github.com/slockit/usn-mvp/blob/develop/contracts/features/RentForSupport.sol)

```javascript
[{"constant":false,"inputs":[{"name":"id","type":"bytes32"},{"name":"start","type":"uint256"}],"name":"removeBooking","outputs":[],"payable":false,"stateMutability":"nonpayable","type":"function"},{"constant":false,"inputs":[{"name":"id","type":"bytes32"},{"name":"secondsToRent","type":"uint32"},{"name":"token","type":"address"},{"name":"controller","type":"address"},{"name":"rentedFrom","type":"uint64"}],"name":"rentFor","outputs":[],"payable":true,"stateMutability":"payable","type":"function"},{"constant":false,"inputs":[{"name":"id","type":"bytes32"},{"name":"secondsToRent","type":"uint32"},{"name":"token","type":"address"},{"name":"controller","type":"address"}],"name":"rentForNow","outputs":[],"payable":true,"stateMutability":"payable","type":"function"},{"constant":true,"inputs":[{"name":"id","type":"bytes32"},{"name":"user","type":"address"},{"name":"start","type":"uint64"},{"name":"secondsToRent","type":"uint32"}],"name":"canBeRented","outputs":[{"name":"","type":"bool"}],"payable":false,"stateMutability":"view","type":"function"}]
```

* **constructor** ()

* function **canBeRented** (`bytes32 id`, `address user`, `uint64 start`, `uint32 secondsToRent`)     
    constant returns (`bool`)    
    checks if a booking is possible
    * id : device id
    * secondsToRent : the duration to book
    * start : the timestamp when the booking should start
    * user : the booking user 
* function **removeBooking** (`bytes32 id`, `uint256 start`)     
        
    cancels a booking
    * id : device id
    * start : the timestamp when the booking should start 
* function **rentFor** (`bytes32 id`, `uint32 secondsToRent`, `address token`, `address controller`, `uint64 rentedFrom`)     
    payable    
    rents a device, which means it will change the state by setting the sender as controller.
    * controller : the user which should be allowed to control the device.
    * id : the deviceid
    * rentedFrom : the start time for this rental
    * secondsToRent : the time of rental in seconds
    * token : the address of the token to pay (See token-addreesses for details) 
* function **rentForNow** (`bytes32 id`, `uint32 secondsToRent`, `address token`, `address controller`)     
    payable    
    rents a device, which means it will change the state by setting the sender as controller.
    * controller : the user which should be allowed to control the device.
    * id : the deviceid
    * secondsToRent : the time of rental in seconds
    * token : the address of the token to pay (See token-addreesses for details) 

## `interface` DepositSupport

supports saving a deposit and returning it afterwards
See [features/DepositSupport.sol](https://github.com/slockit/usn-mvp/blob/develop/contracts/features/DepositSupport.sol)

```javascript
[{"constant":true,"inputs":[{"name":"id","type":"bytes32"},{"name":"user","type":"address"},{"name":"secondsToRent","type":"uint32"},{"name":"token","type":"address"}],"name":"deposit","outputs":[{"name":"","type":"uint128"}],"payable":false,"stateMutability":"view","type":"function"},{"constant":true,"inputs":[{"name":"user","type":"address"},{"name":"id","type":"bytes32"}],"name":"storedDeposit","outputs":[{"name":"amount","type":"uint128"},{"name":"token","type":"address"},{"name":"access","type":"uint64"}],"payable":false,"stateMutability":"view","type":"function"},{"constant":false,"inputs":[{"name":"user","type":"address"},{"name":"id","type":"bytes32"}],"name":"returnDeposit","outputs":[],"payable":false,"stateMutability":"nonpayable","type":"function"},{"anonymous":false,"inputs":[{"indexed":true,"name":"user","type":"address"},{"indexed":true,"name":"id","type":"bytes32"},{"indexed":false,"name":"amount","type":"uint128"},{"indexed":false,"name":"token","type":"address"},{"indexed":false,"name":"access","type":"uint64"}],"name":"LogDepositStored","type":"event"},{"anonymous":false,"inputs":[{"indexed":true,"name":"user","type":"address"},{"indexed":true,"name":"id","type":"bytes32"},{"indexed":false,"name":"amount","type":"uint128"},{"indexed":false,"name":"token","type":"address"}],"name":"LogDepositReturned","type":"event"}]
```

* **constructor** ()
* event **LogDepositStored** (`addressindexed user`, `bytes32indexed id`, `uint128 amount`, `address token`, `uint64 access`)     
    triggered when a deposit was stored
* event **LogDepositReturned** (`addressindexed user`, `bytes32indexed id`, `uint128 amount`, `address token`)     
    triggered when a deposit was returned

* function **deposit** (`bytes32 id`, `address user`, `uint32 secondsToRent`, `address token`)     
    constant returns (`uint128`)    
    calculates the needed deposit for a device
    * id : deviceid
    * secondsToRent : timespan of the rent
    * token : token used for payment   
    * user : user with deposit 
* function **returnDeposit** (`address user`, `bytes32 id`)     
        
    returns the deposit for the given user, which can be called after the wait-period
    * id : deviceid 
    * user : the user that wants his deposit back 
* function **storedDeposit** (`address user`, `bytes32 id`)     
    constant returns (`uint128 amount`, `address token`, `uint64 access`)    
    gives information about the stored deposit of a user
    * id : deviceid
    * user : the user with the deposit 

## `interface` BlackListSupport

defines a blacklist for users to be rejected
See [features/BlackListSupport.sol](https://github.com/slockit/usn-mvp/blob/develop/contracts/features/BlackListSupport.sol)

```javascript
[{"constant":false,"inputs":[{"name":"id","type":"bytes32"},{"name":"user","type":"address"},{"name":"forbidden","type":"bool"}],"name":"setBlacklisted","outputs":[],"payable":false,"stateMutability":"nonpayable","type":"function"},{"constant":true,"inputs":[{"name":"id","type":"bytes32"},{"name":"user","type":"address"}],"name":"isBlacklisted","outputs":[{"name":"","type":"bool"}],"payable":false,"stateMutability":"view","type":"function"}]
```

* **constructor** ()

* function **isBlacklisted** (`bytes32 id`, `address user`)     
    constant returns (`bool`)    
    checks if a user is blacklisted
    * id : device id
    * user : the user to check 
* function **setBlacklisted** (`bytes32 id`, `address user`, `bool forbidden`)     
        
    changes the blacklist
    * forbidden : if true he will be blacklisted
    * id : device id
    * user : the user to check 

## `interface` DependendAccessSupport

supports acces control for depending devices.
See [features/DependendAccessSupport.sol](https://github.com/slockit/usn-mvp/blob/develop/contracts/features/DependendAccessSupport.sol)

```javascript
[{"constant":false,"inputs":[{"name":"id","type":"bytes32"},{"name":"contracts","type":"address[]"},{"name":"ids","type":"bytes32[]"}],"name":"setDependingService","outputs":[],"payable":false,"stateMutability":"nonpayable","type":"function"},{"constant":true,"inputs":[{"name":"id","type":"bytes32"}],"name":"getDependingService","outputs":[{"name":"contracts","type":"address[]"},{"name":"ids","type":"bytes32[]"}],"payable":false,"stateMutability":"view","type":"function"}]
```

* **constructor** ()

* function **getDependingService** (`bytes32 id`)     
    constant returns (`address[] contracts`, `bytes32[] ids`)    
    returns a list of contracts and deviceIds, which will also have access, if the user books this device.
    * id : the device id 
* function **setDependingService** (`bytes32 id`, `address[] contracts`, `bytes32[] ids`)     
        
    sets a list of contracts and deviceIds, which will also have access, if the user books this device.
    * contracts : the depending contracts
    * id : the device id
    * ids : the depending device ids 

## `interface` DiscountSupport

support controlling acces for only identifieable users, which can be based on keybase or other whitleists
See [features/DiscountSupport.sol](https://github.com/slockit/usn-mvp/blob/develop/contracts/features/DiscountSupport.sol)

```javascript
[{"constant":true,"inputs":[{"name":"id","type":"bytes32"},{"name":"user","type":"address"},{"name":"secondsToRent","type":"uint32"},{"name":"token","type":"address"},{"name":"standardPrice","type":"uint128"}],"name":"discount","outputs":[{"name":"","type":"uint128"}],"payable":false,"stateMutability":"view","type":"function"},{"constant":false,"inputs":[{"name":"ruleId","type":"uint64"},{"name":"discountType","type":"uint8"},{"name":"limit","type":"uint128"},{"name":"_discount","type":"uint128"}],"name":"setDiscountRule","outputs":[],"payable":false,"stateMutability":"nonpayable","type":"function"}]
```

* **constructor** ()

* function **discount** (`bytes32 id`, `address user`, `uint32 secondsToRent`, `address token`, `uint128 standardPrice`)     
    constant returns (`uint128`)    
    the amount, which should price for rentinng the device
    * id : the deviceid
    * secondsToRent : the time of rental in seconds
    * token : the address of the token to pay (See token-addreesses for details)
    * user : the user because prices may depend on the user (whitelisted or discount) 
* function **setDiscountRule** (`uint64 ruleId`, `uint8 discountType`, `uint128 limit`, `uint128 _discount`)     
        
    defines a new rule for discounts
    * _discount : discount
    * discountType : type of the discount
    * limit : limit of the discount
    * ruleId : ruleid 

## `interface` GroupedSupport


See [features/GroupedSupport.sol](https://github.com/slockit/usn-mvp/blob/develop/contracts/features/GroupedSupport.sol)

```javascript
[{"constant":true,"inputs":[{"name":"id","type":"bytes32"}],"name":"nextID","outputs":[{"name":"","type":"bytes32"}],"payable":false,"stateMutability":"view","type":"function"},{"constant":true,"inputs":[{"name":"group","type":"bytes32"}],"name":"getCount","outputs":[{"name":"","type":"uint64"}],"payable":false,"stateMutability":"view","type":"function"}]
```

* **constructor** ()

* function **getCount** (`bytes32 group`)     
    constant returns (`uint64`)    
    returns the number of devices for the given group
    * group : group 
* function **nextID** (`bytes32 id`)     
    constant returns (`bytes32`)    
    returns the next deviceId 
    * id : deviceid 

## `interface` IdentitySupport

support controlling acces for only identifieable users, which can be based on keybase or other whitleists
See [features/IdentitySupport.sol](https://github.com/slockit/usn-mvp/blob/develop/contracts/features/IdentitySupport.sol)

```javascript
[{"constant":true,"inputs":[{"name":"user","type":"address"}],"name":"isIdentified","outputs":[{"name":"","type":"bool"}],"payable":false,"stateMutability":"view","type":"function"}]
```

* **constructor** ()

* function **isIdentified** (`address user`)     
    constant returns (`bool`)    
    returns tru if the user can be identified.
    * user : user 

## `class` MultisigSupport

support mutlisigs as controller of a device. In this case all keyholders have access when rented.
See [features/MultisigSupport.sol](https://github.com/slockit/usn-mvp/blob/develop/contracts/features/MultisigSupport.sol)

```javascript
[]
```

* **constructor** ()


## `interface` RemoteFeatureProvider

interface for a remote feature-contract
See [features/RemoteFeatureSupport.sol](https://github.com/slockit/usn-mvp/blob/develop/contracts/features/RemoteFeatureSupport.sol)

```javascript
[{"constant":true,"inputs":[{"name":"registry","type":"address"},{"name":"id","type":"bytes32"},{"name":"user","type":"address"},{"name":"rentedFrom","type":"uint64"},{"name":"secondsToRent","type":"uint32"},{"name":"token","type":"address"},{"name":"srcPrice","type":"uint128"}],"name":"price","outputs":[{"name":"","type":"uint128"}],"payable":false,"stateMutability":"view","type":"function"},{"constant":true,"inputs":[{"name":"registry","type":"address"},{"name":"_id","type":"bytes32"},{"name":"_user","type":"address"},{"name":"action","type":"bytes4"},{"name":"_controller","type":"address"},{"name":"_rentedUntil","type":"uint64"},{"name":"subnode","type":"bytes32"}],"name":"canAccessIf","outputs":[{"name":"","type":"bool"}],"payable":false,"stateMutability":"view","type":"function"},{"constant":true,"inputs":[{"name":"registry","type":"address"},{"name":"_id","type":"bytes32"},{"name":"_secondsToRent","type":"uint32"},{"name":"_token","type":"address"},{"name":"_controller","type":"address"},{"name":"_rentedFrom","type":"uint64"},{"name":"props","type":"uint128"},{"name":"nodeID","type":"bytes32"}],"name":"canRentFor","outputs":[{"name":"rentable","type":"bool"}],"payable":false,"stateMutability":"view","type":"function"}]
```

* **constructor** ()

* function **canAccessIf** (`address registry`, `bytes32 _id`, `address _user`, `bytes4 action`, `address _controller`, `uint64 _rentedUntil`, `bytes32 subnode`)     
    constant returns (`bool`) 
* function **canRentFor** (`address registry`, `bytes32 _id`, `uint32 _secondsToRent`, `address _token`, `address _controller`, `uint64 _rentedFrom`, `uint128 props`, `bytes32 nodeID`)     
    constant returns (`bool rentable`) 
* function **price** (`address registry`, `bytes32 id`, `address user`, `uint64 rentedFrom`, `uint32 secondsToRent`, `address token`, `uint128 srcPrice`)     
    constant returns (`uint128`) 

## `interface` StateChannelSupport

support  StateChannels
See [features/StateChannelSupport.sol](https://github.com/slockit/usn-mvp/blob/develop/contracts/features/StateChannelSupport.sol)

```javascript
[{"constant":true,"inputs":[{"name":"id","type":"bytes32"}],"name":"supportStateChannels","outputs":[{"name":"","type":"bool"}],"payable":false,"stateMutability":"view","type":"function"},{"constant":true,"inputs":[],"name":"stateChannelMgr","outputs":[{"name":"","type":"address"}],"payable":false,"stateMutability":"view","type":"function"}]
```

* **constructor** ()

* function **stateChannelMgr** ()     
    constant returns (`address`)    
    the address of the stateChannel manager 
* function **supportStateChannels** (`bytes32 id`)     
    constant returns (`bool`)    
    check whether the single device is also allowing it
    * id : deviceid 

## `interface` TokenSupport

supports exchanges between different tokens
See [features/TokenSupport.sol](https://github.com/slockit/usn-mvp/blob/develop/contracts/features/TokenSupport.sol)

```javascript
[{"constant":false,"inputs":[{"name":"id","type":"bytes32"},{"name":"token","type":"address"},{"name":"newTarget","type":"address"}],"name":"setTokenReceiver","outputs":[],"payable":false,"stateMutability":"nonpayable","type":"function"},{"constant":false,"inputs":[{"name":"id","type":"bytes32"},{"name":"addresses","type":"address[]"}],"name":"setSupportedTokens","outputs":[],"payable":false,"stateMutability":"nonpayable","type":"function"}]
```

* **constructor** ()

* function **setSupportedTokens** (`bytes32 id`, `address[] addresses`)     
        
    sets the list of supported tokens
    * addresses : array with supported token-addresses
    * id : deviceid 
* function **setTokenReceiver** (`bytes32 id`, `address token`, `address newTarget`)     
        
    sets the receiver of a token
    * id : deviceid
    * newTarget : new receiver of the tokens
    * token : supported token address 

## `interface` VerifiedDeviceSupport

supports for signing messages by the device.
See [features/VerifiedDeviceSupport.sol](https://github.com/slockit/usn-mvp/blob/develop/contracts/features/VerifiedDeviceSupport.sol)

```javascript
[{"constant":true,"inputs":[{"name":"id","type":"bytes32"}],"name":"devicePubKey","outputs":[{"name":"","type":"bytes32"}],"payable":false,"stateMutability":"view","type":"function"},{"constant":false,"inputs":[{"name":"id","type":"bytes32"},{"name":"_address","type":"bytes32"}],"name":"setDevicePubKey","outputs":[],"payable":false,"stateMutability":"nonpayable","type":"function"}]
```

* **constructor** ()

* function **devicePubKey** (`bytes32 id`)     
    constant returns (`bytes32`)    
    returns the public address of the device
    * id : deviceid 
* function **setDevicePubKey** (`bytes32 id`, `bytes32 _address`)     
        
    sets the public address of the device
    * _address : address
    * id : deviceid 

## `interface` VerifiedDeviceSupportPerContractImpl

> is [OwnerSupport](https://github.com/slockit/usn-mvp/blob/develop/contracts/features/README.md#interface-ownersupport), [VerifiedDeviceSupport](https://github.com/slockit/usn-mvp/blob/develop/contracts/features/README.md#interface-verifieddevicesupport)    

the Implementation for a whitelist for all devices in this contract.
See [features/VerifiedDeviceSupport.sol](https://github.com/slockit/usn-mvp/blob/develop/contracts/features/VerifiedDeviceSupport.sol)

```javascript
[{"constant":true,"inputs":[],"name":"deviceVerificationAddress","outputs":[{"name":"","type":"bytes32"}],"payable":false,"stateMutability":"view","type":"function"},{"constant":true,"inputs":[{"name":"","type":"bytes32"}],"name":"devicePubKey","outputs":[{"name":"","type":"bytes32"}],"payable":false,"stateMutability":"view","type":"function"},{"constant":false,"inputs":[{"name":"_id","type":"bytes32"},{"name":"_address","type":"bytes32"}],"name":"setDevicePubKey","outputs":[],"payable":false,"stateMutability":"nonpayable","type":"function"},{"constant":true,"inputs":[{"name":"id","type":"bytes32"}],"name":"deviceOwner","outputs":[{"name":"","type":"address"}],"payable":false,"stateMutability":"view","type":"function"}]
```

* **constructor** ()

* function **deviceVerificationAddress** ()     
    constant returns (`bytes32`)    
    the address 

## `interface` VerifiedDeviceSupportPerDeviceImpl

> is [VerifiedDeviceSupport](https://github.com/slockit/usn-mvp/blob/develop/contracts/features/README.md#interface-verifieddevicesupport), [OwnerSupport](https://github.com/slockit/usn-mvp/blob/develop/contracts/features/README.md#interface-ownersupport)    

supports for signing messages by the device with one signature per device.
See [features/VerifiedDeviceSupport.sol](https://github.com/slockit/usn-mvp/blob/develop/contracts/features/VerifiedDeviceSupport.sol)

```javascript
[{"constant":true,"inputs":[{"name":"_id","type":"bytes32"}],"name":"devicePubKey","outputs":[{"name":"","type":"bytes32"}],"payable":false,"stateMutability":"view","type":"function"},{"constant":false,"inputs":[{"name":"_id","type":"bytes32"},{"name":"_address","type":"bytes32"}],"name":"setDevicePubKey","outputs":[],"payable":false,"stateMutability":"nonpayable","type":"function"},{"constant":true,"inputs":[{"name":"id","type":"bytes32"}],"name":"deviceOwner","outputs":[{"name":"","type":"address"}],"payable":false,"stateMutability":"view","type":"function"}]
```

* **constructor** ()


## `interface` WeekCalendarSupport

supports controlling acces by setting opening times per weekday
See [features/WeekCalendarSupport.sol](https://github.com/slockit/usn-mvp/blob/develop/contracts/features/WeekCalendarSupport.sol)

```javascript
[{"constant":false,"inputs":[{"name":"id","type":"bytes32"},{"name":"_blockedTimes","type":"uint256"}],"name":"setBlockedTimes","outputs":[],"payable":false,"stateMutability":"nonpayable","type":"function"},{"constant":true,"inputs":[{"name":"id","type":"bytes32"},{"name":"time","type":"uint64"}],"name":"isBlocked","outputs":[{"name":"","type":"bool"}],"payable":false,"stateMutability":"view","type":"function"},{"constant":true,"inputs":[{"name":"id","type":"bytes32"}],"name":"blockedTimes","outputs":[{"name":"","type":"uint256"}],"payable":false,"stateMutability":"view","type":"function"}]
```

* **constructor** ()

* function **blockedTimes** (`bytes32 id`)     
    constant returns (`uint256`)    
    a bitmask for 24*7 bits where each set bit is blocking and signals closed
    * id : deviceid 
* function **isBlocked** (`bytes32 id`, `uint64 time`)     
    constant returns (`bool`)    
    checks if this is closed
    * id : deviceid
    * time : intended accesstime 
* function **setBlockedTimes** (`bytes32 id`, `uint256 _blockedTimes`)     
        
    a bitmask for 24*7 bits where each set bit is blocking and signals closed
    * _blockedTimes : blocked times
    * id : deviceid 

## `interface` WhitelistSupport

Defines additional access for certain users, (like the cleaning lady in an appartment)
See [features/WhitelistSupport.sol](https://github.com/slockit/usn-mvp/blob/develop/contracts/features/WhitelistSupport.sol)

```javascript
[{"constant":false,"inputs":[{"name":"id","type":"bytes32"},{"name":"user","type":"address"},{"name":"hasAccess","type":"bool"}],"name":"setAccessWhitelist","outputs":[],"payable":false,"stateMutability":"nonpayable","type":"function"},{"anonymous":false,"inputs":[{"indexed":true,"name":"id","type":"bytes32"},{"indexed":true,"name":"controller","type":"address"},{"indexed":false,"name":"permission","type":"bool"}],"name":"LogAccessChanged","type":"event"}]
```

* **constructor** ()
* event **LogAccessChanged** (`bytes32indexed id`, `addressindexed controller`, `bool permission`)     
    whenever the whitelist changes, an event must be triggered

* function **setAccessWhitelist** (`bytes32 id`, `address user`, `bool hasAccess`)     
        
    changes values on the whitelist
    * hasAccess : true|false to give permission
    * id : device id
    * user : the user to put on the whitelist 

