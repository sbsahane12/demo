#include<stdio.h>
#include<stdlib.h>
#include<string.h>
#include<time.h>

struct dirfile {
    char fname[20];
    int startblk, length;
} direntry[20];

int bv[64];
int used=0;
int totalfile=0;
int n;

void initialize() {
    int i;
    srand(time(NULL));
    for(i=0; i<n; i++) {
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
    printf("block number \t status\n");
    for(i=0; i<n; i++) {
        printf("%d\t\t", i);
        if(bv[i]==0) {
            printf("allocated\n");
        } else {
            printf("free\n");
        }
    }
}

int search(int length) {
    int i, j, flag=1, blknum;
    for(i=0; i<n; i++) {
        if(bv[i]==1) {
            flag=1;
            for(blknum=i, j=0; j<length; j++) {
                if(bv[blknum]==1) {
                    blknum++;
                    continue;
                } else {
                    flag=0;
                    break;
                }
            }
            if(flag==1)
                return i;
        }
    }
    return -1;
}

void createfile() {
    char fname[10];
    int length, blknum, k;
    printf("\n enter file name:");
    scanf("%s", &fname);
    printf("\n enter the length of file:");
    scanf("%d", &length);
    if(length<=n-used) {
        blknum=search(length);
    } else {
        blknum=-1;
    }
    if(blknum==-1) {
        printf("error: no disk space available\n");
    } else {
        printf("\nblock allocated\n");
        used=used+length;
        for(k=blknum; k<(blknum+length); k++) {
            bv[k]=0;
        }
        k=totalfile++;
        strcpy(direntry[k].fname, fname);
        direntry[k].startblk=blknum;
        direntry[k].length=length;
    }
}

void displaydir() {
    int k;
    printf("\tfilename\tstart\tsize\n");
    for(k=0; k<totalfile; k++) {
        printf("%s\t%d\t%d\n", direntry[k].fname, direntry[k].startblk, direntry[k].length);
    }
    printf("\nused block=%d", used);
    printf("\nfree block=%d", n-used);
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
                printf("exiting...\n"); 
                break;
            default:
                printf("error:invalid choice\n");
                break;
        }
    } while(choice!=4);
    return 0;
}


#include <mpi.h>
#include <stdio.h>
#include <stdlib.h>
#include <time.h>
#include <limits.h>

#define ARRAY_SIZE 1000

int main(int argc, char* argv[]) {
    int rank, size, i;
    int array[ARRAY_SIZE];
    int local_min = INT_MAX;
    int global_min;

// Initialize the MPI environment
MPI_Init(&argc, &argv);
// Get the number of processes
MPI_Comm_size(MPI_COMM_WORLD, &size);
// Get the rank of the process
MPI_Comm_rank(MPI_COMM_WORLD, &rank);

// Seed the random number generator to get different results each time
srand(rank + time(NULL));

// Generate random numbers in each process
for(i = 0; i < ARRAY_SIZE; i++) {
    array[i] = rand() % 100;
    if(array[i] < local_min) {
        local_min = array[i];
    }
}

// Print the local min of each process
printf("Local min for process %d is %d\n", rank, local_min);

// Reduce all of the local minima into the global min
MPI_Reduce(&local_min, &global_min, 1, MPI_INT, MPI_MIN, 0, MPI_COMM_WORLD);

// Print the global min once at the root
if (rank == 0) {
    printf("Global min = %d\n", global_min);
}

// Finalize the MPI environment
MPI_Finalize();

return 0;
}
