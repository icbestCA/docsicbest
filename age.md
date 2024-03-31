# age.icbest.ca
This website calculates your age with extreme precision, right down to the millisecond. You can see it in action [here](https://age.icbest.ca). If you’re interested in how it works or want to contribute, you can check out the code on [GitHub](https://github.com/icbestCA/age-timer).
### Values
**Input Date** You enter your birth date. The time is automatically set to midnight of that day. 
**Current Date** This is the current date and time, measured in milliseconds since the start of the [UNIX time](https://unixtime.org/) (January 1, 1970).
### Calculations

We begin by calculating the **milliseconds passed** by subtracting the `inputdate` from the `currenttime`.

Next, we determine the number of leap years that have occurred between the **inputdate** and **currentdate**.
```
currentdate - inputdate
```
 Leap years, which occur every four years, are easily calculated and stored in **leapYearsSinceDate**.

To find the total milliseconds, we subtract the leap days from the milliseconds passed, as leap days are not typically counted in age calculations:

```
millisecondstot = (millisecondsPassed - (86400000 * leapYearsSinceDate)) 
``` 
*86400000 being the number of milliseconds in a day.

The total years is calculated by dividing millisecondstot by the number of milliseconds in a year.
```
(1000 milliseconds * 60 seconds * 60 minutes * 24 hours * 365 days)
```
 The result is stored in ```years```.

To test the precision of age.icbest.ca, set your birth date to tomorrow. You’ll witness the counter increment by a year at midnight, showcasing the real-time accuracy of the site.