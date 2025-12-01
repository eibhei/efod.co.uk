---
layout: post
title: "Advent Of Code 2025, Day 1"
date: 2025-12-01
tags: [code, aoc]
excerpt: "In which I (re)discover the modulus function"
image: /assets/images/post_aoc_25.1.jpg
---
For a time I was quite into participating in the [Advent of Code](https://adventofcode.com). For the uninitiated, AOC is an online Advent calendar in which participants are given short coding challenges. I don't claim to be a talented coder, but I like a puzzle. And my rudimentary Python (and less rudimentary Googling) skills are usually enough to bumble my way through.

I've never finished it before, but there are only 12 puzzles this year, which is around the point I start to feel burned out. So there's a good chance I can nail it this time! I thought I'd do a little write-up of each puzzle to add to my enjoyment.

## Day 1: Secret Entrance
[Advent of Code: Day 1](https://adventofcode.com/2025/day/1)

### Part One
I've been tasked with getting through a locked secret entrance. Frustratingly, the password has been changed, and I need to turn the safe dial on the combination lock in accordance with some instructions. Instead of landing on the correct number though, I am instead instructed to figure out how many times I land on the number 0 in the 4000+ lines of instruction. What fun!

```python
#! /usr/bin/env python3

dial_numbers = tuple(range(0,100))
zero_count = 0
dial_position = 50

def parse_line(string):
    if string[0] == "L":
        new_string = int(string[1:]) * -1
    if string[0] == "R":
        new_string = int(string[1:])
    return new_string

def rotate(position,number):
    current_position = dial_numbers.index(position)
    new_position = ( current_position + number ) % len(dial_numbers)

    return new_position

with open("input.txt","r") as input:
    for line in input:
        turn = parse_line(line)
        position = rotate(dial_position,turn)

        if position == 0:
            zero_count +=1

        dial_position = position

print(zero_count)
```

Here's my solution to the first part. I had some trouble figuring out how to work in a circle with that tuple `dial_numbers`. It turns out it's quite straightforward to do, but you need to use the modulus `%`, a feature which I _constantly_ forget the existence of (and was only reminded of thanks to a stint on Google). For my fellow amateurs, modulus is a funky form of division where you get the remainder. So `4 % 3 = 1`. It transpires we can use that same function to get the wrap-around index on our safe dial as well. I was skeptical that it would work for negative numbers though, but it does! I'm not fully sure I understand the reasoning behind it. Apparently it's because Python seeks a value between 0 and the denominator that, when added to a multiple of the denominator, gives the numerator. Who knew.

Anyway, this does the trick really nicely, and gives the correct answer according to the input given by the Advent of Code.

### Part 2
Oh damn, they've changed the algorithm! Instead of counting the number of times an instruction ends with me on 0, I have to count the number of times I pass through 0 as well.
```python
# identical to the above up until the with statement

with open("input.txt","r") as input:
    for line in input:
        turn = parse_line(line)
        t = 0
        if turn > 0:
            while t < turn:
                position = rotate(dial_position,1)
                if position == 0:
                    zero_count += 1
                t += 1
                dial_position = position
        if turn < 0:
            while t > turn:
                position = rotate(dial_position,-1)
                if position == 0:
                    zero_count += 1
                t -= 1
                dial_position = position

print(zero_count)
```

This one was much trickier (as is tradition), and I was scratching my head for how best to achieve it for a good while. I ended up in this position, which is about as inefficient as it could possibly be.
Essentially it figures out how far you need to move in either direction, moves once, and then checks if the current number is 0. It gets the right answer still, but I couldn't figure out how to make the rest of the maths work at all.

I had a vague idea about using greater/less than operations to figure out if the new position was less than the old one, while the number of moves was larger than the result, and... It didn't work. Or rather, it did exactly what I told it to, and spat out the wrong answer. I hadn't factored in the possibility of moving around the dial multiple times in one instruction. D'oh! That's resolved now, and here I am, sitting on my two stars!

So we ended up here. It's janky, and it would probably make a senior dev shudder, but it's mine.

```
❯ time ./part_2.py
$REDACTED$
./part_2.py  0.29s user 0.00s system 97% cpu 0.305 total
```

And hey, it doesn't take _too_ long to run :)