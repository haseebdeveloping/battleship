# Battleship: Rip current edition

## Introduction

Battleship is essentially a guessing game between two players around
the premise of sea battle. In the traditional version of the game,
players first place their ships on a board. They keep the location of
these ships constant throughout game play, and a secret from their
opponent. As the game progresses, players make a series of guesses
in an attempt to identify ("hit") the placement of ships on their
opponents board. More information can be found on
[Wikipedia](https://en.wikipedia.org/wiki/Battleship_(game)), or from
the [television commercial](https://youtu.be/PrHs8CWDzmc).

The objective of this assignment is to implement that game, with a few
notable caveats:

1. Guessing is one-way: the only player to place ships is the
computer, and the only player to guess is the human.

2. Ships do not remain stationary, but instead move with a "current."

## The `layout` module

Before getting started it is worth mentioning the `layout`
module. This is code that has been provided to make your life easier,
and final code more generic (that is, easier to grade). Understanding
everything that is going on in the file is not important. How to use
what is going on in the file is.

The first thing your implementation should do is `import layout`. All
variables will be accessible from within your implementation by
prepending the name with `layout`; for example:
```python
import layout
print(layout.rows)
```
Rather than listing the different variables available in this module,
it is probably easiest to understand those variables in the context of
the implementation.

To acquire the module, right-click on the file name above ("layout.py")
and download it to your machine. When developing your implementation,
you will need to import the file; to do so successfully, layout.py
must be in the same directory as your implementation.

## Implementation (70 points)

The implementation has been broken down into parts. Each part has a
point value so that if you are unable to complete the entire
assignment, you know the (maximum amount of) partial credit to
expect. While it is possible to perform these tasks out of order and
complete the game, you might find doing so to be a bit harder.

### Making the board (10 / 70)

You should start by making the board. The board should be represented
using a list containing (sub)lists of strings. By default, the game is
played on a 10-by-10 grid, however your game should be size
agnostic. To help with this, use the `rows` and `columns` variables in
`layout`. To get an idea what those values are, you can print them:
```python
import layout
print(layout.rows, layout.columns)
```

Initially, the contents of the sub-lists should be a spaces. Again,
there is a variable in `layout` to help with this:
`layout.marker.water`. Setting the contents of the sub-lists to this
value, rather than the literal string ' ' is the suggested approach.

### Printing the board (10 / 70)

Next, develop code that prints the board using decorations. Each cell
should be surrounded by characters such that it appears to be a
box. For example, an empty 3-by-3 board would be printed as follows:
```
+-+-+-+
| | | |
+-+-+-+
| | | |
+-+-+-+
| | | |
+-+-+-+
```
Again, the `layout` module can help with the decorators:

|Variable|Value|
|--------|-----|
|`layout.board.side`   | `|`|
|`layout.board.top`    | `-`|
|`layout.board.corner` | `+`|

For example, rather than
```python
print('+-+')
```
you can use
```python
print(layout.board.corner + layout.board.top + layout.board.corner)
```
While this is more code, it has the advantage that if you want to
change the way the board looks, there is only one place---the value of
the variables in `layout`---that needs to be changed.

### Adding ships (10 / 70)

Populate the board with "ships." It is good enough that ships be
represented with the character 'x' (`layout.marker.ship`), while
places that have no ship be represented with a space
(`layout.marker.water`).

Traditional Battleship is played with five ships: an "aircraft
carrier," a "battleship," a "submarine," a "cruiser," and a
"destroyer." On the board, each ship can consume only a single line,
and cannot be placed diagonally; thus we can represent the placement
of each ship using its starting and ending coordinates. For example,
```python
aircraft_carrier = [ [2,1], [2,5] ]
```
would represent a ship (presumably the aircraft carrier) on the third
row of the board, placed horizontally starting in the second column
and ending at the sixth. For illustration, if we were playing on a
3-by-6 board, and the aircraft carrier---with the aforementioned
coordinates---were the only ship in play, our board would look
as follows:
```
+-+-+-+-+-+-+
| | | | | | |
+-+-+-+-+-+-+
| | | | | | |
+-+-+-+-+-+-+
| |x|x|x|x|x|
+-+-+-+-+-+-+
```
The `layout` module provides a `ships`, a data structure containing
all of the ship coordinates that need to be placed. `layout.ships` is
essentially a list containing a sequence of data types like
`aircraft_carrier` (above). For details, you can either examine the
definition of ships in layout.py or print the variable:
```python
print(layout.ships)
```
This variable should be iterated over when placing the ships on the
board. You can assume that ships will never be specified in such a way
as to require diagonal placement.

### Guessing (10 / 70)

Once the ships are placed, the game can commence. Your code should
repeatedly ask the user for the coordinates they would like to
target. The input should/will be in the form of "row-comma-column":
```
Enter your guess (row,col): 2,3
```
In this case the user has entered the coordinates "2,3". You can
assume the user is a computer scientist, and will know that the first
row and column are at zero, not one. You cannot assume that the user
will enter values nicely:
```
Enter your guess (row,col):     2   ,     3
```
You program should be able to deal with this.

Once you have the coordinates, you should then recognize whether a
ship is in that position. If it is, add a star (`layout.marker.hit`)
to your board in that position; if it is not, add an 'o'
(`layout.marker.miss`). You should then display your board followed by
the current score. In the 3-by-5 setting from above, we would have the
following interaction:
```
Enter your guess (x,y): 0,1
+-+-+-+-+-+-+
| |o| | | | |
+-+-+-+-+-+-+
| | | | | | |
+-+-+-+-+-+-+
| | | | | | |
+-+-+-+-+-+-+
Hits: 0, Misses: 1, Guesses: 1
Enter your guess (x,y): 2,3
+-+-+-+-+-+-+
| |o| | | | |
+-+-+-+-+-+-+
| | | | | | |
+-+-+-+-+-+-+
| | | |*| | |
+-+-+-+-+-+-+
Hits: 1, Misses: 1, Guesses: 2
```
You can assume that the user will not guess values that are outside of
the grid boundaries.

### Game over (10 / 70)

The game is over when all enemy ships have been hit. Each time a hit
has been made, you should check for this condition and leave the loop.

### Keeping stats (10 / 70)

Once the number of rounds has been exhausted, you should display the
users "hit" ratio as the final score. If the interaction above had a
single ship occupying a single cell, the game would be over and the
following would be displayed:
```
Final score: 0.5
```
(Since they had a single hit in two guesses.)

### Competition printing (5 / 70)

Before now, the board was being printed inclusive of enemy
ships. Whether to print the enemy ships should actually be dependent
on whether the game is in "debug" mode. That is, depending on the
Boolean value of `layout.competition`. If `layout.competition` is true
then enemy ships should be printed; if it is not, then enemy ships
should be replaced with spaces (`layout.marker.water`), as though they
were empty cells.

By default, `layout.competition` is false. However, to put your
program into competition printing mode, you should reset the value to
true:
```python
import layout
layout.competition = True
```
This should be done in your battleship code. Do not alter `layout`.

It is important to keep in mind that this is a printing detail, not an
augmentation to the board itself. That is, the contents of the board
should not change---this is really just a Boolean test as to whether
the actual contents of the board are printed, or something else.

Hits and misses in competition printing should continue to be
displayed as is.

### Water current (5 / 70)

In traditional Battleship the ships are stationary, but in the open
waters of Rip Current Edition, they are not. This means that between
guesses the ships move based on the waters current.

Specifically, assume you have a list of two integers (more on how you
will get that list later). The first integer specifies the movement of
all ships in the vertical direction; the second integer the movement
in the horizontal direction. Consider the following board with two
ships (ship labels are characters for clarity):
```
+-+-+-+-+-+-+
| |a|b|c| | |
+-+-+-+-+-+-+
| | | | | | |
+-+-+-+-+-+-+
| | | |d| | |
+-+-+-+-+-+-+
| | | | | | |
+-+-+-+-+-+-+
```
After a current of `[1, 1]` the layout would change:
```
+-+-+-+-+-+-+
| | | | | | |
+-+-+-+-+-+-+
| | |a|b|c| |
+-+-+-+-+-+-+
| | | | | | |
+-+-+-+-+-+-+
| | | | |d| |
+-+-+-+-+-+-+
```
Portions of ships that are at the edge of the board should "wrap
around" to the other side. Again, after a current of `[1, 1]`
```
+-+-+-+-+-+-+
| | | | |c|d|
+-+-+-+-+-+-+
| | |a| | | |
+-+-+-+-+-+-+
| | |b| | | |
+-+-+-+-+-+-+
```
ship positions would become
```
+-+-+-+-+-+-+
| | | |b| | |
+-+-+-+-+-+-+
|d| | | | |c|
+-+-+-+-+-+-+
| | | |a| | |
+-+-+-+-+-+-+
```

Note that a current shifts everything, even past user guesses (as they
become a part of the board once they are made). Thus, to avoid a new
guess appearing to move immediately, the move should take place in
your game-loop, before the board is printed and the user is asked for
a guess.

To obtain current information, you can use the `current` function in
the `layout` module. The function returns a list containing two random
integers:
```python
movement = layout.current()
```
In this case, `movement` might be the list `[1,-1]`.

## Expectations (30 points)

1. The code that you submit should run. Even if you have not
implemented all parts of the assignment, the subset of parts that are
implemented should be free of Python errors.

2. The `layout` module must be used when generating the board, when
placing ships, and when calculating the changing current. These values
will be altered when testing, and your code should work
regardless. For example, the values of `rows` or `columns` should not
matter; whether `current()` returns values between negative and
positive one should not matter; and the number of ships, along with
their respective lengths, should not matter.

3. If competition printing is attempted, the decision as to what should
be printed should be based on `layout.competition`. Failure to do so will
result in no credit for that portion.

4. There should be comments at the top of the program specifying your
name, and the purpose of the program. Additionally, there should be
comments throughout the code announcing which sub-problem this section
of code is implementing. If you use a concept that has not been
covered in class, something you found online perhaps, you should add a
comment explaining what is going on. Without an explanation, we can
only assume you have cheated.

## Submission

A single Python file should be submitted via NYU Classes, against the
corresponding homework assignment. There is no need to submit a
`layout` file.
