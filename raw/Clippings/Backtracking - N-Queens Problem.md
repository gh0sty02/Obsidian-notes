---
title: "Backtracking - N-Queens Problem"
source: "https://algorithm-visualizer.org/backtracking/n-queens-problem"
author:
published:
created: 2026-06-16
description: "The eight queens puzzle is the problem of placing eight chess queens on an 8×8 chessboard so that no two queens threaten each other. Thus, a solution requires that no two queens share the same row, column, or diagonal. The eight queens puzzle is an example of the more general n queens problem of placing n non-attacking queens on an n×n chessboard, for which solutions exist for all natural numbers n with the exception of n=2 and n=3."
tags:
  - "clippings"
---
README.md

// import visualization libraries {...}

const N = 4; // just change the value of N and the visuals will reflect the configuration!

const board = (function createArray(N) {

const result = \[\];

for (let i = 0; i < N; i++) {

result\[i\] = Array(...Array(N)).map(Number.prototype.valueOf, 0);

}

return result;

}(N));

const queens = (function qSetup(N) {

const result = \[\];

for (let i = 0; i < N; i++) {

result\[i\] = \[-1, -1\];

}

return result;

}(N));

// define tracer variables {...}

function validState(row, col, currentQueen) {

for (let q = 0; q < currentQueen; q++) {

const currentQ = queens\[q\];

if (row === currentQ\[0\] || col === currentQ\[1\] || (Math.abs(currentQ\[0\] - row) === Math.abs(currentQ\[1\] - col))) {

return false;

}

}

return true;

}

function nQ(currentQueen, currentCol) {

// logger {...}

if (currentQueen >= N) {

// logger {...}

return true;

}

let found = false;

let row = 0;

while ((row < N) && (!found)) {

// visualize {...}

if (validState(row, currentCol, currentQueen)) {

queens\[currentQueen\]\[0\] = row;

queens\[currentQueen\]\[1\] = currentCol;

// visualize {...}

found = nQ(currentQueen + 1, currentCol + 1);

}

if (!found) {

// visualize {...}

}

row++;

}

return found;

}

// logger {...}

nQ(0, 0);

// logger {...}