#include <stdio.h>
#define TRUE 1
#define FALSE 0

int m, n, max[10][10], alloc[10][10], avail[10], need[10][10], finish[10], i, j;

void computeNeed() {
    for (i = 0; i < m; i++)
        for (j = 0; j < n; j++)
            need[i][j] = max[i][j] - alloc[i][j];
}

int isFeasible(int pno) {
    int cnt = 0;
    for (j = 0; j < n; j++)
        if (need[pno][j] <= avail[j])
            cnt++;
    return (cnt == n);
}

void checkSystem() {
    int ans[m], cnt = 0, flag;
    while (TRUE) {
        flag = FALSE;
        for (i = 0; i < m; i++)
            if (!finish[i]) {
                if (isFeasible(i)) {
                    flag = TRUE;
                    finish[i] = TRUE;
                    ans[cnt++] = i;
                    for (j = 0; j < n; j++)
                        avail[j] += alloc[i][j];
                }
            }
        if (!flag)
            break;
    }
    flag = TRUE;
    for (i = 0; i < m; i++)
        if (!finish[i])
            flag = FALSE;
    if (flag) {
        printf("\n System is in safe state\n");
        printf("\n Safe sequence is as follows\n");
        for (i = 0; i < cnt; i++)
            printf("p%d\t", ans[i]);
    } else
        printf("\n System is not in safe state\n");
}

void acceptData(int x[10][10]) {
    for (i = 0; i < m; i++) {
        for (j = 0; j < n; j++) {
            scanf("%d", &x[i][j]);
        }
    }
}

void displayData() {
    printf("\n\tAllocation\tMax\tNeed\n");
    for (i = 0; i < m; i++) {
        printf("\n p%d\t", i);
        for (j = 0; j < n; j++)
            printf("%4d", alloc[i][j]);
        printf("\t");
        for (j = 0; j < n; j++)
            printf("%4d", max[i][j]);
        printf("\t");
        for (j = 0; j < n; j++)
            printf("%4d", need[i][j]);
    }
    printf("\n Available\n");
    for (j = 0; j < n; j++)
        printf("%4d", avail[j]);
}

int main() {
    printf("\n Enter the number of processes and resources");
    scanf("%d %d", &m, &n);
    printf("\n Enter the allocation\n");
    acceptData(alloc);
    printf("\n Enter the max limit\n");
    acceptData(max);
    printf("\n Enter the availability\n");
    for (i = 0; i < n; i++)
        scanf("%d", &avail[i]);
    computeNeed();
    displayData();
    checkSystem();
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
