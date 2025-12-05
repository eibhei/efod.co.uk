---
layout: post
title: "Advent Of Code 2025, Day 4"
date: 2025-12-04
tags: [python, aoc]
excerpt: "In which I remain on the struggle bus until 10pm"
image: /assets/images/post_aoc_25.4.jpg
---

This is the fourth in a series of blog posts about the 2025 Advent of Code. For previous entries in this thrilling series, see
<a href="/tag/aoc">here</a>.

## Day 4: Printing Department
[Advent of Code: Day 4](https://adventofcode.com/2025/day/4)

### Part One
This time I am asked with helping the Elves move some reams of wrapping paper around. I am presented with some data that looks like this and told that the @ symbols are reams of paper. I have to count the reams which have fewer than 4 other reams adjacent.

```
..@@.@@@@.
@@@.@.@.@@
@@@@@.@.@@
@.@@@@..@.
@@.@@@@.@@
.@@@@@@@.@
.@.@.@.@@@
@.@@@.@@@@
.@@@@@@@@.
@.@.@@@.@.
```

I'll level with you. I _really_ struggled on this one. I've battered my head against it for most of the day. Selecting a coordinate is easy enough, and checking the character is trivial. But accounting for edge cases, where there are no adjacent cells on one or two sides really threw me. For a long, long time. After I finally got it, I was so frazzled that putting together my loop was an effort in so much trial and error that I managed to crash my laptop! Anyway, the finished article is about as gorey as it can get. I'm having to keep reminding myself that it's not the elegance or efficiency that counts, but simply getting there.


```python
#! /usr/bin/env python3

grid = []

with open("input.txt","r") as input:
    grid = [list(line.strip()) for line in input]

x_max = len(grid)
y_max = len(grid[0])

def is_corner(x,y):
    if (x == 0 and y == 0):
        return True
    if (x == x_max-1 and y == 0):
        return True
    if (x == 0 and y == y_max-1):
        return True
    if (x == x_max-1 and y == y_max-1):
        return True
    else:
        return False

def check_cell(x,y):
    if grid[x][y] == "@":
        return True
    else:
        return False

def get_adj(x,y):
    adjacents = [
        [x-1, y-1], # position 0
        [x, y-1],   # position 1
        [x+1, y-1], # position 2
        [x-1, y],   # position 3
        [x+1, y],   # position 4
        [x-1, y+1], # position 5
        [x, y+1],   # position 6
        [x+1, y+1], # position 7
    ]
    if x == 0:
        removals = [5,3,0]
        for index in removals:
            del adjacents[index]
        return adjacents
    if x == x_max - 1:
        removals = [7,4,2]
        for index in removals:
            del adjacents[index]
        return adjacents
    if y == 0:
        removals = [2,1,0]
        for index in removals:
            del adjacents[index]
        return adjacents
    if y == y_max - 1:
        removals = [7,6,5]
        for index in removals:
            del adjacents[index]
        return adjacents
    return adjacents

papers_count = 0
for x in range(0,x_max):
    for y in range(0,y_max):
        if check_cell(x,y):
            if is_corner(x,y):
                papers_count+=1
            else:
                adjacents = get_adj(x,y)
                count = 0
                for cell in adjacents:
                    if check_cell(cell[0],cell[1]):
                        count +=1
                if count < 4:
                    papers_count += 1
print(papers_count)
```

Look, this is not pretty. There's probably a lot that doesn't need to be a function in there. But I'm still pleased with the outcome. It stores the maximum possible set of surrounding coordinates as a list, and then scrubs that list when x or y are at their minimums or maximums. There is almost certainly a more programmatic way to achieve this, and if I ever manage to face looking at this again, I'll come back and tidy it up.

Anyway, after that it's just a simple for loop for each coordinate in the grid, checking for a ream of paper and then counting the surroundings. Straightforward enough, and a run is relatively quick too:

```
time ./part_1.py
$REDACTED$
./part_1.py  0.03s user 0.00s system 98% cpu 0.030 total
```

### Part 2
Now we need to update the grid as we go, removing a ream every time we find one that has fewer than 4 adjacent reams. Then we run the process again until we can no longer remove from the grid. This _should_ have been straightforward, but by the time I got to it delerium was setting in and I hadn't blinked in whole minutes. After a long break I came back to it, and with some intensive Googling, I finally cracked it.

```python
...
def change_cell(x,y):
    grid[x][y] = "x"
...
papers_count = 0
previous_count = 0
while True:
    for x in range(0,x_max):
        for y in range(0,y_max):
            if check_cell(x,y):
                if is_corner(x,y):
                    papers_count+=1
                    previous_count+=1
                    change_cell(x,y)
                else:
                    adjacents = get_adj(x,y)
                    count = 0
                    for cell in adjacents:
                        if check_cell(cell[0],cell[1]):
                            count +=1
                    if count < 4:
                        papers_count += 1
                        previous_count+=1
                        change_cell(x,y)
    if previous_count == 0:
        break
    else:
        previous_count = 0
print(papers_count)
```

For something as simple as this, that change took me far longer than it should. I had a bunch of bugs staring me in the face, and I just wasn't seeing it. I actually got the correct answer before the code was correct: I ended up in an infinite loop where the correct answer was being repeatedly printed, but the code wasn't exiting. The reason being that I forgot that you can't just set the variable `False` to exit a `while True:` loop.

```
❯ time ./part_2.py
$REDACTED$
./part_2.py  0.41s user 0.01s system 99% cpu 0.415 total
```
Anyway, this version runs, and it's still in under a second! I saw some really awesome visualisations of this second half on Reddit, and I'm tempted to try and put one together some time.