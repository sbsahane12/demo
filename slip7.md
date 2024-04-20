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


#include<stdio.h>

int main() {
int queue[20], n, head, i, j, k, seek = 0, max, diff, temp, queue1[20], queue2[20], temp1 = 0, temp2 = 0;
float avg;

printf("Enter the max range of disk: ");
scanf("%d", &max);
printf("Enter the initial head position: ");
scanf("%d", &head);
printf("Enter the size of the request queue: ");
scanf("%d", &n);
printf("Enter the queue of disk positions to be read: ");
for (i = 0; i < n; i++) {
scanf("%d", &temp);
if (temp >= head) {
    queue1[temp1] = temp;
    temp1++;
} else {
    queue2[temp2] = temp;
    temp2++;
}
}
for (i = 0; i < temp1 - 1; i++) {
for (j = i + 1; j < temp1; j++) {
    if (queue1[i] > queue1[j]) {
        temp = queue1[i];
        queue1[i] = queue1[j];
        queue1[j] = temp;
    }
}
}

for (i = 0; i < temp2 - 1; i++) {
for (j = i + 1; j < temp2; j++) {
    if (queue2[i] < queue2[j]) {
        temp = queue2[i];
        queue2[i] = queue2[j];
        queue2[j] = temp;
    }
}
}

for (i = 1, j = 0; j < temp1; i++, j++)
queue[i] = queue1[j];
queue[i] = max;
for (i = temp1 + 2, j = 0; j < temp2; i++, j++)
queue[i] = queue2[j];
queue[i] = 0;
queue[0] = head;

for (j = 0; j <= n + 1; j++) {
diff = abs(queue[j + 1] - queue[j]);
seek += diff;
printf("Disk head moves from %d to %d with %d\n", queue[j], queue[j + 1], diff);
}

printf("Total seek time is %d\n", seek-14);
avg = seek / (float)n;
printf("Average seek time is %.2f\n", avg);

return 0;
}
