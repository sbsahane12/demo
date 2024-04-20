#include<stdio.h>
#include<stdlib.h>
#include<stdbool.h> 
#define MAX_PROCESSES 10
#define MAX_RESOURCES 10

int m, n, max[MAX_PROCESSES][MAX_RESOURCES], alloc[MAX_PROCESSES][MAX_RESOURCES], avl[MAX_RESOURCES], need[MAX_PROCESSES][MAX_RESOURCES], finish[MAX_PROCESSES], i, j;

void computeNeed() {
for (i = 0; i < m; i++)
for (j = 0; j < n; j++)
need[i][j] = max[i][j] - alloc[i][j];
}

bool isFeasible(int pno) {
int cnt = 0;
for (j = 0; j < n; j++)
if (need[pno][j] <= avl[j])
cnt++;
return (cnt == n);
}

void checkSystem() {
int ans[MAX_PROCESSES], cnt = 0;
bool flag;
for (i = 0; i < m; i++)
finish[i] = false;
do {
flag = false;
for (i = 0; i < m; i++) {
if (!finish[i]) {
printf("\nTrying for p%d", i);
if (isFeasible(i)) {
flag = true;
printf("\nProcess p%d granted resources\n", i);
finish[i] = true;
ans[cnt++] = i;
for (j = 0; j < n; j++)
avl[j] += alloc[i][j];
} else
printf("\nProcess p%d cannot be granted resources\n", i);
}
}
} while (flag);
flag = true;
for (i = 0; i < m; i++)
if (!finish[i])
flag = false;
if (flag) {
printf("\nSystem is in safe state\n");
printf("\nSafe sequence is as follows\n");
for (i = 0; i < cnt; i++)
printf("p%d\t", ans[i]);
} else
printf("\nSystem is not in safe state\n");
}

void acceptData(int x[MAX_PROCESSES][MAX_RESOURCES]) {
printf("\nEnter data:\n");
for (i = 0; i < m; i++) {
printf("p%d\n", i);
for (j = 0; j < n; j++) {
printf("%c:", 65 + j);
scanf("%d", &x[i][j]);
}
}
}

void acceptAvailability() {
printf("\nEnter availability:\n");
for (i = 0; i < n; i++) {
printf("%c:", 65 + i);
scanf("%d", &avl[i]);
}
}

void displayData() {
printf("\n\tAllocation\t\tMax\t\tNeed\n");
printf("\t");
for (i = 0; i < m; i++) {
for (j = 0; j < n; j++)
printf("%4c", 65 + j);
printf("\t");
}
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
printf("%4d", avl[j]);
}

int main() {
printf("\nEnter the number of processes and resources: ");
scanf("%d %d", &m, &n);
acceptData(alloc);
printf("\nEnter the max limit: ");
acceptData(max);
acceptAvailability();
computeNeed();
displayData();
checkSystem();
return 0;
}

#include <stdio.h>
#include <stdlib.h>
#include <mpi.h>

#define ARRAY_SIZE 1000

int main(int argc, char *argv[]) {
int rank, size;
int local_min = 100; 
int global_min = 0;

MPI_Init(&argc, &argv);
MPI_Comm_rank(MPI_COMM_WORLD, &rank);
MPI_Comm_size(MPI_COMM_WORLD, &size);


for (int i = 0; i < ARRAY_SIZE; i++) {
int random_num = rand() % 100;
local_min = (local_min < random_num) ? local_min : random_num;
}


MPI_Reduce(&local_min, &global_min, 1, MPI_INT, MPI_MIN, 0, MPI_COMM_WORLD);


if (rank == 0) {
printf("Minimum number in the array is: %d\n", global_min);
}

MPI_Finalize();
return 0;
}
