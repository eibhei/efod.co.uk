---
layout: post
title: "Advent Of Code 2025, Day 3"
date: 2025-12-03
tags: [python, aoc]
excerpt: "In which I write my first recursive function ever"
image: /assets/images/post_aoc_25.3.jpg
---

This is the third in a series of blog posts about the 2025 Advent of Code. For previous entries in this thrilling series, see
<a href="/tag/aoc">here</a>.

## Day 3: Lobby
[Advent of Code: Day 3](https://adventofcode.com/2025/day/3)

### Part One
Once again I am side tracked from my main quest of decorating the North Pole by an Elf asking for help fixing up the lifts. I need to restore power from the emergency battery banks. The banks are rendered as a string of numbers such as 3121910778619, with each number representing a battery and its power rating. I have to turn on exactly 2 batteries and provide the sum total of power provided by all the battery banks. The correct batteries in each bit of input form the highest two-digit number, read from left to right, where I can skip whatever numbers I want between the first digit and the last. So in 3121**9**1077861**9** the highest number I can make is 99. Note that if the string were 3121410775689, the highest possible number is 89: I cannot rearrange the numbers. I have not explained this that well, but it'll have to do. On to the code!



```python
#! /usr/bin/env python3

def get_joltage(magnitude,array,sum):
    if magnitude == 0:
        return sum
    else:
        lim = len(array) - magnitude + 1
        cut_array = array[:lim]

        max_val = max(cut_array)
        idx = cut_array.index(max_val)

        total = int(max_val) * 10**(magnitude - 1)

        sum += total
        array = array[(idx + 1):]

        return get_joltage(magnitude - 1, array, sum)

banks_list = []
joltage_sum = 0

with open("input.txt","r") as input:
    banks_list = input.read().splitlines()
    banks_list = (line for line in banks_list if line)

for bank in banks_list:
    joltage_sum += get_joltage(2,bank,0)

print(joltage_sum)
```

Initially I thought this could be achieved with some fancy regex, and it probably can. But I did regex yesterday, and I wanted to try something new today. I began by writing a whole bunch of very small functions, that would do things like grab the first instance of the largest number in the string, and then try to find the second largest number that followed it. It quickly became apparent that this was both impractical and (crucially) non-functional.

I also had a hunch that I'd need to be able to capture longer numbers for part 2. So I needed to find a way to make my code

1. More flexible than the hard-coded method I'd come up with, and
2. Efficiently use the same function for grabbing each digit.

As I mused, I remembered a coding class I'd attended years and years ago. They spoke about recursion, and at the time I fully didn't get it. But after reminding myself of what recursion was for (with a handy bit of ELI5 from an LLM), I realised that it was the perfect method. All I needed to do was find the repeatable set of actions needed to find the solution, which you see above. The critical part of this code is:

```python
def get_joltage(magnitude,array,sum):
    if magnitude == 0:
        return sum
    else:
        lim = len(array) - magnitude + 1
        cut_array = array[:lim]

        max_val = max(cut_array)
        idx = cut_array.index(max_val)

        total = int(max_val) * 10**(magnitude - 1)

        sum += total
        array = array[(idx + 1):]

        return get_joltage(magnitude - 1, array, sum)

```

I'll try to keep this brief, but in essence, we define a function called get_joltage, that needs three bits of input: the order of magnitude the solution needs to be in (`magnitude`), the string of numbers to work on (`array`), and the running total of the solution (`sum`).

The function first checks if `magnitude` is 0, and if so it concludes its run by returning the current value of `sum`. If `magnitude` is greater than zero, then we move onto the fun part.

First we find the last possible point in the array that we can begin from. Remember that we have to read left to right, and can't rearrange the numbers. Therefore, our tens digit can be anything up to and including the penultimate digit in the string. Once we have the cut-off point, we trim the array up to that part of the string.

Next, we find the highest single digit in the spliced string, and also make a note of exactly where in the list it appears. Then we make sure it is an integer, and multiply it by 10^`magnitude -1`. There is probably a less noddy way to do this.

Then we increase the running sum by the new value we've found, and cut the number string right after the number we found, creating a new one.

Finally we feed it all back into another run of the function, decreasing `magnitude` by one, and feeding in the new values for `array` and `sum`.

After that, we just need a `for` loop to run through each line of input (I added a line to strip any blank lines as my code freaked out over a blank line at the end of the input file). And then it spits out the answer in record time!

```
time ./part_1.py
$REDACTED$
./part_1.py  0.02s user 0.01s system 97% cpu 0.028 total
```

### Part 2
Scotty, we need more power! Now we need to turn on twelve batteries instead of two. My hunch was correct (although I had guessed that it would be three), and so the work I did to make my code flexible has massively paid dividends. As with yesterday, all we need is a single character to get the right answer:

`joltage_sum += get_joltage(2,bank,0)` becomes `joltage_sum += get_joltage(12,bank,0)`


```
❯ time ./part_2.py
$REDACTED$
./part_2.py  0.03s user 0.00s system 91% cpu 0.032 total
```

Honestly, I'm really proud of this one. It felt like a stretch, but completely within my grasp once I figured out the need for recursion. Incredibly satisfying to have it work.