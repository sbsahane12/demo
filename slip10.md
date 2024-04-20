#include <mpi.h>
#include <stdio.h>
#include <stdlib.h>
#include <time.h> // Include <time.h> for time() function

#define ARRAY_SIZE 1000

int main(int argc, char* argv[]) {
  int rank, size, i;
  int array[ARRAY_SIZE];
  int local_sum = 0, total_sum;
  float average;
  
  // Initialize the MPI environment
  MPI_Init(&argc, &argv);
  // Get the number of processes
  MPI_Comm_size(MPI_COMM_WORLD, &size);
  // Get the rank of the process
  MPI_Comm_rank(MPI_COMM_WORLD, &rank);

  // Seed the random number generator to get different results for each process
  srand(rank + time(NULL)); // Use rank as part of the seed
  
  // Generate random numbers in each process
  for(i = 0; i < ARRAY_SIZE; i++) {
      array[i] = rand() % 100;
      local_sum += array[i];
  }

  // Print the local sum of each process
  printf("Local sum for process %d is %d\n", rank, local_sum);

  // Reduce all of the local sums into the total sum
  MPI_Reduce(&local_sum, &total_sum, 1, MPI_INT, MPI_SUM, 0, MPI_COMM_WORLD);

  // Calculate the average
  average = total_sum / (float)ARRAY_SIZE; // Correct calculation

  // Print the total sum and average once at the root
  if (rank == 0) {
      printf("Total sum = %d\n", total_sum);
      printf("Average = %.2f\n", average);
  }

  // Finalize the MPI environment
  MPI_Finalize();
  return 0;
}


#include<stdio.h>

int main() {
  int queue[20], n, head, i, j, k, seek = 0, max, diff, temp, queue1[20], queue2[20], temp1 = 0, temp2 = 0;
  float avg;
  
  printf("Enter the max range of disk\n");
  scanf("%d", &max);
  
  printf("Enter the initial head position\n");
  scanf("%d", &head);
  
  printf("Enter the size of queue request\n");
  scanf("%d", &n);
  
  printf("Enter the queue of disk positions to be read\n");
  for(i = 1; i <= n; i++) {
      scanf("%d", &temp);
      if(temp >= head) {
          queue1[temp1] = temp;
          temp1++;
      } else {
          queue2[temp2] = temp;
          temp2++;
      }
  }

  // Sorting queues omitted for brevity

  for(i = 1, j = 0; j < temp1; i++, j++)
      queue[i] = queue1[j];
  queue[i] = max;
  queue[i + 1] = 0;
  for(i = temp1 + 3, j = 0; j < temp2; i++, j++)
      queue[i] = queue2[j];
  queue[0] = head;
  
  for(j = 0; j <= n + 1; j++) {
      diff = abs(queue[j + 1] - queue[j]);
      seek += diff;
      printf("Disk head moves from %d to %d with %d\n", queue[j], queue[j + 1], diff); // Fix printf formatting
  }
  printf("Total seek time is %d\n", seek);
  
  avg = seek / (float)n;
  printf("Average seek time is %f\n", avg);
  
  return 0;
}
