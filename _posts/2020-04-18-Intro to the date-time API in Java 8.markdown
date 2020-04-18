---
layout: post
title:  "Introduction to the date-time API in Java 8"
excerpt: "Understand the different important time related entities in Java 8 and their use cases"
date:   2020-04-18 1:00:00 +0530 
author: Pavan Kalyan Damalapati
---

This post will cover some of the more frequently used Classes in Java 8's date time API.

### Introduction
In college, I learned about the Date class in Java, but in the Industry Java 8 is widely utilized and Java 8 does away with the outdated Date and Calendar class, and introduces a whole new set of classes which are thread-safe, better equipped to handle time-zones and easier to understand.

For my work at Hevo, I needed to utilize regular conversion of date time objects with different time-zones. I noticed myself regularly googling common conversions of different Java time classes and common doubts about the `java.time` package, and thought it would be worth it to spend a few hours to learn the proper usage of the `java.time` package.

### Instant (java.time.Instant)

Consider a timeline, with nanosecond precision starting from the standard Java epoch (1970-01-01). An Instant represents a point on this timeline. A positive value of an Instant means it lies on right side of the standard Java epoch and negative value means that it lies on the left side of the standard Java epoch. An Instant is always in UTC, and does not contain any information about timezones.

Some important ways to create Instants

~~~ java
//get Instant representing current time
Instant instant = Instant.now();


//get Instant from a long timestamp
instant = Instant.ofEpochMilli(1587160334000L);


//get Instant from only strings of format '2007-12-03T10:15:30.00Z'
instant = Instant.parse("2007-12-03T10:15:30.00Z");
~~~

Instants also provides methods for calculations.

~~~ java
Instant instant = Instant.now().plus(5, ChronoUnit.DAYS);

instant = instant.minus(Duration.ofDays(5));
~~~

ChronoUnit is an Enum with different Units like DAYS, SECONDS, MINUTES, etc.
but not all of them are supported by Instant calculations.

Instants are also comparable
~~~ java
Instant previousInstant = Instant.now().minus(5, ChronoUnit.DAYS);

Instant instant = Instant.now();

if(instant.isAfter(previousInstant)) {
	System.out.println("This 'instant' is after 'previousInstant' ");
}
~~~


### LocalDateTime (java.time.LocalDateTime)

LocalDateTime contains Date and time information. For example, this value `2nd October 2007 at 13:45.30.123456789` can be stored in a LocalDateTime object. LocalDateTime does not store or represent a time-zone.

Some ways to create a LocalDateTime object

~~~ java
//internally converts the datetime from an instant(UTC) to the system timezone.
LocalDateTime localDateTime = LocalDateTime.now();

//internally converts the datetime from an instant(UTC) to the timezone provided.
localDateTime = LocalDateTime.now(ZoneId.of("Asia/Kolkata"));

//It can also be construced with specific elements for example (year, month, day, hour, minute, second)
localDateTime = LocalDateTime.of(2020, 4, 18, 1, 1, 1);

//It can also be created from LocalDate and a LocalTime object
localDateTime = LocalDateTime.of(LocalDate.now(), LocalTime.now());
~~~

