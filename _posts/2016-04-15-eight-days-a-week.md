---
published: false
---
## 8 Days a Week: Rolling up Unique Days of the Week onto a Parent Record

**Use Case**

A master object (think "Course") has a detail object (think "Session"). The detail object is primarily defined by a date field, so in this case, the date of the Session. What if you want a summary of those Sessions, just the days of the week that the Sessions occur on, in a field on the Course record? So if the Session records fell on Mondays and Wednesdays, you would want something like "M, W", "Mon/Wed", or "Mondays, Wednesdays". And if you added a Thursday session, the field would need to concatenate that too, now reading "M, W, Th", "Mon/Wed/Thur", or "Mondays, Wednesdays, Thursdays". And what if you wanted to _entirely automate this field_ to preserve data integrity?

Well, if you're me, you stew on the concept for a few days pondering how to attack it with code, and then you wake up like a shot at 6AM one morning with a really simple declarative solution. Huzzah!

It all starts with a formula field. In this case, I'm just looking for a "M/T/W/Th/F/Sa/Su" style. So in a hidden formula field on the Session record called `Day_of_Week__c`, we'll figure out which day of the week that Session is. 

```
CASE(
MOD( Date__c - DATE( 1900, 1, 7 ), 7 ), 
0, "Su", 
1, "M", 
2, "T", 
3, "W", 
4, "Th", 
5, "F", 
"Sa" 
)
```

This is a really common pattern for finding the day of week of a date. We take the date (`Date__c`), subtract a known Sunday (`DATE( 1900, 1, 7 )`) to get the total number of days between the two dates, divide by 7 (a week), and check the remainder (that's the `MOD()` function!) with a `CASE()` function. If the remainder is 0, `Date__c` is a Sunday. If the remainder is 1, it's a Monday. And so on. (By the time we get to Saturday, we're at our `else` condition in the `CASE()` function, so that's why we don't explicitly check for a remainder of 6.)

So now each Session has a hidden field with the day of week abbreviation on it. That's handy, but they're still not on the Course record.

Enter [Declarative Lookup Rollup Summaries](https://github.com/afawcett/declarative-lookup-rollup-summaries), one of my favorite apps. It not only lets you create rollups across lookup relationships (_or_ master-details, it doesn't actually care), but it also has more powerful filtering (relative date filters, anyone?) and functions. One of those is the ability to concatenate text strings, and even concatenate only _unique_ text strings. Do you see it yet?

All I have to do is set up a rollup that concatenates only the _unique_ values of the `Day_of_Week__c` field from the `Session__c` record up on to the `Course__c` object, into a new field called `Days_of_Week__c`, and set the concatenation delimiter (the thing that goes between my unique values) to `/`! 

Voila! We just turned this...

![Sessions Related List.jpg]({{site.baseurl}}/_drafts/Sessions Related List.jpg)

...into this!

![Concatenated FIeld.png]({{site.baseurl}}/_drafts/Concatenated FIeld.png)

What other use cases can you think of for the **contcatenate unique** function in DLRS?
