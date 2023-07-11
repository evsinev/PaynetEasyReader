PaynetEasyReader SDK for iOS
============================

PaynetEasyReader SDK provides a fast and easy integration with mPOS terminals in your mobile apps.

Supported mPOS
--------------

* Spire Payments
 * SPm2
 * SPm20
 * Miura Systems
 * Miura Shuttle (M006)
 * Miura M010
 * Miura M007
 * Verifone Vx820
 * PAX SP30, D200

Get Started
------------

The PaynetEasyReader SDK includes header files and a single static library. We'll walk you through the integration and the usage.

### Requirements

*   Supports target deployment of iOS version 7.0+ and instruction set armv7+ (including 64-bit), x86_64, i386 (for emulator).

### Add the SDK to your project

Add to your Podfile
```
pod "PaynetEasyReader", :git => 'git@github.com:evsinev/PaynetEasyReader.git', :tag => '$VERSION'
```
Please change $VERSION to the latest from the https://github.com/payneteasy/PaynetEasyReader/releases

### For Miura, Spire and Pax

Add to your *-Info.plist
```xml
<key>UISupportedExternalAccessoryProtocols</key>
  <array>
    <string>com.miura.shuttle</string>
    <string>com.thyron</string>
    <string>com.paxsz.ipos</string>    
  </array>
```

### Sample Code

Implement the PNEReaderPresenter protocol 
```obj-c

- (void)stateChanged:(PNEReaderEvent *)aEvent {
   // displays reader status
   // see an example at https://github.com/payneteasy/ReaderExample/blob/master/ReaderExample/PaymentModule/PaymentPresenter.m#L69
}

- (PNEProcessingContinuation *)onCard:(PNECard *)aCard {
    // provide payneteasy.com account info
    PNEProcessingContinuation * continuation = [PNEProcessingContinuation
            continuationWithBaseUrl:@"https://sandbox.payneteasy.com/paynet"
                      merchantLogin:MERCHANT_LOGIN
                        merchantKey:MERCHANT_KEY
                 merchantEndPointId:END_POINT_ID
                 orderInvoiceNumber:[[NSUUID UUID] UUIDString]];

    return continuation;
}

- (void)onCardError:(PNECardError *)aError {
    // deal with the error
    // see an example at https://github.com/payneteasy/ReaderExample/blob/master/ReaderExample/PaymentModule/PaymentPresenter.m#L93
}

- (void)onProcessingEvent:(PNEProcessingEvent *)aEvent {
    // wait for Result event
    // see an example at https://github.com/payneteasy/ReaderExample/blob/master/ReaderExample/PaymentModule/PaymentPresenter.m#L96
}

- (PNEConfigurationContinuation *)onConfiguration {
    return [[PNEConfigurationContinuation alloc]
            initWithBaseUrl:@"https://paynet-qa.clubber.me/paynet/rki"
              merchantLogin:_payment.merchantLogin
                merchantKey:_payment.merchantKey
         merchantEndPointId:_payment.merchantEndPointId
               merchantName:_payment.merchantName
    ];
}
```

Starts Reader Manager

```obj-c
PNEReaderFactory *factory = [[PNEReaderFactory alloc] init];
PNEReaderInfo *reader = [PNEReaderInfo infoWithType:PNEReaderType_MIURA_OR_SPIRE];
// Note: manager must be a property or a field or a static local variable, to prevent an elimination
manager = [factory createManager:reader
                          amount:[NSDecimalNumber decimalNumberWithString:@"1.00"]
                        currency:@"RUB"
                       presenter:self];
[manager start];
```

```obj-c
PNEReaderFactory *factory = [[PNEReaderFactory alloc] init];
PNEReaderInfo *reader = [PNEReaderInfo infoWithType:PNEReaderType_INPAS address:@"10.0.0.100:27015"];
manager = [factory createManager:reader
                          amount:[NSDecimalNumber decimalNumberWithString:@"1.00"]
                        currency:@"RUB"
                       presenter:self];
[manager start];
```

Cancel Payment

```obj-c
PNEReaderFactory *factory = [[PNEReaderFactory alloc] init];
PNEReaderInfo *reader = [PNEReaderInfo infoWithType:PNEReaderType_INPAS address:@"10.0.0.100:27015"];
manager = [factory createManager:reader
                          amount:[NSDecimalNumber decimalNumberWithString:@"1.00"]
                        currency:@"RUB",
                             rrn:@"1234567890",
                       presenter:self];
[manager cancelPayment];
```

Return Payment

```obj-c
PNEReaderFactory *factory = [[PNEReaderFactory alloc] init];
PNEReaderInfo *reader = [PNEReaderInfo infoWithType:PNEReaderType_INPAS address:@"10.0.0.100:27015"];
manager = [factory createManager:reader
                          amount:[NSDecimalNumber decimalNumberWithString:@"1.00"]
                        currency:@"RUB",
                             rrn:@"1234567890",
                       presenter:self];
[manager returnPayment];
```

Reconciliation

```obj-c
PNEReaderFactory *factory = [[PNEReaderFactory alloc] init];
PNEReaderInfo *reader = [PNEReaderInfo infoWithType:PNEReaderType_INPAS address:@"10.0.0.100:27015"];
manager = [factory createManager:reader presenter:self];
[manager reconciliation];
```

## Examples

* Objective-c with Cocoapods - https://github.com/payneteasy/ReaderExample
* Swift with Cocoapods - https://github.com/payneteasy/ReaderExampleSwift
* Swift with Carthage  - https://github.com/payneteasy/ReaderExampleSwiftCarthage

## Sign up for your account 

* Please contact info@payneteasy.com for your merchant account.
 
