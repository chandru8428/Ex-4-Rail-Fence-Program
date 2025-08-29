# Ex-4 Rail-Fence-Program

# IMPLEMENTATION OF RAIL FENCE – ROW & COLUMN TRANSFORMATION TECHNIQUE

# AIM:

# To write a C program to implement the rail fence transposition technique.

# DESCRIPTION:

In the rail fence cipher, the plain text is written downwards and diagonally on successive "rails" of an imaginary fence, then moving up when we reach the bottom rail. When we reach the top rail, the message is written downwards again until the whole plaintext is written out. The message is then read off in rows.

# ALGORITHM:

STEP-1: Read the Plain text.
STEP-2: Arrange the plain text in row columnar matrix format.
STEP-3: Now read the keyword depending on the number of columns of the plain text.
STEP-4: Arrange the characters of the keyword in sorted order and the corresponding columns of the plain text.
STEP-5: Read the characters row wise or column wise in the former order to get the cipher text.

# PROGRAM
```
#include <stdio.h>
#include <string.h>
#include <ctype.h>

#define MAX 100

// Function to remove spaces and convert to uppercase
void formatText(char text[]) {
    int i, j = 0;
    char temp[MAX];
    for (i = 0; text[i]; i++) {
        if (isalpha(text[i])) {
            temp[j++] = toupper(text[i]);
        }
    }
    temp[j] = '\0';
    strcpy(text, temp);
}

// Function to implement Rail Fence (Row & Column Transformation)
void railFence(char text[], char key[]) {
    int len = strlen(text);
    int cols = strlen(key);
    int rows = (len + cols - 1) / cols;
    char matrix[rows][cols];
    int k = 0;

    // Fill matrix row by row
    for (int i = 0; i < rows; i++) {
        for (int j = 0; j < cols; j++) {
            if (k < len)
                matrix[i][j] = text[k++];
            else
                matrix[i][j] = 'X';  // padding
        }
    }

    // Display the matrix
    printf("\nRow-Column Matrix:\n");
    for (int i = 0; i < rows; i++) {
        for (int j = 0; j < cols; j++)
            printf("%c ", matrix[i][j]);
        printf("\n");
    }

    // Sort keyword characters with index
    int index[cols];
    char keyCopy[MAX];
    strcpy(keyCopy, key);

    for (int i = 0; i < cols; i++) index[i] = i;
    
    // Bubble sort key with index tracking
    for (int i = 0; i < cols - 1; i++) {
        for (int j = 0; j < cols - i - 1; j++) {
            if (keyCopy[j] > keyCopy[j + 1]) {
                char tmp = keyCopy[j];
                keyCopy[j] = keyCopy[j + 1];
                keyCopy[j + 1] = tmp;

                int t = index[j];
                index[j] = index[j + 1];
                index[j + 1] = t;
            }
        }
    }

    // Generate Cipher Text column-wise based on sorted keyword
    char cipher[MAX] = "";
    int pos = 0;
    for (int k = 0; k < cols; k++) {
        for (int i = 0; i < rows; i++) {
            cipher[pos++] = matrix[i][index[k]];
        }
    }
    cipher[pos] = '\0';

    printf("\nCipher Text: %s\n", cipher);
}

int main() {
    char text[MAX], key[MAX];

    printf("Enter Plain Text: ");
    fgets(text, sizeof(text), stdin);
    text[strcspn(text, "\n")] = 0;  // remove newline
    formatText(text);

    printf("Enter Keyword: ");
    fgets(key, sizeof(key), stdin);
    key[strcspn(key, "\n")] = 0;  // remove newline
    formatText(key);

    railFence(text, key);

    return 0;
}
```
# OUTPUT
<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/0e50e433-cc3e-478a-8023-30d90bf01d8e" />

# RESULT
