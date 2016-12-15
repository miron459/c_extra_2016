# c_extra_2016#include <string.h>
#include <stdio.h>
#include <stdlib.h>
typedef struct Word Word;
struct Word
{
    int cnt;
    char* item;
    Word* next;
};
int Word_get(Word* Word, char* index);
int Word_get_index(Word* Word, char* index);
void Word_set(Word* Word, unsigned index, int item);
Word* Word_add(Word* head, char* item)
{
    if (Word_get(head, item) == -1)
    {
        Word* new_item = (Word*)malloc(sizeof(Word));
        new_item->item = item;
        new_item->next = NULL;
        new_item->cnt = 1;
        if (head)
        {
            Word* Word;
            for (Word = head; Word->next; Word = Word->next)
                ;
            Word->next = new_item;
        }
        else
        {
            head = new_item;
        }
    }
    else
    {
        Word_set(head, Word_get(head, item), Word_get_index(head, item) + 1);
    }
    return head;
}

int Word_get_index(Word* Word, char* index)
{
    unsigned i;
    for (i = 0; Word; Word = Word->next, i++)
        if (strcmp(Word->item, index) == 0)
            return Word->cnt;
    return -1;
}
int Word_get(Word* Word, char* index)
{
    unsigned i;
    for (i = 0; Word; Word = Word->next, i++)
    {
        if (strcmp(Word->item, index) == 0)
        {
            return i;
        }
    }
    return -1;
}
void Word_set(Word* Word, unsigned index, int item)
{
    unsigned i;
    for (i = 0; Word; Word = Word->next, i++)
        if (i == index)
        {
            Word->cnt = item;
            return;
        }
}
void Word_print(Word* Word)
{
    for (; Word; Word = Word->next)
        printf("%s %d->\n", Word->item, Word->cnt);
    printf("NULL\n");
}
void Word_destroy(Word* Word)
{
    if (Word)
    {
        Word_destroy(Word->next);
        free(Word);
    }
}
int main()
{
    Word* Word = NULL;
    int n, k = 0, i = 0;
    char* t;
    char tmp[1025];
    FILE* p = fopen("input.txt", "r");
    char** mas = (char**)malloc(n * sizeof(char*));
    char** mas_copy = (char**)malloc(n * sizeof(char*));
    for (n = 0; fgets(tmp, sizeof(tmp), p); n++)
    {
        int len = strlen(tmp);
        mas[n] = strdup(tmp);
        mas_copy[n] = strdup(tmp);
    }
    fclose(p);
    for (i = 0; i < n; i++)
    {
        t = strtok(mas_copy[i], " ,.-");
        while (t != NULL)
        {
            if ((t[0] >= 'A') && (t[0] <= 'Z'))
            {
                Word = Word_add(Word, t);
                k++;
            }
            t = strtok(NULL, " ,.-");
        }
    }
    Word_print(Word);
    printf("k = %d\n", k);
    p = fopen("output.txt", "w");
    if (p != NULL)
    {
        for (i = 0; i < n; i++)
        {
            fprintf(p, "%s", mas[i]);
            free(mas[i]);
        }
        fclose(p);
    }
    free(mas);
    free(mas_copy);
    return 0;
}
