#include<stdio.h>
#include<stdlib.h>
#include<string.h>
#include<time.h>

struct dirfile {
    char fname[20];
    int length;
    int indexblock;
    int a[10];
} direntry[20];

int bv[64];
int used=0;
int totalfile=0;
int n;

void initialize() {
    int i;
    srand(time(NULL));
    for(i=0;i<n;i++) {
        if(rand()%2==0) {
            bv[i]=0;
            used++;
        } else {
            bv[i]=1;
        }
    }
}

void showbv() {
    int i;
    printf("block number\t status\n");
    for(i=0;i<n;i++) {
        printf("%d\t\t",i);
        if(bv[i]==0) {
            printf("allocated\n");
        } else {
            printf("Free\n");
        }
    }
}

int findFreeBlock() {
    for (int i = 0; i < n; ++i) {
        if (bv[i] == 1) {
            return i;
        }
    }
    return -1; // No free block found
}

void allocateBlocks(int length) {
    int allocatedblk=0;
    int blocknum;
    direntry[totalfile].indexblock=0;
    while (allocatedblk < length) {
        blocknum = findFreeBlock();
        if (blocknum == -1) {
            printf("Error: No free space available!\n");
            return ;
        }
        // Allocate block
        bv[blocknum] = 0;
        if (direntry[totalfile].indexblock == 0) {
            direntry[totalfile].indexblock = blocknum;
        } else {
            direntry[totalfile].a[allocatedblk]=blocknum;
            allocatedblk++;
        }
    }
}

void createfile() {
    char fname[10];
    int length;
    printf("\nEnter File Name : ");
    scanf("%s", fname);
    printf("Enter the length of file:");
    scanf("%d", &length);
    allocateBlocks(length);
    printf("\n block allocated\n");
    used=used+length;
    int k = totalfile++;
    strcpy(direntry[k].fname, fname);
    direntry[k].length = length;
}

void displaydir() {
    int k, i;
    printf("\t filename\t start_block\n");
    for(k=0; k<totalfile; k++) {
        printf("%s", direntry[k].fname);
        printf("Index Block = %d", direntry[k].indexblock);
        printf("\tLength = %d", direntry[k].length);
        printf("\tBlocks: ");
        i=0;
        while (i < direntry[k].length) {
            printf("\t%d ", direntry[k].a[i] );
            i++;
        }
        printf("\n\n");
    }
    printf("\n used block=%d", used);
    printf("\n free block =%d\n", n-used);
}

int main() {
    int choice;
    printf("enter the number of blocks in the disk:");
    scanf("%d",&n);
    initialize();
    do {
        printf("\n menu:\n");
        printf("1.bit vector \n");
        printf("2.create new file\n");
        printf("3.show directory\n");
        printf("4.exit\n");
        printf("Enter your choice:");
        scanf("%d",&choice);
        switch(choice) {
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
                printf("Exiting.....");
                break;
            default:
                printf("Error: invalid choice\n");
                break;
        }
    } while(choice!=4);
    return 0;
}

#include<stdio.h>

void main() {
    int queue[20], n, head, i, j, k, seek = 0, max, diff, temp, queue1[20], queue2[20], temp1 = 0, temp2 = 0;
    printf("Enter the max range of disk: ");
    scanf("%d", &max);
    printf("Enter the initial head position: ");
    scanf("%d", &head);
    printf("Enter the number of queue elements: ");
    scanf("%d", &n);
    printf("Enter the queue elements: ");
    for(i=1; i<=n; i++) {
        scanf("%d", &temp);
        // Process the queue elements into two separate queues
        if(temp >= head) {
            queue1[temp1] = temp;
            temp1++;
        } else {
            queue2[temp2] = temp;
            temp2++;
        }
    }
    // Sort queue1 - increasing order
    for(i=0; i<temp1-1; i++) {
        for(j=i+1; j<temp1; j++) {
            if(queue1[i] > queue1[j]) {
                temp = queue1[i];
                queue1[i] = queue1[j];
                queue1[j] = temp;
            }
        }
    }
    // Sort queue2 - decreasing order
    for(i=0; i<temp2-1; i++) {
        for(j=i+1; j<temp2; j++) {
            if(queue2[i] < queue2[j]) {
                temp = queue2[i];
                queue2[i] = queue2[j];
                queue2[j] = temp;
            }
        }
    }
    // Join the two queues
    for(i=1, j=0; j<temp1; i++, j++) {
        queue[i] = queue1[j];
    }
    queue[i] = max;
    for(i=temp1+2, j=0; j<temp2; i++, j++) {
        queue[i] = queue2[j];
    }
    queue[i] = 0;
    // Calculate the head movements
    for(j=0; j<=n+1; j++) {
        diff = abs(queue[j+1] - queue[j]);
        seek += diff;
        printf("Disk head moves from %d to %d with seek %d\n", queue[j], queue[j+1], diff);
    }
    printf("Total seek time is %d\n", seek);
}
