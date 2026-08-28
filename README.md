## EX. NO:2 IMPLEMENTATION OF PLAYFAIR CIPHER

 # Name: Shrenidhi
 # Reg No: 212223040196
 # Dept : CSE

## AIM: To write a C program to implement the Playfair Substitution technique.

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




# Program:

```
#include <stdio.h>
#include <string.h>
#include <ctype.h>

char keyTable[5][5];

// Generate the 5x5 key table
void generateKeyTable(char key[]) {
    int used[26] = {0};
    int i, j = 0, k = 0;

    used['j' - 'a'] = 1; // Treat J as I

    // Fill key letters
    for (i = 0; key[i] != '\0'; i++) {
        char ch = tolower(key[i]);

        if (ch == 'j')
            ch = 'i';

        if (!used[ch - 'a']) {
            keyTable[j][k] = ch;
            used[ch - 'a'] = 1;
            k++;
            if (k == 5) {
                j++;
                k = 0;
            }
        }
    }

    // Fill remaining alphabet
    for (i = 0; i < 26; i++) {
        if (!used[i]) {
            keyTable[j][k] = i + 'a';
            k++;
            if (k == 5) {
                j++;
                k = 0;
            }
        }
    }
}

// Display key table
void displayKeyTable() {
    int i, j;
    printf("\nKey Table:\n");
    for (i = 0; i < 5; i++) {
        for (j = 0; j < 5; j++) {
            printf("%c ", toupper(keyTable[i][j]));
        }
        printf("\n");
    }
}

// Find position of a letter
void findPosition(char ch, int *row, int *col) {
    if (ch == 'j')
        ch = 'i';

    for (int i = 0; i < 5; i++) {
        for (int j = 0; j < 5; j++) {
            if (keyTable[i][j] == ch) {
                *row = i;
                *col = j;
                return;
            }
        }
    }
}

// Prepare plaintext
void prepareText(char text[]) {
    char temp[100];
    int i, j = 0;

    // Remove spaces and convert to lowercase
    for (i = 0; text[i] != '\0'; i++) {
        if (text[i] != ' ') {
            char ch = tolower(text[i]);
            if (ch == 'j')
                ch = 'i';
            temp[j++] = ch;
        }
    }
    temp[j] = '\0';

    // Insert X between repeated letters
    char result[100];
    int k = 0;

    for (i = 0; i < j; i++) {
        result[k++] = temp[i];

        if (i + 1 < j && temp[i] == temp[i + 1]) {
            result[k++] = 'x';
        }
    }

    result[k] = '\0';

    // Add Z if odd length
    if (k % 2 != 0) {
        result[k++] = 'z';
        result[k] = '\0';
    }

    strcpy(text, result);
}

// Encrypt
void encrypt(char text[]) {
    int i;

    printf("\nCipher Text: ");

    for (i = 0; text[i] != '\0'; i += 2) {

        int r1, c1, r2, c2;

        findPosition(text[i], &r1, &c1);
        findPosition(text[i + 1], &r2, &c2);

        if (r1 == r2) {
            printf("%c%c",
                   toupper(keyTable[r1][(c1 + 1) % 5]),
                   toupper(keyTable[r2][(c2 + 1) % 5]));
        }
        else if (c1 == c2) {
            printf("%c%c",
                   toupper(keyTable[(r1 + 1) % 5][c1]),
                   toupper(keyTable[(r2 + 1) % 5][c2]));
        }
        else {
            printf("%c%c",
                   toupper(keyTable[r1][c2]),
                   toupper(keyTable[r2][c1]));
        }
    }

    printf("\n");
}

int main() {

    char key[100];
    char plaintext[100];

    printf("Enter Key: ");
    scanf("%s", key);

    printf("Enter Plain Text: ");
    scanf("%s", plaintext);

    generateKeyTable(key);

    displayKeyTable();

    prepareText(plaintext);

    printf("\nPrepared Plain Text: %s\n", plaintext);

    encrypt(plaintext);

    return 0;
}

```





## Output:

<img width="544" height="410" alt="image" src="https://github.com/user-attachments/assets/7360ae13-58ac-4d4e-ad24-719544cd4889" />

## RESULT

Thus, the Playfair Cipher algorithm was successfully implemented using C language.

