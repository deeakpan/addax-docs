# Trading

The book is bids and asks. The price you get is the best price that can fill the size. Orders at the same price are filled in time order.

A post only order is added to the book and is rejected if it would trade immediately. An immediate or cancel order trades what it can and cancels the rest. A good till time order stays until it fills, you cancel it, or it expires. Expiry can be from 5 minutes to 30 days.

Funding runs once an hour. A positive rate means longs pay shorts.

Addax is built on [Lighter](https://lighter.xyz/).
