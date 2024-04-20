#include<stdio.h>
#include<stdbool.h>

#define M 10
#define N 10

int m, n, max[M][N], alloc[M][N], avl[N], need[M][N], finish[M];

void computeNeed() {
for (int i = 0; i < m; i++)
for (int j = 0; j < n; j++)
    need[i][j] = max[i][j] - alloc[i][j];
}

bool isFeasible(int pno) {
int cnt = 0;
for (int j = 0; j < n; j++)
if (need[pno][j] <= avl[j])
    cnt++;
return cnt == n;
}

void checkSystem() {
int ans[M], cnt = 0;
bool flag;
for (int i = 0; i < m; i++)
finish[i] = 0;

while (1) {
flag = false;
for (int i = 0; i < m; i++) {
    if (!finish[i]) {
        printf("\nTrying for p%d", i);
        if (isFeasible(i)) {
            flag = true;
            printf("\nProcess p%d granted resources\n", i);
            finish[i] = 1;
            ans[cnt++] = i;
            for (int j = 0; j < n; j++)
                avl[j] += alloc[i][j];
        } else {
            printf("\nProcess p%d cannot be granted resources\n", i);
        }
    }
}
if (!flag)
    break;
}

flag = true;
for (int i = 0; i < m; i++)
if (!finish[i])
    flag = false;
if (flag) {
printf("\nSystem is in safe state\n");
printf("\nSafe sequence is as follows\n");
for (int i = 0; i < cnt; i++)
    printf("p%d\t", ans[i]);
} else {
printf("\nSystem is not in safe state\n");
}
}

void acceptData(int x[M][N]) {
for (int i = 0; i < m; i++) {
printf("p%d\n", i);
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
printf("\n\tAllocation\t\tMax\t\tNeed\n");
printf("\t");
for (int i = 0; i < m; i++) {
for (int j = 0; j < n; j++)
    printf("%4c", 65 + j);
printf("\t");
}
for (int i = 0; i < m; i++) {
printf("\n p%d\t", i);
for (int j = 0; j < n; j++)
    printf("%4d", alloc[i][j]);
printf("\t");
for (int j = 0; j < n; j++)
    printf("%4d", max[i][j]);
printf("\t");
for (int j = 0; j < n; j++)
    printf("%4d", need[i][j]);
}
printf("\n Available\n");
for (int j = 0; j < n; j++)
printf("%4d", avl[j]);
}

int main() {
printf("\nEnter the number of processes and resources: ");
scanf("%d %d", &m, &n);
printf("\nEnter the allocation: \n");
acceptData(alloc);
printf("\nEnter the max limit: \n");
acceptData(max);
printf("\nEnter the availability: \n");
acceptAvailability();
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
printf("Enter the size of queue request: ");
scanf("%d", &n);
printf("Enter the queue of disk positions to be read: ");
for (i = 1; i <= n; i++) {
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
    if (queue2[i] > queue2[j]) {
        temp = queue2[i];
        queue2[i] = queue2[j];
        queue2[j] = temp;
    }
}
}
for (i = 1, j = 0; j < temp1; i++, j++)
queue[i] = queue1[j];
queue[i] = max;
queue[i + 1] = 0;
for (i = temp1 + 3, j = 0; j < temp2; i++, j++)
queue[i] = queue2[j];
queue[0] = head;
for (j = 0; j <= n + 1; j++) {
diff = abs(queue[j + 1] - queue[j]);
seek += diff;
printf("Disk head moves from %d to %d with %d\n", queue[j], queue[j + 1], diff);
}
printf("Total seek time is %d\n", seek);
avg = seek / (float) n;
printf("Average seek time is %f\n", avg);
return 0;
}


