---
published: false
---
---
published: false
---
## 8 Days a Week: Rolling up Unique Days of the Week onto a Parent Record

**Use Case**
A master object (think "Course") has a detail object (think "Session"). The detail object is primarily defined by a date field, so in this case, the date of the Session. What if you want to represent the days of the week that the Sessions occur on in a field on the Course record? So if the Session records fell on Mondays and Wednesdays, you would want something like "M, W", "Mon/Wed", or "Mondays, Wednesdays". And if you added a Thursday session, the field would read "M, W, Th", "Mon/Wed/Thur", or "Mondays, Wednesdays, Thursdays". And what if you wanted to _entirely automate this field_ to preserve data integrity?

Well, if you're me, you stew on the concept for a few days pondering how to attack it with code, and then you wake up like a shot at 6AM one morning with a really simple declarative solution. Huzzah!