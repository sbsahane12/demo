#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <time.h>

struct block {
    int blkno;
    struct block *next;
};

struct dirfile {
    char fname[20];
    int length;
    struct block *startblk;
} direntry[20];

int bv[64];
int used = 0;
int totalfile = 0;
int n;

void initBitVector() {
    srand(time(NULL));
    for (int i = 0; i < n; i++) {
        if (rand() % 2 == 0) {
            bv[i] = 0;
            used++;
        } else {
            bv[i] = 1;
        }
    }
}

void showBitVector() {
    printf("Block number\tStatus\n");
    for (int i = 0; i < n; i++) {
        printf("%d\t\t%s\n", i, bv[i] == 0 ? "Allocated" : "Free");
    }
}

int findFreeBlock() {
    for (int i = 0; i < n; ++i) {
        if (bv[i] == 1) {
            return i;
        }
    }
    return -1;
}

struct block* allocateBlocks(int length) {
    struct block* start = NULL;
    struct block* current = NULL;
    int allocatedblk = 0;
    int blocknum;
    while (allocatedblk < length) {
        blocknum = findFreeBlock();
        if (blocknum == -1) {
            printf("Error: No free space available!\n");
            return NULL;
        }
        bv[blocknum] = 0;
        struct block* newblock = (struct block*)malloc(sizeof(struct block));
        if (newblock == NULL) {
            printf("Memory allocation failed!\n");
            return NULL;
        }
        newblock->blkno = blocknum;
        newblock->next = NULL;
        if (start == NULL) {
            start = newblock;
        } else {
            current->next = newblock;
        }
        current = newblock;
        allocatedblk++;
    }
    return start;
}

void createFile() {
    char fname[10];
    int length, k;
    struct block *sblock = NULL;
    printf("\nEnter File Name: ");
    scanf("%s", fname);
    printf("Enter the length of file: ");
    scanf("%d", &length);
    sblock = allocateBlocks(length);
    if (sblock == NULL) {
        printf("File creation failed!\n");
        return;
    }
    printf("\nBlocks allocated\n");
    used += length;
    k = totalfile++;
    strcpy(direntry[k].fname, fname);
    direntry[k].startblk = sblock;
}

void displayDirectory() {
    printf("\tFilename\tStart_Block\n");
    for (int k = 0; k < totalfile; k++) {
        printf("%s\tBlocks: ", direntry[k].fname);
        struct block* current = direntry[k].startblk;
        while (current != NULL) {
            printf("%d ", current->blkno);
            current = current->next;
        }
        printf("\n\n");
    }
    printf("\nUsed blocks = %d\n", used);
    printf("Free blocks = %d\n", n - used);
}

int main() {
    int choice;
    printf("Enter the number of blocks in the disk: ");
    scanf("%d", &n);
    initBitVector();
    do {
        printf("\nMenu:\n");
        printf("1. Bit Vector\n");
        printf("2. Create new file\n");
        printf("3. Show directory\n");
        printf("4. Exit\n");
        printf("Enter your choice: ");
        scanf("%d", &choice);
        switch (choice) {
            case 1:
                showBitVector();
                break;
            case 2:
                createFile();
                break;
            case 3:
                displayDirectory();
                break;
            case 4:
                printf("Exiting...\n");
                break;
            default:
                printf("Error: Invalid choice\n");
                break;
        }
    } while (choice != 4);
    return 0;
}

#include<stdio.h>
#include<stdlib.h> // Include <stdlib.h> for abs() function

int main()
{
int queue[20], n, head, i, j, k, seek = 0, max, diff, temp, queue1[20], queue2[20];
float avg;

printf("Enter the max range of disk\n");
scanf("%d", &max);

printf("Enter the initial head position\n");
scanf("%d", &head);

printf("Enter the size of queue request\n");
scanf("%d", &n);

printf("Enter the queue of disk positions to be read\n");

int temp1 = 0, temp2 = 0; // Initialize temp1 and temp2 to zero

for (i = 1; i <= n; i++)
{
scanf("%d", &temp);
if (temp >= head)
{
queue1[temp1] = temp;
temp1++;
}
else
{
queue2[temp2] = temp;
temp2++;
}
}

for (i = 0; i < temp1 - 1; i++)
{
for (j = i + 1; j < temp1; j++)
{
if (queue1[i] > queue1[j])
{
temp = queue1[i];
queue1[i] = queue1[j];
queue1[j] = temp;
}
}
}

for (i = 0; i < temp2 - 1; i++)
{
for (j = i + 1; j < temp2; j++)
{
if (queue2[i] > queue2[j])
{
temp = queue2[i];
queue2[i] = queue2[j];
queue2[j] = temp;
}
}
}

for (i = 1, j = 0; j < temp1; i++, j++)
queue[i] = queue1[j];
queue[i] = max;
queue[i + 1] = 0;

for (i = temp1 + 3, j = 0; j < temp2; i++, j++)
queue[i] = queue2[j];

queue[0] = head;

for (j = 0; j <= n + 1; j++)
{
diff = abs(queue[j + 1] - queue[j]);
seek += diff;
printf("Disk head moves from %d to %d with %d\n", queue[j], queue[j + 1], diff);
}

printf("Total seek time is %d\n", seek);
avg = seek / (float)n;
printf("Average seek time is %f\n", avg);

return 0;
}


