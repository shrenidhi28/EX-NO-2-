**EX. NO: 2: IMPLEMENTATION OF PLAYFAIR CIPHER**

**Name:** Shrenidhi

**Reg No:** 212223040196

**Dept:** CSE

**AIM:**

To write a C program to implement the Playfair Substitution technique.

**DESCRIPTION:**

The Playfair Cipher starts by creating a key table. The key table is a 5×5 grid of letters that acts as the key for encrypting the plaintext. Each of the 25 letters must be unique, and one letter of the alphabet is omitted from the table because there are only 25 positions.

To encrypt a message, the plaintext is divided into digrams (groups of two letters). For example, **HELLOWORLD** becomes **HE LX LO WO RL DX** after handling repeated letters and padding where necessary.

The following rules are applied to each pair of letters:

1. If both letters are the same, insert an **X** after the first letter.
2. If both letters appear in the same row of the table, replace them with the letters immediately to their right.
3. If both letters appear in the same column of the table, replace them with the letters immediately below them.
4. If the letters are in different rows and columns, replace them with the letters in the same row but at the opposite corners of the rectangle formed by the original pair.

**EXAMPLE:**

![image](https://github.com/Hemamanigandan/EX-NO-2-/assets/149653568/e6858d4f-b122-42ba-acdb-db18ec2e9675)

**ALGORITHM:**

**STEP 1:** Read the plain text from the user.

**STEP 2:** Read the keyword from the user.

**STEP 3:** Arrange the keyword without duplicates in a 5×5 matrix in row order and fill the remaining cells with the unused letters in alphabetical order. Note that **I** and **J** occupy the same cell.

**STEP 4:** Group the plain text into pairs and match the corresponding letters using the Playfair Cipher rules.

**STEP 5:** Display the obtained cipher text.

**PROGRAM:**

```c
#include <stdio.h>
#include <string.h>
#include <ctype.h>

char keyTable[5][5];

// Generate 5x5 key table
void generateKeyTable(char key[])
{
    int used[26] = {0};
    int i, j = 0, k = 0;

    used['j' - 'a'] = 1;   // Treat J as I

    // Fill key letters
    for (i = 0; key[i] != '\0'; i++)
    {
        char ch = tolower(key[i]);

        if (ch == 'j')
            ch = 'i';

        if (ch >= 'a' && ch <= 'z' && !used[ch - 'a'])
        {
            keyTable[j][k] = ch;
            used[ch - 'a'] = 1;

            k++;
            if (k == 5)
            {
                k = 0;
                j++;
            }
        }
    }

    // Fill remaining alphabet
    for (i = 0; i < 26; i++)
    {
        if (!used[i])
        {
            keyTable[j][k] = i + 'a';

            k++;
            if (k == 5)
            {
                k = 0;
                j++;
            }
        }
    }
}

// Display key table
void displayKeyTable()
{
    int i, j;

    printf("\nKey Table:\n");

    for (i = 0; i < 5; i++)
    {
        for (j = 0; j < 5; j++)
        {
            printf("%c ", toupper(keyTable[i][j]));
        }
        printf("\n");
    }
}

// Find position of character
void findPosition(char ch, int *row, int *col)
{
    if (ch == 'j')
        ch = 'i';

    for (int i = 0; i < 5; i++)
    {
        for (int j = 0; j < 5; j++)
        {
            if (keyTable[i][j] == ch)
            {
                *row = i;
                *col = j;
                return;
            }
        }
    }
}

// Prepare plaintext
void prepareText(char text[])
{
    char temp[200];
    char result[200];
    int i, j = 0, k = 0;

    // Remove spaces and convert to lowercase
    for (i = 0; text[i] != '\0'; i++)
    {
        if (isalpha(text[i]))
        {
            char ch = tolower(text[i]);

            if (ch == 'j')
                ch = 'i';

            temp[j++] = ch;
        }
    }

    temp[j] = '\0';

    // Create pairs
    for (i = 0; i < j;)
    {
        result[k++] = temp[i];

        if (i + 1 < j)
        {
            if (temp[i] == temp[i + 1])
            {
                result[k++] = 'x';
                i++;
            }
            else
            {
                result[k++] = temp[i + 1];
                i += 2;
            }
        }
        else
        {
            result[k++] = 'x';
            i++;
        }
    }

    result[k] = '\0';

    strcpy(text, result);
}

// Encrypt
void encrypt(char text[])
{
    int i;

    printf("\nCipher Text: ");

    for (i = 0; text[i] != '\0'; i += 2)
    {
        int r1, c1, r2, c2;

        findPosition(text[i], &r1, &c1);
        findPosition(text[i + 1], &r2, &c2);

        if (r1 == r2)
        {
            printf("%c%c",
                   toupper(keyTable[r1][(c1 + 1) % 5]),
                   toupper(keyTable[r2][(c2 + 1) % 5]));
        }
        else if (c1 == c2)
        {
            printf("%c%c",
                   toupper(keyTable[(r1 + 1) % 5][c1]),
                   toupper(keyTable[(r2 + 1) % 5][c2]));
        }
        else
        {
            printf("%c%c",
                   toupper(keyTable[r1][c2]),
                   toupper(keyTable[r2][c1]));
        }
    }

    printf("\n");
}

// Decrypt
void decrypt(char text[])
{
    int i;

    printf("Decrypted Text: ");

    for (i = 0; text[i] != '\0'; i += 2)
    {
        int r1, c1, r2, c2;

        findPosition(tolower(text[i]), &r1, &c1);
        findPosition(tolower(text[i + 1]), &r2, &c2);

        if (r1 == r2)
        {
            printf("%c%c",
                   toupper(keyTable[r1][(c1 + 4) % 5]),
                   toupper(keyTable[r2][(c2 + 4) % 5]));
        }
        else if (c1 == c2)
        {
            printf("%c%c",
                   toupper(keyTable[(r1 + 4) % 5][c1]),
                   toupper(keyTable[(r2 + 4) % 5][c2]));
        }
        else
        {
            printf("%c%c",
                   toupper(keyTable[r1][c2]),
                   toupper(keyTable[r2][c1]));
        }
    }

    printf("\n");
}

int main()
{
    char key[100];
    char plaintext[200];
    char ciphertext[200];

    printf("Enter Key: ");
    scanf("%s", key);

    getchar();

    printf("Enter Plain Text: ");
    fgets(plaintext, sizeof(plaintext), stdin);

    plaintext[strcspn(plaintext, "\n")] = '\0';

    generateKeyTable(key);

    displayKeyTable();

    prepareText(plaintext);

    printf("\nPrepared Plain Text: %s\n", plaintext);

    // Encrypt
    encrypt(plaintext);

    // Enter ciphertext for decryption
    printf("\nEnter Cipher Text for Decryption: ");
    scanf("%s", ciphertext);

    decrypt(ciphertext);

    return 0;
}
```

**OUTPUT:**

<img width="509" height="438" alt="image" src="https://github.com/user-attachments/assets/6639cac3-7f56-4f73-9fa9-77e1ce8611ec" />

**RESULT:**

Thus, the Playfair Cipher algorithm was successfully implemented using the C language.
