
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
    scanf("%s", &fname);
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

#include <mpi.h>
#include <stdio.h>
#include <stdlib.h>
#include <time.h>

#define ARRAY_SIZE 1000

int main(int argc, char* argv[]) {
    int rank, size, i;
    int array[ARRAY_SIZE];
    int local_sum = 0, total_sum;
    
    MPI_Init(&argc, &argv);
    MPI_Comm_size(MPI_COMM_WORLD, &size);
    MPI_Comm_rank(MPI_COMM_WORLD, &rank);
    
    srand(rank + time(NULL));
    
    for (i = 0; i < ARRAY_SIZE; i++) {
        array[i] = rand() % 100;
        local_sum += array[i];
    }
    
    printf("Local sum for process %d is %d\n", rank, local_sum);
    
    MPI_Reduce(&local_sum, &total_sum, 1, MPI_INT, MPI_SUM, 0, MPI_COMM_WORLD);
    
    if (rank == 0) {
        printf("Total sum = %d\n", total_sum);
    }
    
    MPI_Finalize();
    return 0;
}