Note that although LocalDateTime.now() uses timezones internally, the returned LocalDateTime object has no context of timezones. It is simply a date and a time, like a birth date and time, which does not consider location of birth as a component. The distinction between Instant and LocalDateTime is wonderfully explained [here](https://stackoverflow.com/a/32443004/7470050).

### LocalDate (java.time.LocalDate)
LocalDate simply contains the date, without any context of timezones.

Some ways to create a LocalDate object

~~~ java
//internally converts the date from an instant(UTC) to the system timezone.
LocalDate localDate = LocalDate.now();

//internally converts the datetime from an instant(UTC) to the timezone provided.
localDate = LocalDate.now(ZoneId.of("Asia/Kolkata"));

//It can also be construced with specific elements for example (year, month, day)
localDate = LocalDate.of(2020, 4, 18);
~~~

### LocalTime (java.time.LocalTime)
LocalTime represents the time, without any context of time-zones.

Some ways to create a LocalTime object

~~~ java
//internally converts the date from an instant(UTC) to the system timezone.
LocalTime localTime = LocalTime.now();

//internally converts the datetime from an instant(UTC) to the timezone provided.
localTime = LocalTime.now(ZoneId.of("Asia/Kolkata"));

//It can also be construced with specific elements for example (hour, minute, second)
localTime = LocalTime.of(21, 1, 1);
~~~

### ZonedDateTime (java.time.ZonedDateTime)
ZonedDateTime represents the date, time with a time-zone.

Some ways to create ZonedDateTime object

~~~ java 
// get the current date-time and sets the time-zone as the system default offset.
ZonedDateTime zonedDateTime = ZonedDateTime.now();

// get the current date-time and sets the time-zone as the time-zone provided.
zonedDateTime = ZonedDateTime.now(ZoneId.of("Asia/Kolkata"));
~~~

**LocalDate, LocalTime and LocalDateTime all utilize similar comparison and calculation methods as those used by Instant.**

LocalDate, LocalTime, LocalDateTime and ZonedDateTime all can be parsed from strings.
All of the following methods use DateTimeFormatter class, internally(`parse(String text)`) or as a parameter(`parse(String text, DateTimeFormatter formatter)`).
~~~ java
//default formatter DateTimeFormatter.ISO_LOCAL_DATE ("yyyy-MM-dd")
LocalDate localDate = LocalDate.parse("2020-01-01");
localDate = LocalDate.parse("2020 01 01", DateTimeFormatter.ofPattern("yyyy MM dd"));

//default formatter DateTimeFormatter.ISO_LOCAL_TIME ("hh:mm:ss")
LocalTime localTime = LocalTime.parse("12:12:12");
localTime = LocalTime.parse("12 12 12", DateTimeFormatter.ofPattern("hh mm ss"));

//default formatter DateTimeFormatter.ISO_ZONED_DATE_TIME
LocalDate localDate = LocalDate.parse("2007-12-03T10:15:30+01:00[Europe/Paris]");
LocalDate localDate = LocalDate.parse("12.12.2020 12:12 GMT+02:00", DateTimeFormatter.ofPattern("dd.MM.yyyy HH:mm z"));
~~~

### Conversion between the different Classes

#### Direct Conversions

##### LocalDate -> LocalDateTime
~~~ java
LocalDate localDate = LocalDate.now();

//at start of day as the default time
LocalDateTime localDateTime = localDate.atStartOfDay();

//with the time provided
LocalDateTime localDateTime = localDate.atTime(LocalTime.now());

//with specific time components. For example, (hours, minutes, seconds)
LocalDateTime localDateTime = localDate.atTime(12, 12, 12);
~~~

##### LocalDate -> ZonedDateTime
~~~ java
LocalDate localDate = LocalDate.now();

//at start of day with timezone provided
ZonedDateTime zonedDateTime = localDate.atStartOfDay(ZoneId.of("Asia/Kolkata"));
~~~
This simply adds a timezone component, but it does not directly change the time component.

##### LocalTime -> LocalDateTime
~~~ java
LocalTime localTime = LocalTime.now();

//returns LocalDateTime with date specified
LocalDateTime localDateTime = localTime.atDate(LocalDate.now());
~~~

##### LocalDateTime -> ZonedDateTime
~~~ java
LocalDateTime localDateTime = LocalDateTime.now();

//at start of day with timezone provided
ZonedDateTime zonedDateTime = localDateTime.atZone(ZoneId.of("Asia/Kolkata"));
~~~

##### LocalDateTime -> LocalDate
~~~ java
LocalDateTime localDateTime = LocalDateTime.now();

//returns without any time component
LocalDate localDate = localDateTime.toLocalDate();
~~~

##### LocalDateTime -> LocalTime
~~~ java
LocalDateTime localDateTime = LocalDateTime.now();

//returns without any date component
LocalTime localTime = localDateTime.toLocalTime();
~~~

##### LocalDateTime -> Instant
~~~ java
LocalDateTime localDateTime = LocalDateTime.now();

//returns Instant
Instant instant = localDateTime.toInstant();
~~~

##### Instant -> LocalDateTime
~~~ java
//It can be created from an Instant, but first internally, the time-zone offset is applied to the instant to get LocalDate and LocalTime objecs
localDateTime = LocalDateTime.ofInstant(Instant.now(), ZoneId.of("Asia/Kolkata"));
~~~

##### Instant -> LocalDate (Java 9)
~~~ java
// in Java 9, there is direct method to convert betweeen Instant and LocalDate
localDate = LocalDate.ofInstant(Instant.now(), ZoneId.of("Asia/Kolkata"));
~~~

##### Instant -> LocalTime (Java 9)
~~~ java
// in Java 9, there is direct method to convert betweeen Instant and LocalDate
localTime = LocalTime.ofInstant(Instant.now(), ZoneId.of("Asia/Kolkata"));
~~~

##### Instant -> ZonedDateTime
~~~ java
//adds the offset to the instant and returns a new time(different from the instant time) with zone component.
ZonedDateTime zonedDateTime = Instant.atZone(ZoneId.of("Asia/Kolkata"));
~~~

##### ZonedDateTime -> Instant
~~~ java
//adds the offset to the instant and returns a new time(different from the old instant) with zone component.
ZonedDateTime zonedDateTime = ZonedDateTime.now();
Instant instant = zonedDateTime.toInstant();
~~~

##### ZonedDateTime -> LocalDateTime
~~~ java
ZonedDateTime zonedDateTime = ZonedDateTime.now();
LocalDateTime localDateTime = zonedDateTime.toLocalDateTime();
~~~

##### ZonedDateTime -> LocalDate
~~~ java
ZonedDateTime zonedDateTime = ZonedDateTime.now();
LocalDate localDate = zonedDateTime.toLocalDate();
~~~

##### ZonedDateTime -> LocalTime
~~~ java
ZonedDateTime zonedDateTime = ZonedDateTime.now();
LocalTime localTime = zonedDateTime.toLocalTime();
~~~

#### Some Useful Indirect Conversions

##### LocalDate -> Instant
~~~ java
LocalDate localDate = LocalDate.now();
LocalDateTime localDateTime = localDate.atStartOfDay();
Instant localDateTime.toInstant();
~~~

##### LocalTime -> Instant
~~~ java
LocalTime localTime = LocalTime.now();
LocalDateTime localDateTime = localTime.atDate(LocalDate.now());
Instant localDateTime.toInstant();
~~~

##### ZonedDateTime -> ZonedDateTime (changing time-zone without changing Instant)
~~~ java
ZonedDateTime zonedDateTime = ZonedDateTime.now().withZoneSameInstant(ZoneId.of("Asia/Kolkata"));
~~~

### References
- [Oracle Java 8 Docs](https://docs.oracle.com/javase/8/docs/api/java/time/package-summary.html)

- [Basil Bourque's Stack Overflow answer](https://stackoverflow.com/a/32443004/7470050)

If there are any issues with the content, please contact me by email(below) and let me know.