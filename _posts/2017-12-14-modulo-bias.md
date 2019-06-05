Say you have a function `rand(1,3)` that returns a random number 1, 2, or 3 and it does a good job and returns each number with a 1/3 probability.

Now lets say for some reason you only need the numbers 0 or 1.

If you want to use your `rand(1,3)` function you’ll have to make it do something with the number 3. As an engineer you may intuitively use the modulo operator like this:

`rand(1,3) % 2` to get the following results:

```
When rand(1,3) returns 1
1 % 2 = 1
```

```
When rand(1,3) returns 2
2 % 2 = 0
```
```
When rand(1,3) returns 3
3 % 2 = 1
```

Uh oh. Suddenly number 2 has a 2/3 probability and number 1 only has a 1/3 probability. Not an even probability of each number.

That is modulo bias.

The more generalized rule for our example is that for `rand(1,a) % n` *n* must be divisible into *a* with a remainder of zero or else there is a possibility of modulo bias. Put differently *a* mod *n* should = 0.

Here are other examples:

```
100 mod 50 = 0
rand(1,100) % 50 // even probability of each number
```

```
100 mod 52 = 48
rand(1,100) % 52 // not even probability of each number
```

You may be thinking that you’d never do this but imagine writing some code to pick a “random” one of 52 playing cards. It’s not unimaginable that you might write it with bias if you didn’t know better.

I would love to play at that casino.