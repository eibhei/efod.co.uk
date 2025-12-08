---
layout: post
title: "Advent Of Code 2025, Day 6"
date: 2025-12-08
tags: [python, aoc]
excerpt: "In which I have to get help from a random Reddit thread"
image: /assets/images/post_aoc_25.5.jpg
---

This is the sixth in a series of blog posts about the 2025 Advent of Code. For previous entries in this thrilling series, see
<a href="/tag/aoc">here</a>. I didn't get any time over the weekend to attempt this, so it's now a game of catch-up.

The code is now up online and available for your reading pleasure! Check it out <a href="https://tea.odwyer.xyz/eibhlin/53.10_Advent-of-Code">here</a>.

## Day 5: Trash Compactor
[Advent of Code: Day 6](https://adventofcode.com/2025/day/6)

### Part One
I've slipped down the rubbish chute and into the tip. The door is locked, but a friendly family of slugs offer to unlock the door if I help the youngest with their maths homework. It seems straightforward enough:

```
123 328  51 64
 45 64  387 23
  6 98  215 314
*   +   *   +
```

Perform the relevant operation (addition or multiplication only) on the column, and sum the overall totals for the correct answer. I didn't struggle overmuch with this one,

```python
#! /usr/bin/env python3
import math

def do_maths(operation,values: list):
    if operation == "+":
        return sum(values)
    if operation == "*":
        return math.prod(values)

with open("input.txt","r") as homework:
    content = homework.read().splitlines()
    hw = []
    for line in content:
        hw.append(line.split())
    operations = hw[-1]
    values = hw[:-1]

    count = 0

    for i in range(0,len(operations)):
        operands = []
        for v in range(0,len(values)):
            operands.append(int(values[v][i]))
        count += do_maths(operations[i],operands)

    print(count)
```

This was fiddly, and I'm learning that my line-by-line approach to writing code is not very efficient. But essentially we're converting each row into a list, and then scanning left to right across all lists at the same time, and then performing each sum. Just the way a human would do it, really. It occurs to me on review, and after having completed part 2, that I could have gone about this in a much more programmatic way, generating the right variables in fewer lines of (admittedly more complex) code. Still, it gets the answer in 1/10th of a second:

```
time ./part_1.py
$REDACTED$
./part_1.py  0.01s user 0.00s system 94% cpu 0.016 total
```

### Part 2
Oh no, slugs do maths differently to humans! They list the sums in columns from right-to-left, where the highest significant digit is at the top. So

```
64
23
314
+
```
Actually translates to `4` + `431` + `623` = `1058`.

I was not remotely prepared for this, and I had not the slightest clue how to begin. I wasn't able to figure out how to use the whitespace to my advantage, and all hope felt lost. I opted to go to the subreddit and look through some of the submitted answers to try and get a feel for the solution. I found [this comment](https://www.reddit.com/r/adventofcode/comments/1pfguxk/comment/nsru7xw) which makes use of the `zip` function, which I had not heard of before.

`zip` enables you to combine multiple lists (or other iterable objects) into one zipped unit. In this case, I was able to combine each column of the input into its own list, thanks to a nifty `*`, which essentially rotates the data (I think, I'm still hazy on the details).

Then it's a matter of turning all that whitespace into useable information, here I convert each to a slash, and use double slashes to delimit where a number ends. Convert to integars and then perform the previous function to get the final answer:


```python
#! /usr/bin/env python3
import math

def do_maths(operation,values: list):
    if operation == "+":
        return sum(values)
    if operation == "*":
        return math.prod(values)

with open("input.txt","r") as homework:
    # ::-1 means to go from right to left
    values = [list(line.strip('\n'))[::-1] for line in homework]

    # Get a list of the operations
    operations = ''.join(values[-1]).split()

    # Use zip to work on all the lists at once, with * rotating the data
    zipped_values = zip(*values[:-1])

    # Strip out the white space
    values_list = [''.join(z).strip() for z in zipped_values]

    # Split into lists for each sum
    values_list = [val.split("/") for val in "/".join(values_list).split("//")]

    # Ensure they're integars
    operands = [list(map(int, vals)) for vals in values_list]

    count = 0
    for i in range(0,len(operands)):
        count += do_maths(operations[i],operands[i])

    print(count)
```

This is one of the larger changes I've had to make in the course of this challenge between part one and two. I made some pretty silly mistakes initially, and timed out my ability to enter answers to the Advent of Code submission page for a while. I had the bright idea to store each range as an array of 2: the lower bound and the upper bound. Then from there, I could simply take the difference between the upper and lower bounds and it would work!

Reader, it did not work. I was stuck for a very long time. What I hadn't appreciated was that there are ranges where the lower bound starts in the middle of another saved range. Completely useless to me, as I want the _unique_ number. So I wrote up some logic to sort the ranges, and then go through one by one and amend the values according to neighbours. This still did not work, and I had to figure out a way to merge the ranges together.

This was not pretty, and it took me a long time to realise that the way I'd set up my loop meant that removing entries in the array would break the code before I could get the answer. Finally I hit on going backwards through the list of ranges, safe in the knowledge that removing a range wouldn't break anything by deleting things that the program expected to check later on.

```
❯ time ./part_2.py
$REDACTED$
./part_2.py  0.02s user 0.00s system 96% cpu 0.027 total
```

Nice and quick again. I would have been absolutely sunk without /u/CDninja on Reddit, and would have definitely have had to have brute forced this had I not gone in search of clues!