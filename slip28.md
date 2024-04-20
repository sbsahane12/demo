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
for(i = 1; i <= n; i++) {
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
for(i = 0; i < temp1 - 1; i++) {
for(j = i + 1; j < temp1; j++) {
if(queue1[i] > queue1[j]) {
temp = queue1[i];
queue1[i] = queue1[j];
queue1[j] = temp;
}
}
}

// Sort queue2 - increasing order
for(i = 0; i < temp2 - 1; i++) {
for(j = i + 1; j < temp2; j++) {
if(queue2[i] > queue2[j]) {
temp = queue2[i];
queue2[i] = queue2[j];
queue2[j] = temp;
}
}
}

// Join the two queues
for(i = 1, j = 0; j < temp1; i++, j++) {
queue[i] = queue1[j];
}
for(i = temp1 + 1, j = 0; j < temp2; i++, j++) {
queue[i] = queue2[j];
}

// Calculate the head movements
for(j = 0; j < n + 1; j++) {
diff = abs(queue[j + 1] - queue[j]);
seek += diff;
printf("Disk head moves from %d to %d with seek %d\n", queue[j], queue[j + 1], diff);
}

printf("Total seek time is %d\n", seek);
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
// Generate random numbers in each process
for(i = 0; i < ARRAY_SIZE; i++) {
array[i] = rand() % 100;
local_sum += array[i];
}
// Print the local sum of each process
printf("Local sum for process %d is %d\n", rank, local_sum);
// Reduce all of the local sums into the total sum
MPI_Reduce(&local_sum, &total_sum, 1, MPI_INT, MPI_SUM, 0, MPI_COMM_WORLD);
// Print the total sum once at the root
if (rank == 0) {
printf("Total sum = %d\n", total_sum);
}
// Finalize the MPI environment
MPI_Finalize();
return 0;
}
