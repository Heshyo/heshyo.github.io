---
layout: post
title: Why I love PHP...or maybe not.
---

I was refactoring some code and removed multiple (apparently) useless calls to a method that basically was doing a `new DateTime()`. The code was then doing some computations on those `DateTime`, like setting the time, adding some durations...

After my modifications, nothing was working correctly anymore. Fortunately a [comment](https://secure.php.net/manual/en/datetime.add.php#102193) on the `DateTime.add` method explained it all: yes, `add` and `sub` return a `DateTime`, but it's not a new one! Great, so now I start adding some `clone` until I realize there's a `DateTimeImmutable`. That sounds a lot more like what I would expect.

So I update the code to use `DateTimeImmutable`, but things are still not working correctly. I track the issue down to a call of `IntlDateFormatter::formatObject`. That function simply does not work on `DateTimeImmutable`

```php
// The following returns what you would expect
IntlDateFormatter::formatObject(new DateTime(), IntlDateFormatter::LONG, 'en');
// While the following returns false...
IntlDateFormatter::formatObject(new DateTimeImmutable(), IntlDateFormatter::LONG, 'en');
```

It would have made sense to me that `DateTime` and `DateTimeImmutable` be interchangeable, especially as they both implement `DateTimeInterface`:

> This class behaves the same as DateTime except it never modifies itself but returns a new object instead.

So now I need to convert my `DateTimeImmutable` to a `DateTime`. You have a `CreateFromMutable` but not the other way, so I guess I can just

```php
$dateTime = new DateTime();
$dateTime->setTimestamp($dateTimeImmutable->getTimestamp());
$dateTime->setTimezone($dateTimeImmutable->getTimezone());
```