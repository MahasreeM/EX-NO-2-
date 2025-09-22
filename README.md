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
```
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#define SIZE 30

void toLowerCase(char plain[], int ps) {
  for (int i = 0; i < ps; i++) {
      if (plain[i] >= 'A' && plain[i] <= 'Z')
          plain[i] += 32;
  }
}

int removeSpaces(char *plain, int ps) {
  int i, count = 0;
  for (i = 0; i < ps; i++) {
      if (plain[i] != ' ')
          plain[count++] = plain[i];
  }
  plain[count] = '\0';
  return count;
}

void generateKeyTable(char key[], int ks, char keyT[5][5]) {
  int i, j, k;
  int dicty[26] = {0};
  for (i = 0; i < ks; i++) {
      if (key[i] != 'j')
          dicty[key[i] - 'a'] = 2;
  }
  dicty['j' - 'a'] = 1;
  i = 0; j = 0;
  for (k = 0; k < ks; k++) {
      if (dicty[key[k] - 'a'] == 2) {
          dicty[key[k] - 'a'] -= 1;
          keyT[i][j] = key[k];
          j++;
          if (j == 5) {
              i++;
              j = 0;
          }
      }
  }
  for (k = 0; k < 26; k++) {
      if (dicty[k] == 0 && (char)(k + 'a') != 'j') {
          keyT[i][j] = (char)(k + 'a');
          j++;
          if (j == 5) {
              i++;
              j = 0;
          }
      }
  }
}

void search(char keyT[5][5], char a, char b, int arr[]) {
  int i, j;
  if (a == 'j') a = 'i';
  if (b == 'j') b = 'i';
  for (i = 0; i < 5; i++) {
      for (j = 0; j < 5; j++) {
          if (keyT[i][j] == a) {
              arr[0] = i;
              arr[1] = j;
          } else if (keyT[i][j] == b) {
              arr[2] = i;
              arr[3] = j;
          }
      }
  }
}

int mod5(int a) {
  return (a % 5 + 5) % 5;
}

int prepare(char str[], int ptrs) {
  if (ptrs % 2 != 0) {
      str[ptrs++] = 'z';
      str[ptrs] = '\0';
  }
  return ptrs;
}

void encrypt(char str[], char keyT[5][5], int ps) {
  int i, a[4];
  for (i = 0; i < ps; i += 2) {
      search(keyT, str[i], str[i + 1], a);
      if (a[0] == a[2]) {
          str[i] = keyT[a[0]][mod5(a[1] + 1)];
          str[i + 1] = keyT[a[2]][mod5(a[3] + 1)];
      } else if (a[1] == a[3]) {
          str[i] = keyT[mod5(a[0] + 1)][a[1]];
          str[i + 1] = keyT[mod5(a[2] + 1)][a[3]];
      } else {
          str[i] = keyT[a[0]][a[3]];
          str[i + 1] = keyT[a[2]][a[1]];
      }
  }
}

void decrypt(char str[], char keyT[5][5], int ps) {
  int i, a[4];
  for (i = 0; i < ps; i += 2) {
      search(keyT, str[i], str[i + 1], a);
      if (a[0] == a[2]) {
          str[i] = keyT[a[0]][mod5(a[1] - 1)];
          str[i + 1] = keyT[a[2]][mod5(a[3] - 1)];
      } else if (a[1] == a[3]) {
          str[i] = keyT[mod5(a[0] - 1)][a[1]];
          str[i + 1] = keyT[mod5(a[2] - 1)][a[3]];
      } else {
          str[i] = keyT[a[0]][a[3]];
          str[i + 1] = keyT[a[2]][a[1]];
      }
  }
}

void encryptByPlayfairCipher(char str[], char key[]) {
  int ps, ks;
  char keyT[5][5];
  ks = strlen(key);
  ks = removeSpaces(key, ks);
  toLowerCase(key, ks);
  ps = strlen(str);
  toLowerCase(str, ps);
  ps = removeSpaces(str, ps);
  ps = prepare(str, ps);
  generateKeyTable(key, ks, keyT);
  encrypt(str, keyT, ps);
}

void decryptByPlayfairCipher(char str[], char key[]) {
  int ps, ks;
  char keyT[5][5];
  ks = strlen(key);
  ks = removeSpaces(key, ks);
  toLowerCase(key, ks);
  ps = strlen(str);
  toLowerCase(str, ps);
  ps = removeSpaces(str, ps);
  generateKeyTable(key, ks, keyT);
  decrypt(str, keyT, ps);
}

int main() {
  char str[SIZE], key[SIZE];
  printf("Playfair Cipher\n");
  strcpy(key, "CRYPTOGRAPHY");
  printf("Key text: %s\n", key);
  strcpy(str, "Elaiyavan");
  printf("Plain text: %s\n", str);
  encryptByPlayfairCipher(str, key);
  printf("Cipher text: %s\n", str);
  decryptByPlayfairCipher(str, key);
  printf("Decrypted text: %s\n", str);
  return 0;
}

```

Output:

<img width="806" height="422" alt="image" src="https://github.com/user-attachments/assets/d46e907d-6b76-4106-abd1-818d35dc59aa" />


Result:

Thus the result was implemented successfully.
