# inAppPurchase

> In-app purchases on Mac App Store.

进程：[主进程](../glossary.md#main-process)

## 事件

The `inAppPurchase` module emits the following events:

### Event: 'transactions-updated'

Emitted when one or more transactions have been updated.

返回:

* `event` Event
* `transactions` ([Transaction[]](structures/transaction.md) - Array of transactions.

## 方法

The `inAppPurchase` module has the following methods:

### `inAppPurchase.purchaseProduct(productID, quantity, callback)`

* `productID` String - The id of the product to purchase. (the id of `com.example.app.product1` is `product1`).
* `quantity` Integer (optional) - The number of items the user wants to purchase.
* `callback` Function (optional) - The callback called when the payment is added to the PaymentQueue. (You should add a listener with `inAppPurchase.addTransactionsListener` to get the transaction status). 
  * `isProductValid` Boolean - Determine if the product is valid and added to the payment queue.

### `inAppPurchase.canMakePayments()`

Returns `Boolean`, whether a user can make a payment.

### `inAppPurchase.getReceiptURL()`

Returns `String`, the path to the receipt.