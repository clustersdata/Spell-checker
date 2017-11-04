# Spell-checker

Spell checker

Description

You, as a member of a development team for a new spell checking program, are to write a module that will check the correctness of given words using a known dictionary of all correct words in all their forms.
If the word is absent in the dictionary then it can be replaced by correct words (from the dictionary) that can be obtained by one of the following operations:

deleting of one letter from the word;

replacing of one letter in the word with an arbitrary letter;

inserting of one arbitrary letter into the word.

Your task is to write the program that will find all possible replacements from the dictionary for every given word.


Input

The first part of the input file contains all words from the dictionary. Each word occupies its own line. This part is finished by the single character '#' on a separate line. All words are different. There will be at most 10000 words in the dictionary.

The next part of the file contains all words that are to be checked. Each word occupies its own line. This part is also finished by the single character '#' on a separate line. There will be at most 50 words that are to be checked.

All words in the input file (words from the dictionary and words to be checked) consist only of small alphabetic characters and each one contains 15 characters at most.


Output

Write to the output file exactly one line for every checked word in the order of their appearance in the second part of the input file. If the word is correct (i.e. it exists in the dictionary) write the message: "is correct". If the word is not correct then write this word first, then write the character ':' (colon), and after a single space write all its possible replacements, separated by spaces. The replacements should be written in the order of their appearance in the dictionary (in the first part of the input file). If there are no replacements for this word then the line feed should immediately follow the colon.

Sample Input

i

is

has

have

be

my

more

contest

me

too

if

award

#

me

aware

m

contest

hav

oo

or

i

fi

mre

#

Sample Output

me is correct

aware: award

m: i my me

contest is correct

hav: has have

oo: too

or:

i is correct

fi: i

mre: more me

注意：（1）用另一个字符数组记录字典，然后排序，用于二分查找的。

（2）先处理与被检查数组相同长度的，然后系，比距长的，最后系比距短的。

（3）数组开得足够大啦

#include<iostream>
  
#include<stdio.h>

#include<cstring>
  
using namespace std;


int cmp(const void *p, const void *q) {

    return strcmp((char *) p, (char *) q);
    
}


struct s {

    char word[20];
    
    int len;
    
} str1[10005];

char str2[10005][20], bc[20], ch[20], *p;

int bcl, n;


void input() {

    int i = 0;
    
    while (strcmp(gets(str1[++i].word), "#")) {
    
        str1[i].len = strlen(str1[i].word);
        
        strcpy(str2[i], str1[i].word);
        
    }
    
    n = i;
    
    qsort(str2 + 1, n, sizeof (str2[0]), cmp);
    
}


void solve() {

    int i, j, k;
    
    while (strcmp(gets(bc), "#")) {
    
    
        bcl = strlen(bc);
        
        p = (char *) bsearch(&bc, str2 + 1, n, sizeof (str2[0]), cmp);
        
        if (p)
        
            cout << bc << " is correct" << endl;
            
        else {
        
            cout << bc << ":";
            
            for (i = 1; i <= n; i++) {
            
                if (str1[i].len == bcl) {
                
                    int flag = 0;
                    
                    for (j = 0; j < str1[i].len; j++) {
                    
                        if (str1[i].word[j] != bc[j])
                        
                            flag++;
                            
                    }
                    
                    if (flag < 2) cout << " " << str1[i].word;
                    
                } else if (bcl == str1[i].len + 1) {
                

                    for (j = 0; j < bcl; j++) {
                    
                        strcpy(ch, bc);
                        
                        for (k = j; k < bcl; k++)
                        
                            ch[k] = ch[k + 1];
                            
                        //cout<<bc<<" "<<str1[i].word<<" "<<ch<<endl;
                        
                        if (strcmp(ch, str1[i].word) == 0) {
                        
                            printf(" %s", str1[i].word);
                            
                            break;
                            
                        }
                        
                    }
                    
                } else if (bcl == str1[i].len - 1) {
                

                    for (j = 0; j < str1[i].len; j++) {
                    
                        strcpy(ch, str1[i].word);
                        
                        for (k = j; k < str1[i].len; k++)
                        
                            ch[k] = ch[k + 1];
                            
                        //cout<<bc<<" "<<str1[i].word<<" "<<ch<<endl;
                        
                        if (strcmp(ch, bc) == 0) {
                        
                            printf(" %s", str1[i].word);
                            
                            break;
                            
                        }
                        
                    }
                    
                }
                
            }
            
            cout << endl;
            
        }
        
    }
    
}


int main() {

    input();
    
    solve();
    
    return 0;
    
}
