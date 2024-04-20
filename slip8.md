#include<stdio.h>
#include<stdlib.h>
#include<string.h>
#include<time.h>

struct dirfile
{
    char fname[20];
    int startblk, length;
} direntry[20];

int bv[64];
int used = 0;
int totalfile = 0;
int n;

void initialize()
{
    int i;
    srand(time(NULL));
    for(i = 0; i < n; i++)
    {
        if(rand() % 2 == 0)
        {
            bv[i] = 0;
            used++;
        }
        else
        {
            bv[i] = 1;
        }
    }
}

void showbv()
{
    int i;
    printf("block number \t status\n");
    for(i = 0; i < n; i++)
    {
        printf("%d\t\t", i);
        if(bv[i] == 0)
        {
            printf("allocated\n");
        }
        else
        {
            printf("free\n");
        }
    }
}

int search(int length)
{
    int i, j, flag = 1, blknum;
    for(i = 0; i < n; i++)
    {
        if(bv[i] == 1)
        {
            flag = 1;
            for(blknum = i, j = 0; j < length; j++)
            {
                if(bv[blknum] == 1)
                {
                    blknum++;
                    continue;
                }
                else
                {
                    flag = 0;
                    break;
                }
            }
            if(flag == 1)
                return i;
        }
    }
    return -1;
}

void createfile()
{
    char fname[10];
    int length, blknum, k;
    printf("\nEnter file name: ");
    scanf("%s", &fname);
    printf("\nEnter the length of file: ");
    scanf("%d", &length);
    if(length <= n - used)
    {
        blknum = search(length);
    }
    else
    {
        blknum = -1;
    }
    if(blknum == -1)
    {
        printf("Error: No disk space available\n");
    }
    else
    {
        printf("\nBlock allocated\n");
        used = used + length;
        for(k = blknum; k < (blknum + length); k++)
        {
            bv[k] = 0;
        }
        k = totalfile++;
        strcpy(direntry[k].fname, fname);
        direntry[k].startblk = blknum;
        direntry[k].length = length;
    }
}

void displaydir()
{
    int k;
    printf("\nFilename\tStart\tSize\n");
    for(k = 0; k < totalfile; k++)
    {
        printf("%s\t\t%d\t%d\n", direntry[k].fname, direntry[k].startblk, direntry[k].length);
    }
    printf("\nUsed blocks = %d", used);
    printf("\nFree blocks = %d\n", n - used);
}

int main()
{
    int choice;
    printf("Enter the number of blocks in the disk: ");
    scanf("%d", &n);
    initialize();
    do
    {
        printf("\nMenu\n");
        printf("1. Show bit vector\n");
        printf("2. Create new file\n");
        printf("3. Show directory\n");
        printf("4. Exit\n");
        printf("Enter your choice: ");
        scanf("%d", &choice);
        switch(choice)
        {
            case 1:
                showbv(n);
                break;
            case 2:
                createfile(n); 
                break;
            case 3:
                displaydir(); 
                break;
            case 4:
                printf("Exiting...\n"); 
                break;
            default:
                printf("Error: Invalid choice\n");
                break;
        }
    } while(choice != 4);
    return 0;
}

#include<math.h>
#include<stdio.h>
#include<stdlib.h>

int main()
{
    int i, n, k, req[50], mov = 0, cp, index[50], min, a[50], j = 0, mini, cp1;
    printf("Enter the current position: ");
    scanf("%d", &cp);
    printf("Enter the number of requests: ");
    scanf("%d", &n);
    cp1 = cp;
    printf("Enter the request order: ");
    for(i = 0; i < n; i++)
    {
        scanf("%d", &req[i]);
    }
    for(k = 0; k < n; k++)
    {
        for(i = 0; i < n; i++)
        {
            index[i] = abs(cp - req[i]);
        }
        min = index[0];
        mini = 0;
        for(i = 1; i < n; i++)
        {
            if(min > index[i])
            {
                min = index[i];
                mini = i;
            }
        }
        a[j] = req[mini];
        j++;
        cp = req[mini];
        req[mini] = 999;
    }
    printf("Sequence is: ");
    printf("%d", cp1);
    mov = mov + abs(cp1 - a[0]);
    printf(" -> %d", a[0]);
    for(i = 1; i < n; i++)
    {
        mov = mov + abs(a[i] - a[i - 1]);
        printf(" -> %d", a[i]);
    }
    printf("\n");
    printf("Total head movement = %d\n", mov);
    return 0;
}
