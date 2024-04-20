#include <stdio.h>
#include <stdlib.h>
#include <math.h>

int main() {
int i, n, req[50], mov = 0, cp;

printf("Enter the current position: ");
scanf("%d", &cp);

printf("Enter the number of requests: ");
scanf("%d", &n);

printf("Enter the request order:\n");
for (i = 0; i < n; i++) {
scanf("%d", &req[i]);
}

mov += abs(cp - req[0]);
printf("%d -> %d", cp, req[0]);

for (i = 1; i < n; i++) {
mov += abs(req[i] - req[i - 1]);
printf(" -> %d", req[i]);
}

printf("\n");
printf("Total head movement = %d\n", mov);

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
int local_sum = 0, total_sum = 0;

// Initialize the MPI environment
MPI_Init(&argc, &argv);
// Get the number of processes
MPI_Comm_size(MPI_COMM_WORLD, &size);
// Get the rank of the process
MPI_Comm_rank(MPI_COMM_WORLD, &rank);

// Seed the random number generator to get different results each time
srand(rank + time(NULL));

// Generate random numbers in each process and add to local sum if even
for (i = rank; i < ARRAY_SIZE; i += size) {
array[i] = rand() % 100;
if (array[i] % 2 == 0) {
local_sum += array[i];
}
}

// Reduce all of the local sums into the total sum
MPI_Reduce(&local_sum, &total_sum, 1, MPI_INT, MPI_SUM, 0, MPI_COMM_WORLD);

// Print the local sum of each process
printf("Local sum for process %d is %d\n", rank, local_sum);

// Print the total sum once at the root
if (rank == 0) {
printf("Total sum = %d\n", total_sum);
}

// Finalize the MPI environment
MPI_Finalize();
return 0;
}
