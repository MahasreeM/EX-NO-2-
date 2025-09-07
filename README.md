## EX. NO:2 IMPLEMENTATION OF PLAYFAIR CIPHER
### REG.NO:212224110035
### DATE:07-09-2025

## AIM:
 

 

To write a C program to implement the Playfair Substitution technique.

## DESCRIPTION:

The Playfair cipher starts with creating a key table. The key table is a 5×5 grid of letters that will act as the key for encrypting your plaintext. Each of the 25 letters must be unique and one letter of the alphabet is omitted from the table (as there are 25 spots and 26 letters in the alphabet).

To encrypt a message, one would break the message into digrams (groups of 2 letters) such that, for example, "HelloWorld" becomes "HE LL OW OR LD", and map them out on the key table. The two letters of the diagram are considered as the opposite corners of a rectangle in the key table. Note the relative position of the corners of this rectangle. Then apply the following 4 rules, in order, to each pair of letters in the plaintext:
1.	If both letters are the same (or only one letter is left), add an "X" after the first letter
2.	If the letters appear on the same row of your table, replace them with the letters to their immediate right respectively
3.	If the letters appear on the same column of your table, replace them with the letters immediately below respectively
4.	If the letters are not on the same row or column, replace them with the letters on the same row respectively but at the other pair of corners of the rectangle defined by the original pair.
## EXAMPLE:
![image](https://github.com/Hemamanigandan/EX-NO-2-/assets/149653568/e6858d4f-b122-42ba-acdb-db18ec2e9675)

 

## ALGORITHM:

STEP-1: Read the plain text from the user.

STEP-2: Read the keyword from the user.

STEP-3: Arrange the keyword without duplicates in a 5*5 matrix in the row order and fill the remaining cells with missed out letters in alphabetical order. Note that ‘i’ and ‘j’ takes the same cell.

STEP-4: Group the plain text in pairs and match the corresponding corner letters by forming a rectangular grid.

STEP-5: Display the obtained cipher text.




Program:
```#include <stdio.h>
#include <string.h>
#include <ctype.h>

#define SIZE 5

char keyTable[SIZE][SIZE];
int pos[26][2]; // store row,col for each letter

// Function to generate key table
void generateKeyTable(char key[]) {
    int used[26] = {0};
    int row = 0, col = 0;

    // Treat I and J as same
    used['J' - 'A'] = 1;

    // Insert key
    for (int i = 0; i < strlen(key); i++) {
        char c = toupper(key[i]);
        if (c < 'A' || c > 'Z') continue;
        if (c == 'J') c = 'I';
        if (!used[c - 'A']) {
            keyTable[row][col] = c;
            pos[c - 'A'][0] = row;
            pos[c - 'A'][1] = col;
            used[c - 'A'] = 1;
            col++;
            if (col == SIZE) { col = 0; row++; }
        }
    }

    // Fill remaining letters
    for (char c = 'A'; c <= 'Z'; c++) {
        if (!used[c - 'A']) {
            keyTable[row][col] = c;
            pos[c - 'A'][0] = row;
            pos[c - 'A'][1] = col;
            used[c - 'A'] = 1;
            col++;
            if (col == SIZE) { col = 0; row++; }
        }
    }
}

// Prepare plaintext into pairs
int prepareText(char text[], char pairs[][2]) {
    int n = strlen(text), k = 0;
    for (int i = 0; i < n; i++) {
        char c = toupper(text[i]);
        if (c < 'A' || c > 'Z') continue;
        if (c == 'J') c = 'I';

        if (k > 0 && pairs[k-1][0] == c && pairs[k-1][1] == '\0') {
            pairs[k-1][1] = 'X';
            i--;
            k++;
        } else {
            if (pairs[k][0] == '\0')
                pairs[k][0] = c;
            else {
                pairs[k][1] = c;
                k++;
            }
        }
    }
    // If odd length → add X
    if (pairs[k][0] != '\0' && pairs[k][1] == '\0') {
        pairs[k][1] = 'X';
        k++;
    }
    return k;
}

// Encrypt one pair
void encryptPair(char a, char b, char *x, char *y) {
    int r1 = pos[a - 'A'][0], c1 = pos[a - 'A'][1];
    int r2 = pos[b - 'A'][0], c2 = pos[b - 'A'][1];

    if (r1 == r2) { // Same row
        *x = keyTable[r1][(c1 + 1) % SIZE];
        *y = keyTable[r2][(c2 + 1) % SIZE];
    } else if (c1 == c2) { // Same column
        *x = keyTable[(r1 + 1) % SIZE][c1];
        *y = keyTable[(r2 + 1) % SIZE][c2];
    } else { // Rectangle
        *x = keyTable[r1][c2];
        *y = keyTable[r2][c1];
    }
}

int main() {
    char key[] = "MAHA SHREE";
    char text[] = "MAHA SHREE";
    char pairs[50][2] = {{0}};
    char encrypted[100];
    int pairCount, k = 0;

    // Generate key table
    generateKeyTable(key);

   

    // Prepare text
    pairCount = prepareText(text, pairs);

    printf("\nPairs:\n");
    for (int i = 0; i < pairCount; i++) {
        printf("%c%c ", pairs[i][0], pairs[i][1]);
    }

    // Encrypt
    printf("\n\nEncrypted Text: ");
    for (int i = 0; i < pairCount; i++) {
        char x, y;
        encryptPair(pairs[i][0], pairs[i][1], &x, &y);
        encrypted[k++] = x;
        encrypted[k++] = y;
    }
    encrypted[k] = '\0';
    printf("%s\n", encrypted);

    return 0;
}
```

Output:

<img width="739" height="229" alt="image" src="https://github.com/user-attachments/assets/22749535-e172-4de5-a4b6-2382f5c883b4" />

Result:

Thus the result was implemented successfully.
