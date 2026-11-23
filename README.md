# sudoku-SAT-solver
A collection of executables that solve a sudoku puzzle. 

The `sud2sat` or `sud2sat1` converts a sudoku puzzle into a CNF formula. After feeding the formula to `minisat` (a SAT solver), `sat2sud`or `sat2sud1` then displays the solved sudoku puzzle visually.

## Contents
| File | Description |
|------|--------|
|`sud2sat`| Reads a Sudoku puzzle from STDIN and outputs a CNF description (in DIMACS format) to STDOUT using **minimal encodings**.|
|`sat2sud`| Reads output produced by `miniSAT` from STDIN,and outputs the solved sudoku puzzle to STDOUT using **minimal encodings**.|
|`sud2sat1`| Same as `sud2sat` using **extended encodings**.|
|`sat2sud1`| Same as `sat2sud` using **extended encodings**.|
| `report.pdf`| Background, implementation details, test results.
| `README.md`| This file

## Requirements

- **python3** : The program was written and tested using `Python 3.15.2` on macOS.
- **No compilation needed** : The programs are Python scripts starting with `#! /usr/bin/env python3`.
- **minisat**: which is a SAT solver, must be installed seperately on your OS. You can install it using homebrew by running `brew install minisat`.

If you get a `permission denied` error, make the scripts runnable by executing the following command in your terminal:
```bash
chmod +x sud2sat sat2sud sud2sat1 sat2sud1
```
## Usage
 
**Minimal encoding:**
 
```bash
# convert sudoku puzzle into a cnf formula
./sud2sat < puzzle.txt > puzzle.cnf
# minisat assigns a solution to the formula
minisat puzzle.cnf assign.txt > stat.txt
# convert formula to solved sudoku puzzle
./sat2sud < assign.txt > solution.txt
```
 
**Extended encoding:**
 
```bash
./sud2sat1 < puzzle.txt > puzzle.cnf
minisat puzzle.cnf assign.txt > stat.txt
./sat2sud1 < assign.txt > solution.txt
```
 
## Input / Output Format of Combined Executables
 
**Input:** 81 characters (`puzzle.txt`). Digits `1`-`9` are the entries from the sudoku puzzle. `0`, `.`, `*`, and `?` mean blank. Whitespace and newlines are ignored.
 
**Output:** 9 rows of 9 digits, grouped in threes (`solution.txt`).
 
 **Note:** after running ` minisat puzzle.cnf assign.txt > stat.txt`, 
 
 `assign.txt` will consist of one of the following options:
 1.  The first line containing "UNSAT", if the formula is unsatisfiable.
 2. The first line containing "SAT", and the second line containing a satisfiable assignment - a sequence of positive and negative variable numbers followed by 0. Positive numbers indicate the variable is assigned true. Negative numbers indicate that the variable is assigned false. 

 `stat.txt` will consist of various information and statistics about the execution of the command.
 
### Example
 
[Replace with real output from your program.]
 
```
$ ./sud2sat < puzzle.txt > puzzle.cnf
$ minisat puzzle.cnf assign.txt > stat.txt
$ ./sat2sud < assign.txt
672 145 398
145 983 672
...
```
 
## Implementation Notes
 
- Variable `(i, j, k)` (cell in row `i`, column `j` contains digit `k`) maps to `81(i-1) + 9(j-1) + (k-1) + 1`. `sat2sud` inverts this to recover row, column, and digit.
- [How the fixed rule clauses are generated or stored.]
- [How the clause count in the `p cnf` header is computed.]
- [Anything unusual you did.]
## Known Limitations
 
Inputs are limited to only Sudoku puzzles that:
- have only one solution.
- can be solved with only reasoning, i.e., with no search.
