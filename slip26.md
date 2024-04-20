#include<stdio.h>
#include<stdbool.h> // Include stdbool.h for boolean type
#define MAX_PROC 10
#define MAX_RES 10

int m, n; // Number of processes and resources
int max[MAX_PROC][MAX_RES], alloc[MAX_PROC][MAX_RES], avl[MAX_RES], need[MAX_PROC][MAX_RES], finish[MAX_PROC];

// Function to compute need matrix
void computeneed() {
for(int i = 0; i < m; i++)
for(int j = 0; j < n; j++)
need[i][j] = max[i][j] - alloc[i][j];
}

// Function to check if the process can be granted resources
bool isfeasible(int pno) {
int cnt = 0;
for(int j = 0; j < n; j++)
if(need[pno][j] <= avl[j])
cnt++;
return (cnt == n);
}

// Function to check the system's safety
void checksystem() {
int ans[MAX_PROC], cnt = 0;
bool flag;

for(int i = 0; i < m; i++)
finish[i] = false;

while(true) {
flag = false;
for(int i = 0; i < m; i++) {
if(!finish[i]) {
printf("\nTrying for p%d", i);
if(isfeasible(i)) {
    flag = true;
    printf("\nProcess p%d granted resources\n", i);
    finish[i] = true;
    ans[cnt++] = i;
    for(int j = 0; j < n; j++)
        avl[j] += alloc[i][j];
} else {
    printf("\nProcess p%d cannot be granted resources\n", i);
}
}
}
if(!flag)
break;
}

flag = true;
for(int i = 0; i < m; i++)
if(!finish[i])
flag = false;

if(flag) {
printf("\nSystem is in safe state\n");
printf("\nSafe sequence is as follows\n");
for(int i = 0; i < cnt; i++)
printf("p%d\t", ans[i]);
} else {
printf("\nSystem is not in safe state\n");
}
}

// Function to accept allocation and maximum limit
void acceptdata(int x[MAX_PROC][MAX_RES]) {
for(int i = 0; i < m; i++) {
printf("p%d\n", i);
for(int j = 0; j < n; j++) {
printf("%c: ", 65 + j);
scanf("%d", &x[i][j]);
}
}
}

// Function to accept availability
void acceptavailability() {
for(int i = 0; i < n; i++) {
printf("%c: ", 65 + i);
scanf("%d", &avl[i]);
}
}

// Function to display allocation, maximum, and need matrices
void displaydata() {
printf("\nAllocation\tMax\tNeed\n");
for(int i = 0; i < m; i++) {
printf("p%d\t", i);
for(int j = 0; j < n; j++)
printf("%d ", alloc[i][j]);
printf("\t");
for(int j = 0; j < n; j++)
printf("%d ", max[i][j]);
printf("\t");
for(int j = 0; j < n; j++)
printf("%d ", need[i][j]);
printf("\n");
}
printf("\nAvailable\n");
for(int j = 0; j < n; j++)
printf("%d ", avl[j]);
printf("\n");
}

int main() {
printf("\nEnter the number of processes and resources: ");
scanf("%d %d", &m, &n);

printf("\nEnter the allocation:\n");
acceptdata(alloc);

printf("\nEnter the maximum limit:\n");
acceptdata(max);

printf("\nEnter the availability:\n");
acceptavailability();

computeneed();

displaydata();

checksystem();

return 0;
}
#include <stdio.h>
#include <stdlib.h>
#include <math.h>

int main() {
int i, n, req[50], mov = 0, cp;

printf("Enter the current position of the head: ");
scanf("%d", &cp);

printf("Enter the number of requests: ");
scanf("%d", &n);

printf("Enter the request order:\n");
for (i = 0; i < n; i++) {
scanf("%d", &req[i]);
}

// Calculate total head movement
mov = abs(cp - req[0]);
printf("%d -> %d", cp, req[0]);
for (i = 1; i < n; i++) {
mov += abs(req[i] - req[i - 1]);
printf(" -> %d", req[i]);
}
printf("\n");

// Print total head movement
printf("Total head movement = %d\n", mov);

return 0;
}
