#include <mpi.h>
#include <stdio.h>
#include <stdlib.h>
#include <time.h>

#define ARRAY_SIZE 1000

int main(int argc, char* argv[]) {
int rank, size, i;
int array[ARRAY_SIZE];
int local_sum = 0, total_sum = 0;

// Initialize the MPI environment
MPI_Init(&argc, &argv);
// Get the number of processes
MPI_Comm_size(MPI_COMM_WORLD, &size);
// Get the rank of the process
MPI_Comm_rank(MPI_COMM_WORLD, &rank);

// Seed the random number generator to get different results each time
srand(rank + time(NULL));
// Generate random numbers in each process and add to local sum if odd
for (i = 0; i < ARRAY_SIZE; i++) {
array[i] = rand() % 100;
if (array[i] % 2 != 0) {
local_sum += array[i];
}
}

// Reduce all of the local sums into the total sum
MPI_Reduce(&local_sum, &total_sum, 1, MPI_INT, MPI_SUM, 0, MPI_COMM_WORLD);

// Print the local sum of each process
printf("Local sum for process %d is %d\n", rank, local_sum);
// Print the total sum once at the root
if (rank == 0) {
printf("Total sum of odd numbers = %d\n", total_sum);
}

// Finalize the MPI environment
MPI_Finalize();
return 0;
}

#include <stdio.h>
#include <stdbool.h>

#define MAX_PROC 10
#define MAX_RES 10

int m, n; // m - number of processes, n - number of resources
int max[MAX_PROC][MAX_RES], alloc[MAX_PROC][MAX_RES], avl[MAX_RES], need[MAX_PROC][MAX_RES];
bool finish[MAX_PROC];

void computeNeed() {
for (int i = 0; i < m; i++) {
for (int j = 0; j < n; j++) {
need[i][j] = max[i][j] - alloc[i][j];
}
}
}

bool isFeasible(int pno) {
int cnt = 0;
for (int j = 0; j < n; j++) {
if (need[pno][j] <= avl[j]) {
cnt++;
}
}
return cnt == n;
}

void checkSystem() {
int ans[MAX_PROC], cnt = 0;
bool flag;
for (int i = 0; i < m; i++)
finish[i] = false;

while (true) {
flag = false;
for (int i = 0; i < m; i++) {
if (!finish[i]) {
printf("\nTrying for P%d\n", i);
if (isFeasible(i)) {
flag = true;
printf("Process P%d granted resources\n", i);
finish[i] = true;
ans[cnt++] = i;
for (int j = 0; j < n; j++) {
avl[j] += alloc[i][j];
}
} else {
printf("Process P%d cannot be granted resources\n", i);
}
}
}
if (!flag)
break;
}

flag = true;
for (int i = 0; i < m; i++) {
if (!finish[i]) {
flag = false;
break;
}
}

if (flag) {
printf("\nSystem is in safe state\n");
printf("Safe sequence is as follows:\n");
for (int i = 0; i < cnt; i++) {
printf("P%d ", ans[i]);
}
printf("\n");
} else {
printf("\nSystem is not in safe state\n");
}
}

void acceptData(int x[MAX_PROC][MAX_RES]) {
for (int i = 0; i < m; i++) {
printf("P%d\n", i);
for (int j = 0; j < n; j++) {
printf("%c: ", 65 + j);
scanf("%d", &x[i][j]);
}
}
}

void acceptAvailability() {
for (int i = 0; i < n; i++) {
printf("%c: ", 65 + i);
scanf("%d", &avl[i]);
}
}

void displayData() {
printf("\nAllocation\tMax\tNeed\n");
for (int i = 0; i < m; i++) {
printf("P%d\t", i);
for (int j = 0; j < n; j++) {
printf("%d ", alloc[i][j]);
}
printf("\t");
for (int j = 0; j < n; j++) {
printf("%d ", max[i][j]);
}
printf("\t");
for (int j = 0; j < n; j++) {
printf("%d ", need[i][j]);
}
printf("\n");
}
printf("\nAvailable: ");
for (int i = 0; i < n; i++) {
printf("%d ", avl[i]);
}
printf("\n");
}

int main() {
printf("\nEnter the number of processes and resources: ");
scanf("%d %d", &m, &n);
printf("\nEnter the allocation:\n");
acceptData(alloc);
printf("\nEnter the max limit:\n");
acceptData(max);
printf("\nEnter the availability:\n");
acceptAvailability();
computeNeed();
displayData();
checkSystem();
return 0;
}
