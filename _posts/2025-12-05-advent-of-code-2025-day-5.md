---
layout: post
title: "Advent Of Code 2025, Day 5"
date: 2025-12-05
tags: [python, aoc]
excerpt: ""
image: /assets/images/post_aoc_25.5.jpg
---

This is the fifth in a series of blog posts about the 2025 Advent of Code. For previous entries in this thrilling series, see
<a href="/tag/aoc">here</a>.

The code is now up online and available for your reading pleasure! Check it out <a href="https://tea.odwyer.xyz/eibhlin/53.10_Advent-of-Code">here</a>.

## Day 5: Cafeteria
[Advent of Code: Day 5](https://adventofcode.com/2025/day/5)

### Part One
The Elves need my help in figuring out which ingredients are safe to use, and which are spoiled. They've given me a bunch of ingredient ID ranges, and then also a bunch of individual IDs. The ranges indicate fresh ingredients, and the individual IDs are the ones that need to be checked:

```
3-5
10-14
16-20
12-18

1
5
8
11
17
32
```

This one is a _really_ straightforward part one, to be honest. It's simply a case of storing the ranges (which in the real data are in the billions and trillions), and then going through the list of checking each available ID and checking if it's in the fresh ingredients range. I struggled a little on that logic to begin with, and started with a function that was way more complicated than it needed to be. What I actually did I can no longer recall, but it caused things to run for a _very_ long time, and eventually spat out the wrong answer.

```python
#!/usr/bin/env python3
def parse_range(input):
    bounds = []
    parsed = input.split("-")
    bound_lower = int(parsed[0])
    bound_upper = int(parsed[1])
    bounds.append(bound_lower)
    bounds.append(bound_upper)
    return bounds

with open("input.txt","r") as raw_input:
    raw_list = raw_input.read().splitlines()
    gap = raw_list.index("")
    fresh_list = []
    fresh_ranges_list = raw_list[:gap]

    available_list = raw_list[gap+1:]
    available_list = [int(i) for i in available_list]

    count = 0
    for i in available_list:
        for n in fresh_ranges_list:
            bounds = parse_range(n)
            lower = int(bounds[0])
            upper = int(bounds[1]) + 1
            if lower < i < upper:
                count+=1
                break
    print(count)
```

There are definitely some efficiencies to be gained here. I don't think it needs a function right at the top that is only called once (the eagle-eyed reader will note that this is an identical function to that used in [day 2]({% post_url 2025-12-02-advent-of-code-2025-day-2 %})), and I think I could import the data into two lists in a couple of lines of code rather than 6. Still, it works, and very rapidly!

```
time ./part_1.py
$REDACTED$
./part_1.py  0.05s user 0.02s system 98% cpu 0.069 total
```

### Part 2
Now we need to just count the number of fresh ingredients in total. This should be easy, but it's way more difficult than expected, due to the fact that the ranges are not unique, and have a lot of overlap (something I did not discover until much later). Given the sheer size of the ranges, it's completely impractical to read the range and save each number to a new list, and then count the number of unique items therein (I should know, I tried).

I eventually figured out that I could do it this way:

```python
#!/usr/bin/env python3
def parse_range(input):
    bounds = []
    parsed = input.split("-")
    bound_lower = int(parsed[0])
    bound_upper = int(parsed[1])
    bounds.append(bound_lower)
    bounds.append(bound_upper)
    return bounds

with open("input.txt","r") as raw_input:
    raw_list = raw_input.read().splitlines()
    gap = raw_list.index("")
    fresh_ranges_list = raw_list[:gap]

    fresh_list = []
    for n in fresh_ranges_list:
        bounds = parse_range(n)
        lower = int(bounds[0])
        upper = int(bounds[1])
        fresh_list.append([lower,upper])

    fresh_list.sort()
    fl = fresh_list

    for r in reversed(range(1,len(fl))):
        c_lo = fl[r][0]
        c_hi = fl[r][1]
        p_lo = fl[r-1][0]
        p_hi = fl[r-1][1]
        if p_lo <= c_lo <= p_hi and p_lo <= c_hi <= p_hi:
            del fl[r]
            continue
        if p_lo <= c_lo <= p_hi and c_hi >= p_hi:
            fl[r-1][1] = c_hi
            del fl[r]
            continue

    count = 0
    for r in fl:
        count += r[1] - r[0] +1
    print(count)
```

This is one of the larger changes I've had to make in the course of this challenge between part one and two. I made some pretty silly mistakes initially, and timed out my ability to enter answers to the Advent of Code submission page for a while. I had the bright idea to store each range as an array of 2: the lower bound and the upper bound. Then from there, I could simply take the difference between the upper and lower bounds and it would work!

Reader, it did not work. I was stuck for a very long time. What I hadn't appreciated was that there are ranges where the lower bound starts in the middle of another saved range. Completely useless to me, as I want the _unique_ number. So I wrote up some logic to sort the ranges, and then go through one by one and amend the values according to neighbours. This still did not work, and I had to figure out a way to merge the ranges together.

This was not pretty, and it took me a long time to realise that the way I'd set up my loop meant that removing entries in the array would break the code before I could get the answer. Finally I hit on going backwards through the list of ranges, safe in the knowledge that removing a range wouldn't break anything by deleting things that the program expected to check later on.

```
❯ time ./part_2.py
$REDACTED$
./part_2.py  0.02s user 0.00s system 96% cpu 0.026 total
```

It runs, and is very quick about it. I was so uncertain that I'd gotten the right answer (some absurdly large number) that I thought I'd misread the submission page when it said I was correct, twice.

On to day 6! It's the weekend tomorrow, and I will level with you that I've been managing to do much of this at my desk, where I tend to spend very little time on a weekend. Hopefully I'll be able to give the next couple of challenges a try, and not develop a backlog, but we'll just have to see.