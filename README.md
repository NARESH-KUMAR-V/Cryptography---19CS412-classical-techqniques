# Cryptography---19CS412-classical-techqniques
# Caeser Cipher
Caeser Cipher using with different key values

# AIM:

To encrypt and decrypt the given message by using Ceaser Cipher encryption algorithm.


## DESIGN STEPS:

### Step 1:

Design of Caeser Cipher algorithnm 

### Step 2:

Implementation using C or pyhton code

### Step 3:

1.	In Ceaser Cipher each letter in the plaintext is replaced by a letter some fixed number of positions down the alphabet.
2.	For example, with a left shift of 3, D would be replaced by A, E would become B, and so on.
3.	The encryption can also be represented using modular arithmetic by first transforming the letters into numbers, according to the   
    scheme, A = 0, B = 1, Z = 25.
4.	Encryption of a letter x by a shift n can be described mathematically as,
                       En(x) = (x + n) mod26
5.	Decryption is performed similarly,
                       Dn (x)=(x - n) mod26


## PROGRAM:
``` C
#include<stdio.h>
#include <string.h>
#include<conio.h>
#include <ctype.h>
int main()
{
char plain[10], cipher[10];
int key,i,length;
int result;
printf("\n Enter the plain text:");
scanf("%s", plain);
printf("\n Enter the key value:");
scanf("%d", &key);
printf("\n \n \t PLAIN TEXt: %s",plain);
printf("\n \n \t ENCRYPTED TEXT: ");
for(i = 0, length = strlen(plain); i < length; i++)
{
cipher[i]=plain[i] + key;
if (isupper(plain[i]) && (cipher[i] > 'Z'))
cipher[i] = cipher[i] - 26;
if (islower(plain[i]) && (cipher[i] > 'z'))
cipher[i] = cipher[i] - 26;
printf("%c", cipher[i]);
}
printf("\n \n \t AFTER DECRYPTION : ");
for(i=0;i<length;i++)
{
plain[i]=cipher[i]-key;
if(isupper(cipher[i])&&(plain[i]<'A'))
plain[i]=plain[i]+26;
if(islower(cipher[i])&&(plain[i]<'a'))
plain[i]=plain[i]+26;
printf("%c",plain[i]);
}
return 0;
}
```



## OUTPUT:

