(This kata is designed to practice [[London-school TDD]].)

You work at a credit card company and as a value-add they want to start providing alerts to users when their spending in any particular category is higher than usual.

## Requirements

* A `Payment` is a simple value object with a `price`, `description`, and `category`
* A `Category` is an enumerable type of a collection of things like "entertainment", "restaurants", and "golf"
* Compare the payments for the current month and previous month, grouped by the total prices by category; flag the categories for which the user spent at least 50% more this month than last month
* Compose an e-mail message to the user that lists the spending categories for which spending was unusually high, with a subject like "Unusual spending of $1076 detected!" and this body:
```
Hello card user!

We have detected unusually high spending on your card in these categories:

* You spent $148 on groceries
* You spent $928 on travel

Love,

The Credit Card Company
```

## Periphery of the app

Suppose that a few pieces are either already built for you or will be provided by other teams in your organization, namely:

* The scheduling daemon that triggers the job is handled separately. So long as you implement an entry point with a `trigger(userId)` function, the scheduler will be able to fire the job
* Another team will provide an API access function for fetching the payments for a particular user, year, and month, but it's not implemented yet (and the method signature might change), so wrap the provided class with an object you own and stub it. It currently looks like this:
``` java
package com.spending.thirdparty;

import java.util.Set;
import com.spending.values.Payment;

public class FetchesUserPaymentsByMonth {
  public static Set<Payment> fetch(long userId, int year, int month) {
    throw new RuntimeException("Data access will be implemented by a different team later");
  }
}
```
* Another team will provide a method for sending e-mails to a particular user for you, but since you don't control it, wrap it with an adapter and spy on that adapter instead; it currently looks like:
``` java
package com.spending.thirdparty;

public class EmailsUser {
  public static void email(long userId, String subject, String body) {
    throw new RuntimeException("Email will be implemented by a different team later");
  } 
}
```

## After you're done…

Once you've completed the kata, if you'd like to test your approach for how easy it is to change, try these requested requirement changes:

* Load three months of payment history and compare the current month to their average totals by category (you're guaranteed to have the most recent month, but either of the two prior months might come back as empty sets, as if the user lacks the payment history)
* Update the e-mail to report what the usual spending amount was, in addition to the unusual spending amount