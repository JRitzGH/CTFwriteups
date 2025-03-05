# Hidden Cells: PWNme Qualifiers (Misc Challenge)
## Given Description
My computer wanted to play sudoku, but it only knows binary.
It gave me a 16×16 grid of 0s and 1s, and asked me to fill in the missing cells.
Apparently, the solution will help me unmask a secret.

Can you help me solve it? I was told I should pay close attention to the format of the flag.

Flag format: `PWNME{.........................}`
### Attachments
- [missing_cells.zip](attachments/missing_cells.zip)

### Author
- Mat

## Analysis

Reading the description, it mentions a binary version of soduko. A quick google search provides us with [this](https://en.wikipedia.org/wiki/Takuzu) Wikipedia article about this game, known as either Takuzu, Binario, or others.
The attached zip file, when extracted, gives us two pieces of information. `secret.txt` and `grid.txt`, the first of which contains `c56217e72f2ee27f0ec1f00f06956493ab02a2f8072159f3a27e79d399a4924f`, a hex string, whereas the latter contains the following incomplete binary grid.
```
. . . . . . . . . . . . . . . .
. . . . . . . . . . . . . . . .
. . . . . . . . . . . . . . . .
. . . . . . . . 0 . . . . . 1 1
. 1 1 . . 1 . 1 . . . 1 . . . .
1 . . 0 . . . . . . . 1 . . . .
. . . . . . . 1 . 0 . . 0 . 0 .
0 . . . . . . . . . . . . . 0 0
. . 0 0 . . . . . . 1 1 . . . .
. . . . . . 0 . 1 . . . . . . 0
. . 1 . . 1 . . . 1 . . . . 1 .
. 1 1 . . . . . . . . 0 . . . .
. . . . . . 1 . . . . . 1 . 1 1
. . . . . . . . . . . . . . . .
. . . . . . 0 . . 1 . . . . . 0
. . . . . . . . . . . . . . . . 
```
Looking up a binairo solver online, I found [this one](https://binarypuzzle.nl/) that appeared to seem just right. Setting the grid to 16x16 and filling in the current numbers we are given provide us with this.