![Screenshot 2024-09-09 101941](https://github.com/user-attachments/assets/c916c6ab-29dc-4634-b165-31ce6792a358)


## RESULT:
The program is executed successfully

---------------------------------

# PlayFair Cipher
Playfair Cipher using with different key values

# AIM:

To implement a program to encrypt a plain text and decrypt a cipher text using play fair Cipher substitution technique.

 
## DESIGN STEPS:

### Step 1:

Design of PlayFair Cipher algorithnm 

### Step 2:

Implementation using C or pyhton code

### Step 3:

Testing algorithm with different key values. 

ALGORITHM DESCRIPTION:
The Playfair cipher uses a 5 by 5 table containing a key word or phrase. To generate the key table, first fill the spaces in the table with the letters of the keyword, then fill the remaining spaces with the rest of the letters of the alphabet in order (usually omitting "Q" to reduce the alphabet to fit; other versions put both "I" and "J" in the same space). The key can be written in the top rows of the table, from left to right, or in some other pattern, such as a spiral beginning in the upper-left-hand corner and ending in the centre.
The keyword together with the conventions for filling in the 5 by 5 table constitutes the cipher key. To encrypt a message, one would break the message into digrams (groups of 2 letters) such that, for example, "HelloWorld" becomes "HE LL OW OR LD", and map them out on the key table. Then apply the following 4 rules, to each pair of letters in the plaintext:
1.	If both letters are the same (or only one letter is left), add an "X" after the first letter. Encrypt the new pair and continue. Some   
   variants of Playfair use "Q" instead of "X", but any letter, itself uncommon as a repeated pair, will do.
2.	If the letters appear on the same row of your table, replace them with the letters to their immediate right respectively (wrapping 
   around to the left side of the row if a letter in the original pair was on the right side of the row).
3.	If the letters appear on the same column of your table, replace them with the letters immediately below respectively (wrapping around 
   to the top side of the column if a letter in the original pair was on the bottom side of the column).
4.	If the letters are not on the same row or column, replace them with the letters on the same row respectively but at the other pair of 
   corners of the rectangle defined by the original pair. The order is important – the first letter of the encrypted pair is the one that 
    lies on the same row as the first letter of the plaintext pair.
To decrypt, use the INVERSE (opposite) of the last 3 rules, and the 1st as-is (dropping any extra "X"s, or "Q"s that do not make sense in the final message when finished).


## PROGRAM:
``` C
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <ctype.h>
#define SIZE 5
void generateKeyTable(char key[], char keyTable[SIZE][SIZE]);
void searchInKeyTable(char keyTable[SIZE][SIZE], char a, char b, int pos[]);
void encrypt(char str[], char keyTable[SIZE][SIZE]);
void decrypt(char str[], char keyTable[SIZE][SIZE]);
void prepareText(char str[], char preparedStr[]);
void removeDuplicates(char str[]);

int main() {
    char key[SIZE*SIZE], keyTable[SIZE][SIZE];
    char message[100], preparedMessage[100];
    printf("Enter the key: ");
    scanf("%s", key);
    removeDuplicates(key);
    generateKeyTable(key, keyTable);
    printf("Enter PLAIN TEXT: ");
    scanf("%s", message);
    prepareText(message, preparedMessage);
    printf("Prepared text: %s\n", preparedMessage);
    encrypt(preparedMessage, keyTable);
    printf("Encrypted text: %s\n", preparedMessage);
    decrypt(preparedMessage, keyTable);
    printf("Decrypted text: %s\n", preparedMessage);

    return 0;
}
void removeDuplicates(char str[]) {
    int hash[26] = {0}; 
    int i, k = 0;
    for (i = 0; str[i]; i++) {
        str[i] = toupper(str[i]);
        if (str[i] == 'J') str[i] = 'I'; 
        if (!hash[str[i] - 'A']) {
            hash[str[i] - 'A'] = 1;
            str[k++] = str[i];
        }
    }
    str[k] = '\0';
}
void generateKeyTable(char key[], char keyTable[SIZE][SIZE]) {
    int i, j, k = 0, hash[26] = {0};
    char currentLetter = 'A';
    for (i = 0; i < SIZE; i++) {
        for (j = 0; j < SIZE; j++) {
            while (k < strlen(key) && hash[key[k] - 'A']) k++;
            if (k < strlen(key)) {
                keyTable[i][j] = key[k];
                hash[key[k] - 'A'] = 1;
                k++;
            } else {
                while (hash[currentLetter - 'A'] || currentLetter == 'J') currentLetter++;
                keyTable[i][j] = currentLetter;
                hash[currentLetter - 'A'] = 1;
            }
        }
    }
}
void searchInKeyTable(char keyTable[SIZE][SIZE], char a, char b, int pos[]) {
    int i, j;
    for (i = 0; i < SIZE; i++) {
        for (j = 0; j < SIZE; j++) {
            if (keyTable[i][j] == a) {
                pos[0] = i;
                pos[1] = j;
            }
            if (keyTable[i][j] == b) {
                pos[2] = i;
                pos[3] = j;
            }
        }
    }
}
void prepareText(char str[], char preparedStr[]) {
    int i, k = 0;
    char a, b;
    for (i = 0; str[i]; i++) {
        if (str[i] == ' ') continue;
        preparedStr[k++] = toupper(str[i] == 'J' ? 'I' : str[i]);
    }
    preparedStr[k] = '\0';
    for (i = 0; i < strlen(preparedStr); i += 2) {
        a = preparedStr[i];
        b = preparedStr[i + 1];
        if (b == '\0' || a == b) {
            memmove(preparedStr + i + 1, preparedStr + i, strlen(preparedStr) - i + 1);
            preparedStr[i + 1] = 'X';
        }
    }
}
void encrypt(char str[], char keyTable[SIZE][SIZE]) {
    int i, pos[4];
    char a, b;
    for (i = 0; i < strlen(str); i += 2) {
        a = str[i];
        b = str[i + 1];
        searchInKeyTable(keyTable, a, b, pos);
        if (pos[0] == pos[2]) { // Same row
            str[i] = keyTable[pos[0]][(pos[1] + 1) % SIZE];
            str[i + 1] = keyTable[pos[2]][(pos[3] + 1) % SIZE];
        } else if (pos[1] == pos[3]) { // Same column
            str[i] = keyTable[(pos[0] + 1) % SIZE][pos[1]];
            str[i + 1] = keyTable[(pos[2] + 1) % SIZE][pos[3]];
        } else { // Rectangle swap
            str[i] = keyTable[pos[0]][pos[3]];
            str[i + 1] = keyTable[pos[2]][pos[1]];
        }
    }
}
void decrypt(char str[], char keyTable[SIZE][SIZE]) {
    int i, pos[4];
    char a, b;
    for (i = 0; i < strlen(str); i += 2) {
        a = str[i];
        b = str[i + 1];
        searchInKeyTable(keyTable, a, b, pos);
        if (pos[0] == pos[2]) { // Same row
            str[i] = keyTable[pos[0]][(pos[1] - 1 + SIZE) % SIZE];
            str[i + 1] = keyTable[pos[2]][(pos[3] - 1 + SIZE) % SIZE];
        } else if (pos[1] == pos[3]) { // Same column
            str[i] = keyTable[(pos[0] - 1 + SIZE) % SIZE][pos[1]];
            str[i + 1] = keyTable[(pos[2] - 1 + SIZE) % SIZE][pos[3]];
        } else { // Rectangle swap
            str[i] = keyTable[pos[0]][pos[3]];
            str[i + 1] = keyTable[pos[2]][pos[1]];
        }
    }
}

```

## OUTPUT:

![Screenshot 2024-10-07 103956](https://github.com/user-attachments/assets/3903c35f-d1c2-4715-bf86-017186ec5c4e)


## RESULT:
The program is executed successfully


---------------------------

# Hill Cipher
Hill Cipher using with different key values

# AIM:

To develop a simple C program to implement Hill Cipher.

## DESIGN STEPS:

### Step 1:

Design of Hill Cipher algorithnm 

### Step 2:

Implementation using C or pyhton code

### Step 3:

Testing algorithm with different key values. 
ALGORITHM DESCRIPTION:
The Hill cipher is a substitution cipher invented by Lester S. Hill in 1929. Each letter is represented by a number modulo 26. To encrypt a message, each block of n letters is multiplied by an invertible n × n matrix, again modulus 26.
To decrypt the message, each block is multiplied by the inverse of the matrix used for encryption. The matrix used for encryption is the cipher key, and it should be chosen randomly from the set of invertible n × n matrices (modulo 26).
The cipher can, be adapted to an alphabet with any number of letters. All arithmetic just needs to be done modulo the number of letters instead of modulo 26.


## PROGRAM:
```
#include <stdio.h>
#include <string.h>

#define MATRIX_SIZE 3

// Function to print matrix
void printMatrix(unsigned int matrix[MATRIX_SIZE][MATRIX_SIZE]) {
    for (int i = 0; i < MATRIX_SIZE; i++) {
        for (int j = 0; j < MATRIX_SIZE; j++) {
            printf("%d ", matrix[i][j]);
        }
        printf("\n");
    }
}

// Function to multiply matrix and vector
void multiplyMatrixVector(unsigned int matrix[MATRIX_SIZE][MATRIX_SIZE], unsigned int vector[MATRIX_SIZE], unsigned int result[MATRIX_SIZE]) {
    for (int i = 0; i < MATRIX_SIZE; i++) {
        unsigned int sum = 0;
        for (int j = 0; j < MATRIX_SIZE; j++) {
            sum += matrix[i][j] * vector[j];
        }
        result[i] = sum % 26;
    }
}

int main() {
    unsigned int a[MATRIX_SIZE][MATRIX_SIZE] = {{6, 24, 1}, {13, 16, 10}, {20, 17, 15}};
    unsigned int b[MATRIX_SIZE][MATRIX_SIZE] = {{8, 5, 10}, {21, 8, 21}, {21, 12, 8}};
    
    unsigned int c[MATRIX_SIZE], d[MATRIX_SIZE];
    char msg[20];
    
    printf("Enter plain text (up to 3 characters): ");
    scanf("%s", msg);
    
    // Ensure message length is 3 or less
    int msgLen = strlen(msg);
    if (msgLen > MATRIX_SIZE) {
        printf("Message too long. Limiting to %d characters.\n", MATRIX_SIZE);
        msgLen = MATRIX_SIZE;
    }
    
    // Convert message to numeric values
    for (int i = 0; i < msgLen; i++) {
        c[i] = msg[i] - 65;
        printf("%d ", c[i]);
    }
    printf("\n");
    
    // Encrypt
    multiplyMatrixVector(a, c, d);
    
    printf("Encrypted Cipher Text: ");
    for (int i = 0; i < MATRIX_SIZE; i++) {
        printf("%c ", d[i] + 65);
    }
    printf("\n");
    
    // Decrypt
    multiplyMatrixVector(b, d, c);
    
    printf("Decrypted Cipher Text: ");
    for (int i = 0; i < MATRIX_SIZE; i++) {
        printf("%c ", c[i] + 65);
    }
    printf("\n");
    
    return 0;
}
```
## OUTPUT:
![Screenshot 2024-10-07 105652](https://github.com/user-attachments/assets/b9f94594-df13-4d02-97ce-5ee9cf783bbd)


## RESULT:
The program is executed successfully

-------------------------------------------------

# Vigenere Cipher
Vigenere Cipher using with different key values

# AIM:

To develop a simple C program to implement Vigenere Cipher.

## DESIGN STEPS:

### Step 1:

Design of Vigenere Cipher algorithnm 

### Step 2:

Implementation using C or pyhton code

### Step 3:

Testing algorithm with different key values. 
ALGORITHM DESCRIPTION:
The Vigenere cipher is a method of encrypting alphabetic text by using a series of different Caesar ciphers based on the letters of a keyword. It is a simple form of polyalphabetic substitution.To encrypt, a table of alphabets can be used, termed a Vigenere square, or Vigenere table. It consists of the alphabet written out 26 times in different rows, each alphabet shifted cyclically to the left compared to the previous alphabet, corresponding to the 26 possible Caesar ciphers. At different points in the encryption process, the cipher uses a different alphabet from one of the rows used. The alphabet at each point depends on a repeating keyword.



## PROGRAM:
PROGRAM:
#include<stdio.h> #include<string.h>
//FunctiontoperformVigenereencryption voidvigenereEncrypt(char*text,constchar*key){ inttextLen= strlen(text);
intkeyLen=strlen(key); for(inti =0;i< textLen;i++){ charc =text[i]; if(c>='A'&&c<='Z'){
//Encryptuppercaseletters
text[i]=((c-'A'+key[i%keyLen]-'A')%26)+'A';
}else if(c>='a'&&c<='z'){
//Encryptlowercaseletters
text[i]=((c-'a'+key[i%keyLen]-'A')%26)+'a';
}
}
}
//FunctiontoperformVigeneredecryption voidvigenereDecrypt(char*text,constchar*key){ inttextLen= strlen(text);
intkeyLen=strlen(key);

for(inti =0;i< textLen;i++){ charc =text[i]; if(c>='A'&&c<='Z'){
//Decryptuppercaseletters
 
text[i]=((c-'A'-(key[i% keyLen]-'A') +26) %26)+ 'A';
}else if(c>='a'&&c<='z'){
//Decryptlowercaseletters
text[i]=((c-'a'-(key[i% keyLen]-'A') +26) %26)+ 'a';
}
}
}
intmain(){
constchar *key="KEY";//Replacewithyourdesired key
char message[]= "Thisisasecretmessage.";//Replace withyourmessage
//Encrypt themessage vigenereEncrypt(message,key); printf("EncryptedMessage:%s\n",message);
//Decrypt themessage backtotheoriginal vigenereDecrypt(message,key); printf("DecryptedMessage:%s\n",message); Return 0;

## OUTPUT:
OUTPUT :

Simulating Vigenere Cipher


Input Message : SecurityLaboratory
Encrypted Message : NMIYEMKCNIQVVROWXC Decrypted Message : SECURITYLABORATORY
## RESULT:
The program is executed successfully

-----------------------------------------------------------------------

# Rail Fence Cipher
Rail Fence Cipher using with different key values

# AIM:

To develop a simple C program to implement Rail Fence Cipher.

## DESIGN STEPS:

### Step 1:

Design of Rail Fence Cipher algorithnm 

### Step 2:

Implementation using C or pyhton code

### Step 3:

Testing algorithm with different key values. 
ALGORITHM DESCRIPTION:
In the rail fence cipher, the plaintext is written downwards and diagonally on successive "rails" of an imaginary fence, then moving up when we reach the bottom rail. When we reach the top rail, the message is written downwards again until the whole plaintext is written out. The message is then read off in rows.

## PROGRAM:

PROGRAM:
#include<stdio.h> #include<string.h> #include<stdlib.h> main()
{
int i,j,len,rails,count,code[100][1000]; char str[1000];
printf("Enter a Secret Message\n"); gets(str);
len=strlen(str);
printf("Enter number of rails\n"); scanf("%d",&rails); for(i=0;i<rails;i++)
{
for(j=0;j<len;j++)
{
code[i][j]=0;
}
}
count=0; j=0;
while(j<len)
{
if(count%2==0)
{
for(i=0;i<rails;i++)
{
//strcpy(code[i][j],str[j]);
code[i][j]=(int)str[j]; j++;
}

}
else
{
 
for(i=rails-2;i>0;i--)
{
code[i][j]=(int)str[j]; j++;
}
}

count++;
}

for(i=0;i<rails;i++)
{
for(j=0;j<len;j++)
{
if(code[i][j]!=0) printf("%c",code[i][j]);
}
}
printf("\n");
}
## OUTPUT:
OUTPUT:
Enter a Secret Message wearediscovered
Enter number of rails 2
waeicvrderdsoee
## RESULT:
The program is executed successfully
