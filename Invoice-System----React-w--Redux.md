

This Kata was presented at XP2006 by EmmanuelGaillot and ChristopheThibaut.

## Problem Description

### User Story 1

Clicking on the New Relic or CircleCi invoice will show you a total of $1,000,000. We'd like this amount to come from the subtotals of the given line items.

Refactor note: We'd like to also look at starting to add testing beyond snapshot tests as our application moves from an MVP phase.

### User Story 2

Use the discount_percent in the payload to discount the line item. Show the previous amount, the new amount, and the amount discounted.  Example below:

<pre>
Name                | Description                              | Unit Price | Quantity | Discount | Subtotal
Anti-Virus Software | Subscription services for virus software | $200       | 1        |      50% | <strike>$200</strike> $100
</pre>
![](https://gitlab.com/testdouble/accounts-receivable-react/uploads/5ea855ea29cf553527c54e211cfb9842/image.png)

### User Story 3

Create a form to store out new invoices along with their line items.  (note we should add some type of product w/ price here to access for a drop down) 

### User Story 4

