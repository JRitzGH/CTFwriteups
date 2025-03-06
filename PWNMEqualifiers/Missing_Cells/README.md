# Hidden Cells: PWNme Qualifiers 2025 (Misc Challenge)
## Given Description
My computer wanted to play sudoku, but it only knows binary.
It gave me a 16×16 grid of 0s and 1s, and asked me to fill in the missing cells.
Apparently, the solution will help me unmask a secret.

Can you help me solve it? I was told I should pay close attention to the format of the flag.

Flag format: `PWNME{.........................}`
### Attachments
- [missing_cells.zip](attachments/Missing_cells.zip)

### Author
- Mat

## [Link to the challenge](https://github.com/Phreaks-2600/PwnMeCTF-2025-quals/tree/main/Misc/Missing_cells)

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
Looking up a binairo solver online, I found [this one](https://binarypuzzle.nl/) that appeared to seem just right. Setting the grid to 16x16 and filling in the current numbers we are given provide us with this solution:

![First try solving - incomplete](https://github.com/JRitzGH/CTFwriteups/blob/main/docs/assets/images/Missing_cells_1.png)

Oh no! There's multiple solutions? Where to go from here?
## Solving the Binairo puzzle
Taking a look at the `secret.txt` file and using [CyberChef](https://gchq.github.io/CyberChef/). We can take the string `c56217e72f2ee27f0ec1f00f06956493ab02a2f8072159f3a27e79d399a4924f` from hex to binary and get: 
```
11000101 01100010 00010111 11100111
00101111 00101110 11100010 01111111
00001110 11000001 11110000 00001111
00000110 10010101 01100100 10010011
10101011 00000010 10100010 11111000
00000111 00100001 01011001 11110011
10100010 01111110 01111001 11010011
10011001 10100100 10010010 01001111
```
With a little bit of reorganizing and spacing, we get this complete 16x16 binary grid from `secret.txt`:
```
1 1 0 0 0 1 0 1 0 1 1 0 0 0 1 0 
0 0 0 1 0 1 1 1 1 1 1 0 0 1 1 1 
0 0 1 0 1 1 1 1 0 0 1 0 1 1 1 0 
1 1 1 0 0 0 1 0 0 1 1 1 1 1 1 1 
0 0 0 0 1 1 1 0 1 1 0 0 0 0 0 1 
1 1 1 1 0 0 0 0 0 0 0 0 1 1 1 1 
0 0 0 0 0 1 1 0 1 0 0 1 0 1 0 1 
0 1 1 0 0 1 0 0 1 0 0 1 0 0 1 1 
1 0 1 0 1 0 1 1 0 0 0 0 0 0 1 0 
1 0 1 0 0 0 1 0 1 1 1 1 1 0 0 0 
0 0 0 0 0 1 1 1 0 0 1 0 0 0 0 1 
0 1 0 1 1 0 0 1 1 1 1 1 0 0 1 1 
1 0 1 0 0 0 1 0 0 1 1 1 1 1 1 0 
0 1 1 1 1 0 0 1 1 1 0 1 0 0 1 1 
1 0 0 1 1 0 0 1 1 0 1 0 0 1 0 0 
1 0 0 1 0 0 1 0 0 1 0 0 1 1 1 1
```
This doesn't match up with ours, though, so what should we do?
### XOR
Typically, when comparing two identically-spaced binary strings, XOR can provide a solution. The case is applicable here.
The likely solution will result from using XOR on the binary grid we are solving with the grid resulting from `secret.txt`. Then turning that binary into text.
This is nice, as we can now use the last hint given
#### "I was told I should pay close attention to the format of the flag."
That's right! we know that the flag will being with `PWNME{` and end with `}`.
With this, we can start to build out a potential solution grid by looking at binary representations of these characters. This results in this grid:
```
0 1 0 1 0 0 0 0 0 1 0 1 0 1 1 1
0 1 0 0 1 1 1 0 0 1 0 0 1 1 0 1
0 1 0 0 0 1 0 1 0 1 1 1 1 0 1 1
. . . . . . . . . . . . . . . .
. . . . . . . . . . . . . . . .
. . . . . . . . . . . . . . . .
. . . . . . . . . . . . . . . .
. . . . . . . . . . . . . . . .
. . . . . . . . . . . . . . . .
. . . . . . . . . . . . . . . .
. . . . . . . . . . . . . . . .
. . . . . . . . . . . . . . . .
. . . . . . . . . . . . . . . .
. . . . . . . . . . . . . . . .
. . . . . . . . . . . . . . . .
. . . . . . . . 0 1 1 1 1 1 0 1
```
Pretty good start if you ask me!
Using the complete secret grid that we are given to XOR with our current solution, we can fill out spots on the grid for the incomplete puzzle we are solving!
Using the partially-solved grid given from the solver, XOR our current solution with the secret brings:
```
1 0 0 1 0 1 0 1 0 0 1 1 0 1 0 1
0 1 0 1 1 0 0 1 1 0 1 0 1 0 1 0
0 1 1 0 1 0 1 0 0 1 0 1 0 1 0 1
. . . 1 . . . . 0 . . 0 1 0 1 1
0 1 1 0 . 1 0 1 . . . 1 0 1 0 .
1 . . 0 . . . 0 . . 0 1 1 0 1 .
. . . 1 . . . 1 . 0 1 0 0 1 0 .
0 . 1 . . . . . . 1 0 0 1 1 0 0
. 1 0 0 1 . . . . 0 1 1 0 0 1 1
. . 0 . . . 0 . 1 . . . . 1 0 0
. . 1 . . 1 . . . 1 . . . . 1 .
0 1 1 0 . . . . . . . 0 . . 0 .
. . 0 . . . 1 . . . . . 1 0 1 1
. . . . . . . . . . . . . . . .
. . . . . . 0 . . 1 . . . . . 0
. . . . . . . . 0 0 1 1 0 0 1 0
```
Now, putting this into the same solver and clicking solves brings us this:

![Second try solving - incomplete](https://github.com/JRitzGH/CTFwriteups/blob/main/docs/assets/images/Missing_cells_2.png)

Uh oh! It seems like we're still not done. What other assumptions can we make?
## The Final Piece
Since the solution that we are attempting to create through XOR will result in a binary string. There are specific properties the binary must have in order to form ASCII text.
The biggest one? Each binary digit must begin with 0.
This means that, in our solution, the entire first and ninth column must be 0 (each line contains 2 characters).
This results in this grid:
```
0 1 0 1 0 0 0 0 0 1 0 1 0 1 1 1
0 1 0 0 1 1 1 0 0 1 0 0 1 1 0 1
0 1 0 0 0 1 0 1 0 1 1 1 1 0 1 1
0 . . . . . . . 0 . . . . . . .
0 . . . . . . . 0 . . . . . . .
0 . . . . . . . 0 . . . . . . .
0 . . . . . . . 0 . . . . . . .
0 . . . . . . . 0 . . . . . . .
0 . . . . . . . 0 . . . . . . .
0 . . . . . . . 0 . . . . . . .
0 . . . . . . . 0 . . . . . . .
0 . . . . . . . 0 . . . . . . .
0 . . . . . . . 0 . . . . . . .
0 . . . . . . . 0 . . . . . . .
0 . . . . . . . 0 . . . . . . .
0 . . . . . . . 0 1 1 1 1 1 0 1
```
And, XORing this and adding it to are in-progress grid gives us this.
```
1 0 0 1 0 1 0 1 0 0 1 1 0 1 0 1
0 1 0 1 1 0 0 1 1 0 1 0 1 0 1 0
0 1 1 0 1 0 1 0 0 1 0 1 0 1 0 1
1 0 0 1 0 1 . . 0 1 0 0 1 0 1 1
0 1 1 0 0 1 0 1 1 0 1 1 0 1 0 0
1 . . 0 1 0 1 0 0 1 0 1 1 0 1 0
0 . . 1 . . 0 1 1 0 1 0 0 1 0 1
0 . 1 . . . . . 1 1 0 0 1 1 0 0
1 1 0 0 1 . . . 0 0 1 1 0 0 1 1
1 . 0 . . . 0 . 1 . . 0 1 1 0 0
0 . 1 . . 1 . . 0 1 0 1 . 0 1 1
0 1 1 0 . . . . 1 . . 0 . 1 0 0
1 . 0 . . . 1 . 0 . . . 1 0 1 1
0 . . . . . . . 1 . . . . . . 1
1 . . . . . 0 . 1 1 . 0 . . . 0
1 . . . . . . 1 0 0 1 1 0 0 1 0
```
Now, putting this in the solver results in:

![Third try solving - complete!](https://github.com/JRitzGH/CTFwriteups/blob/main/docs/assets/images/Missing_cells_3.png)

Great! Now lets XOR This solved puzzle with our secret to get the solution. You can do this online, by hand, or with code.
I chose to do it with this python code:
```
def xor_binary_strings(bin_str1, bin_str2):
    if len(bin_str1) != len(bin_str2):
        raise ValueError("Binary strings must be of the same length")
    
    xor_result = ''.join(str(int(bit1) ^ int(bit2)) for bit1, bit2 in zip(bin_str1, bin_str2))
    
    return xor_result

binary1 = "1001010100110101010110011010101001101010010101011001011001001011011001011011010010101010010110100101100110100101001101101100110011001001001100111100110011001100001101100101001101101001101011001001001101001011001001101011010111001100110010101011001100110010"
binary2 = "1100010101100010000101111110011100101111001011101110001001111111000011101100000111110000000011110000011010010101011001001001001110101011000000101010001011111000000001110010000101011001111100111010001001111110011110011101001110011001101001001001001001001111"


result = xor_binary_strings(binary1, binary2)
print("XOR Result:", result)
```
This resulted in this solution grid:
```
0 1 0 1 0 0 0 0 0 1 0 1 0 1 1 1
0 1 0 0 1 1 1 0 0 1 0 0 1 1 0 1
0 1 0 0 0 1 0 1 0 1 1 1 1 0 1 1
0 1 1 1 0 1 0 0 0 0 1 1 0 1 0 0
0 1 1 0 1 0 1 1 0 1 1 1 0 1 0 1
0 1 0 1 1 0 1 0 0 1 0 1 0 1 0 1
0 1 0 1 1 1 1 1 0 0 1 1 0 0 0 0
0 1 0 1 0 0 1 0 0 1 0 1 1 1 1 1
0 1 1 0 0 0 1 0 0 0 1 1 0 0 0 1
0 1 1 0 1 1 1 0 0 0 1 1 0 1 0 0
0 0 1 1 0 0 0 1 0 1 1 1 0 0 1 0
0 0 1 1 0 0 0 0 0 1 0 1 1 1 1 1
0 0 1 1 0 0 0 1 0 0 1 1 0 1 0 1
0 1 0 1 1 1 1 1 0 1 1 0 0 1 1 0
0 1 0 1 0 1 0 1 0 1 1 0 1 1 1 0
0 0 1 0 0 0 0 1 0 1 1 1 1 1 0 1
```
This, when converted to text, created `PWNME{t4kuZU_0R_b1n41r0_15_fUn!}`
What a fun challenge!
