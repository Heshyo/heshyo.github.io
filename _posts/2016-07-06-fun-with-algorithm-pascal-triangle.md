---
layout: post
title: "Fun with algorithm: Pascal's Triangle"
---

I saw a mention to the [Pascal's Triangle](https://en.wikipedia.org/wiki/Pascal%27s_triangle) not that long ago and thought of writing some code to solve it. It's a lot easier than the [8 queens puzzle](/2016/07/06/fun-with-algorithm-8-queens-puzzle.html) and can be done easily under 10 minutes. I wrote it in Chrome's snippet. The Javascript errors are not that great though. I forgot a semi colon inside a `for` loop, and all I got was `Uncaught SyntaxError: Unexpected identifier` on the next line. So it was a bit confusing. A real IDE would have shown me the error straight. Anyway, he're what I came up with.


```javascript

function compute(size) {
    size = parseInt(size);
    if (isNaN(size) || size < 1) {
        return null;
    }

    if (size === 1) {
        return [[1]];
    } else {
        var previousSolutions = compute(size -1);
        var previousSolution = previousSolutions[size - 2];
        var newSolution = [];

        // 1st element is always 1.
        newSolution.push(1);
        for(var i=1; i<size - 1; i++){
            newSolution.push(previousSolution[i - 1] + previousSolution[i])
        }
        // Last element is always 1.
        newSolution.push(1);

        previousSolutions.push(newSolution);
        return previousSolutions;
    }
}

console.table(compute(10));

```