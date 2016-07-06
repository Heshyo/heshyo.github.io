---
layout: post
title: "Fun with algorithm: 8 queens puzzle"
---

I remember my brother telling me about the [8 queens puzzle](https://en.wikipedia.org/wiki/Eight_queens_puzzle) when I was around 18. The goal is to put 8 queens on a 8x8 board, with no queen being able to take another one. It means 2 queens cannot be on the same row, nor on the same column, nor on the same diagonal.

At that time I just had an [HP48G](https://en.wikipedia.org/wiki/HP_48_series) calculator and I like playing with it to make small programs using the [RPL](https://en.wikipedia.org/wiki/RPL_(programming_language)). You can check the [full programming manual](http://www.transnull.com/files/hp48/hp48gaur.pdf). It was quite messy to even type the program on a small calculator with a small screen, and there was no debugging whatsoever. Still I somehow managed to write a working program. I let it run for I think 4 or 6 hours, and it found maybe 50 or 60 solutions out of 92. Now there's definitely some symmetry in the solutions, so I could easily have found the remaining ones. My brother used XL to solve the puzzle and got all the results at once.

I remembered it lately and thought of trying it in Javascript. I like Javascript as you can just press F12 in your browser and off you go, you can start typing. I like Chrome as you can easily save snippets. Similar things probably exists in other browsers.

Here's the solution I wrote in just under an hour, without too much distraction. Probably not the best way to do it, but it works. Here I simply `console.log` all the results. I could return an array of all the solutions, but it would require cloning the solutions to keep them, so an additional (small) function to make.

```javascript

(function() {
    var BoardSize = 8;
    var NumberOfSolutions = 0;

    function createBoard() {
        var board = [];
        for(var i=0; i<BoardSize; i++) {
            var row = [];
            for(var j=0; j<BoardSize; j++) {
                row.push(false);
            }
            board.push(row);
        }
        return board;
    }

    function compute() {
        walk(createBoard(), 0);
    }

    function walk(currentPositions, row) {
        for(var column=0; column<BoardSize; column++) {
            if (checkPosition(currentPositions, row, column)) {
                currentPositions[row][column] = true;
                if (row + 1 < BoardSize) {
                    // Continue to the next row.
                    walk(currentPositions, row + 1);
                } else {
                    // We're on the last row, we found a solution.
                    // Display it and continue.
                    display(currentPositions);
                }

                // Resets the current solution as we've already tried everything afterwards.
                currentPositions[row][column] = false;
            }
        }
    }

    function checkPosition(currentPositions, row, column) {
        // Check line (by design it's useless)

        // Check column
        for(var i=0; i<row; i++) {
            if (currentPositions[i][column]) {
                return false;
            }
        }

        // Check diagonal
        for(var col=0; col<BoardSize; col++) {
            if (col !== column) {
                var diff = Math.abs(column - col);
                var rowMinus = row - diff;
                if (rowMinus >= 0 && currentPositions[rowMinus][col]) {
                    return false;
                }

                var rowPlus = row + diff;
                if (rowPlus < BoardSize && currentPositions[rowPlus][col]) {
                    return false;
                }
            }
        }

        return true;
    }

    function display(solution) {
        NumberOfSolutions++;
        console.log("SOLUTION " + NumberOfSolutions);

        for(var i=0; i<BoardSize; i++) {
            var line = "";
            for(var j=0; j<BoardSize; j++) {
                line += solution[i][j] ? " X " : " _ ";
            }
            console.log(line);
        }

        console.log("--------------------------------");
    }

    compute();
})();

```