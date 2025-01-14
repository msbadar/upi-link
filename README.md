# upi-link 
Simple npm module to generate UPI deep-links, based on UPI Linking Specifications [Version 1.5.1](https://www.npci.org.in/sites/all/themes/npcl/images/PDF/UPI_Linking_Specs_ver_1.5.1.pdf) :money_mouth_face:

## installation
```
$ npm i upi-link
```
## API
#### Factory methods
| Function Name |  Input params | Returns |
| ------------- |-------------- |---------- |
| Satic   | VPA ID(M), Payee Name(M), Amount(O)| Builder object |
| Dynamic | VPA ID(M), Payee Name(M), Amount(M) | Builder object|
| SaticP   | VPA ID(M), Payee Name(M), Amount(O)| Promise |
| DynamicP | VPA ID(M), Payee Name(M), Amount(M) | Promise|

#### Setter methods
1. __setMerchant( mc, ti)__ : Set merchant takes tow parameter _Merchant ID_ (mandatory) and _Transaction ID_ (optional). Both the ID should be generated by the PSP, and should be passed as it is.

2. __setTxRef( refid, note)__ : Set merchant takes tow parameter _Transaction Reference ID_ (mandatory) and _Transaction Note_ (optional). Transaction Reference ID should generated by you. It could be order number, subscription number, Bill ID, booking ID, insurance renewal reference, etc. 
    * ___Note___ : Setting up TxRef is mandatory, when either _Merchant_ is set or the _URL type_ is _Dynamic_
3. __setMinAmount(amount)__ : Minimum Amount will only be set when _Transaction Amount_ is already set and the _Minimum Amount_ is different from the _Transaction Amount_.
4. __setCallback(url)__ : This should be a URL when clicked provides customer with further transaction details like complete bill details, bill copy, order copy, ticket details, etc. This can also be used to deliver digital goods.
    * ___Note___ : Setting up _Callback URL_ doesn't guarantee the it will be called by the PSPs post transaction. Call to this _URL_ is subjected _white listing by the PSPs_.

#### Build Method
- __getLink()__ : This will return a QR code friendly formated _UPI URI_ string is all conditions are met.

## Chaining
 Build object chaining
 ```js
 const upi = require('upi-link')
 
 let uri = upi.Static("xxxxx@ybl", "Xyz Abc")
 .setMerchant("ALPHABET")
 .setTxRef('INV001')
 .getLink()
 
 console.log('URI:  ',uri);
 ```
 
 Promise chaining ( __*__ )
 ```js
 const upi = require('upi-link')
 
 upi.DynamicP("xxxxx@ybl", "Xyz Abc", 100000)
 .setMinAmount(1000)
 .setTxRef('INV001')
 .getLink()
 .then( uri => console.log('URI:  ',uri))
 .catch( err => console.error('Error: ',err.message))
 ```
 
 __*__ _Promise chaining is the recomended method_. If you are using plain build object you should ___handled errors asseted by each metods___.
