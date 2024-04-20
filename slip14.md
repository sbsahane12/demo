#include<stdio.h>
#include<stdlib.h>
#include<string.h>
#include<time.h>

struct dirfile {
    char fname[20];
    int startblk, length;
} direntry[20];

int bv[64];
int used = 0;
int totalfile = 0;
int n;

void initialize() {
    int i;
    srand(time(NULL));
    for (i = 0; i < n; i++) {
        if (rand() % 2 == 0) {
            bv[i] = 0;
            used++;
        } else {
            bv[i] = 1;
        }
    }
}

void showbv() {
    int i;
    printf("block number \t status\n");
    for (i = 0; i < n; i++) {
        printf("%d\t\t", i);
        if (bv[i] == 0) {
            printf("allocated\n");
        } else {
            printf("free\n");
        }
    }
}

int search(int length) {
    int i, j, flag = 1, blknum;
    for (i = 0; i < n; i++) {
        if (bv[i] == 1) {
            flag = 1;
            for (blknum = i, j = 0; j < length; j++) {
                if (bv[blknum] == 1) {
                    blknum++;
                    continue;
                } else {
                    flag = 0;
                    break;
                }
            }
            if (flag == 1)
                return i;
        }
    }
    return -1;
}

void createfile() {
    char fname[20];
    int length, blknum, k;
    printf("\n enter file name:");
    scanf("%s", fname);
    printf("\n enter the length of file:");
    scanf("%d", &length);
    if (length <= n - used) {
        blknum = search(length);
    } else {
        blknum = -1;
    }
    if (blknum == -1) {
        printf("error: no disk space available\n");
    } else {
        printf("\nblock allocated\n");
        used = used + length;
        for (k = blknum; k < (blknum + length); k++) {
            bv[k] = 0;
        }
        strcpy(direntry[totalfile].fname, fname);
        direntry[totalfile].startblk = blknum;
        direntry[totalfile].length = length;
        totalfile++; // Increment here to avoid off-by-one error
    }
}

void displaydir() {
    int k;
    printf("\tfilename\tstart\tsize\n");
    for (k = 0; k < totalfile; k++) {
        printf("%s\t%d\t%d\n", direntry[k].fname, direntry[k].startblk, direntry[k].length);
    }
    printf("\nused block=%d", used);
    printf("\nfree block=%d", n - used);
}

int main() {
    int choice;
    printf("enter the number of blocks in the disk:");
    scanf("%d", &n);
    initialize();
    do {
        printf("\nmenu\n");
        printf("1.bit vector\n");
        printf("2.create new file\n");
        printf("3.show directory\n");
        printf("4.exit\n");
        printf("enter your choice: ");
        scanf("%d", &choice);
        switch (choice) {
            case 1:
                showbv();
                break;
            case 2:
                createfile();
                break;
            case 3:
                displaydir();
                break;
            case 4:
                printf("exiting...\n");
                break;
            default:
                printf("error: invalid choice\n");
                break;
        }
    } while (choice != 4);
    return 0;
}

#include <stdio.h>
#include <stdlib.h>
#include <math.h>

int main() {
    int i, n, k, mov = 0, cp, mini, cp1;
    int req[50], a[50];

printf("enter the current position: ");
scanf("%d", &cp);

printf("enter the number of requests: ");
scanf("%d", &n);

cp1 = cp;

printf("enter the request order: ");
for (i = 0; i < n; i++) {
    scanf("%d", &req[i]);
}

for (k = 0; k < n; k++) {
    int index[50];
    for (i = 0; i < n; i++) {
        index[i] = abs(cp - req[i]);
    }
    int min = index[0];
    mini = 0;
    for (i = 1; i < n; i++) {
        if (min > index[i]) {
            min = index[i];
            mini = i;
        }
    }
    a[k] = req[mini];
    mov += abs(cp - a[k]);
    cp = a[k];
    req[mini] = 999;
}

printf("Sequence is: %d", cp1);
mov += abs(cp1 - a[0]);
printf(" -> %d", a[0]);
for (i = 1; i < n; i++) {
    mov += abs(a[i] - a[i - 1]);
    printf(" -> %d", a[i]);
}
printf("\n");
printf("total head movement = %d\n", mov);

return 0;
}
