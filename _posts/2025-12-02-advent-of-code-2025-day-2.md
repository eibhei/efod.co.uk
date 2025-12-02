---
layout: post
title: "Advent Of Code 2025, Day 2"
date: 2025-12-02
tags: [python, aoc]
excerpt: "In which I spend more time doing regex than Python"
image: /assets/images/post_aoc_25.2.jpg
---

This is the second in a series of blog posts about the 2025 Advent of Code. For previous entries in this thrilling series, see
<a href="/tag/aoc">here</a>.

## Day 2: Gift Shop
[Advent of Code: Day 2](https://adventofcode.com/2025/day/2)

### Part One
Much like in real life as a tech support worker, I have been tasked with fixing a "really small problem", while en route to do my intended job. The Elves need me to identify which gift shop product IDs are invalid. Simple. A product ID is just a number, and the invalid ones are where the number is just a sequence of digits repeated twice. For example, 55 is invalid, but 101 is valid.

```python
#! /usr/bin/env python3
import re

regex_rule = re.compile('^(\\d*)\\1$')
invalid_ids_sum = 0

with open("input.txt","r") as input:
    id_ranges = input.read().split(',')

def parse_input(input):
    bounds = []
    parsed = input.split("-")
    bound_lower = int(parsed[0])
    bound_upper = int(parsed[1])
    bounds.append(bound_lower)
    bounds.append(bound_upper)
    return bounds

def check_id(id):
    return regex_rule.match(str(id))

for id_range in id_ranges:
    bounds = parse_input(id_range)
    lower = int(bounds[0])
    upper = int(bounds[1]) + 1 # Add one to make range work later

    for id in range(lower,upper):
        if check_id(id):
            invalid_ids_sum += id

print(invalid_ids_sum)
```

After a bit more head scratching, I settled (reluctantly) on using regex for this. Regular expressions (regex for short) are a programmatic way of matching patterns in a given string. So you can write something like `'[a-z]'` which will match a single lowercase letter. You can start to get really fancy with it by adding operators such as `'*'`, `'+'`, and `'?'`; and you can also group matches together with brackets. It's a really powerful tool, and it's been around in its current, standardised form for about 50 years.

And I hate it. It's really picky, and extremely complicated to get write. I don't think I have ever, or will ever, write a regular expression correctly the first time. An enormous shout-out at this stage to [regex101](https://regex101.com/) without with I would not have been able to complete today's challenge.

It took a massive amount of trial and error, but finally I settled on the quite excellent `'^(\d*)\1$'`. The double quotes in the code above are a quirk of the way Python works, and not standard regex syntax.

There are a few parts to this: `^`, `(\d)`, `*`, `\1`, and `$`. The carat and dollar characters denote the beginning and end of the string respectively. `(\d)` denotes a single numeric digit, and inclusion of the asterisk matches zero or more. So zero or more numeric digits (0-9), starting at the beginning of the string. Then we have `\1` which matches the previous match exactly once, immediately after the first match, and then the end of the string (the dollar). So 5353 is a match because 53 appears twice in a row, but 53253 doesn't because there's a 2 in between. Smart eh?

Anyway, this took me most of the morning to figure out, with no small amount of under-breath cursing. But it worked! The rest of the script is pretty straightforward, we convert input in the format "22-99" into usable numbers (Python interprets that as a string of characters, not integars), and then iterate over all of the numbers in a given range running them against that regex.

A quick scrub over the subreddit for the Advent of Code talks about more efficient ways of doing this, but I couldn't really follow them to be quite honest. Anyway, we get our correct answer in under a second:

```
$REDACTED$
./part_1.py  0.67s user 0.01s system 98% cpu 0.688 total
```

### Part 2
Wait! There are actually more invalid IDs! If the same sequence of digits is repeated _at least_ twice, then it's invalid. This... This was incredible. The Advent of Code always requires you to make small adjustments to your code to complete the second part, but this might be the smallest yet:

`'^(\d*)\1$'` becomes `'^(\d*)\1+$'`

A single character is all we needed to complete part 2! Adding the `+` after `\1` tells regex to look for one or more of the preceeding matching criteria. So now 535353 is an invalid ID that is successfully caught. I confess to feeling _extremely_ smug when I figured out that it was the difference of one character.


```
❯ time ./part_2.py
$REDACTED$
./part_2.py  0.96s user 0.00s system 98% cpu 0.973 total
```

Again, under a second. I'm sure there are significant efficiencies to be gained here, but again, I'm just looking to complete the challenge. More tomorrow.