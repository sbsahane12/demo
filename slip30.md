#include <mpi.h>
#include <stdio.h>
#include <stdlib.h>
#include <time.h>

#define ARRAY_SIZE 1000

int main(int argc, char* argv[]) {
int rank, size, i;
int array[ARRAY_SIZE];
int local_min = 100, global_min = 100;

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

#include<stdio.h>
#include<stdlib.h>
#include<string.h>
#include<time.h>

void main() {
int n, req[50], mov = 0, cp;
printf("enter the current position: ");
scanf("%d", &cp);
printf("enter the number of requests: ");
scanf("%d", &n);
printf("enter the request order: ");
for(int i = 0; i < n; i++) {
scanf("%d", &req[i]);
}
mov = mov + abs(cp - req[0]);
printf("%d -> %d", cp, req[0]);
for(int i = 1; i < n; i++) {
mov = mov + abs(req[i] - req[i - 1]);
printf(" -> %d", req[i]);
}
printf("\n");
printf("total head movement = %d\n", mov);
}
