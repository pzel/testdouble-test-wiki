(This kata is designed to practice [[London-school TDD]].)

You work at a credit card company and as a value-add they want to start providing alerts to users when their spending in any particular category is higher than usual.

## Assumptions

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