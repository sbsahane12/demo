#include<stdio.h>
#include<stdbool.h>

#define MAX_PROC 10
#define MAX_RES 10

int m, n; // m - number of processes, n - number of resources
int max[MAX_PROC][MAX_RES], alloc[MAX_PROC][MAX_RES], avl[MAX_RES], need[MAX_PROC][MAX_RES];
bool finish[MAX_PROC];

void computeNeed() {
for(int i = 0; i < m; i++) {
    for(int j = 0; j < n; j++) {
        need[i][j] = max[i][j] - alloc[i][j];
    }
}
}

bool isFeasible(int pno) {
int cnt = 0;
for(int j = 0; j < n; j++) {
    if(need[pno][j] <= avl[j]) {
        cnt++;
    }
}
return cnt == n;
}

void checkSystem() {
int ans[MAX_PROC], cnt = 0;
bool flag;
for(int i = 0; i < m; i++)
    finish[i] = false;

while(true) {
    flag = false;
    for(int i = 0; i < m; i++) {
        if(!finish[i]) {
            printf("\nTrying for P%d\n", i);
            if(isFeasible(i)) {
                flag = true;
                printf("Process P%d granted resources\n", i);
                finish[i] = true;
                ans[cnt++] = i;
                for(int j = 0; j < n; j++) {
                    avl[j] += alloc[i][j];
                }
            } else {
                printf("Process P%d cannot be granted resources\n", i);
            }
        }
    }
    if(!flag)
        break;
}

flag = true;
for(int i = 0; i < m; i++) {
    if(!finish[i]) {
        flag = false;
        break;
    }
}

if(flag) {
    printf("\nSystem is in safe state\n");
    printf("Safe sequence is as follows:\n");
    for(int i = 0; i < cnt; i++) {
        printf("P%d ", ans[i]);
    }
    printf("\n");
} else {
    printf("\nSystem is not in safe state\n");
}
}

void acceptData(int x[MAX_PROC][MAX_RES]) {
for(int i = 0; i < m; i++) {
    printf("P%d\n", i);
    for(int j = 0; j < n; j++) {
        printf("%c: ", 65 + j);
        scanf("%d", &x[i][j]);
    }
}
}

void acceptAvailability() {
for(int i = 0; i < n; i++) {
    printf("%c: ", 65 + i);
    scanf("%d", &avl[i]);
}
}

void displayData() {
printf("\nAllocation\tMax\tNeed\n");
for(int i = 0; i < m; i++) {
    printf("P%d\t", i);
    for(int j = 0; j < n; j++) {
        printf("%d ", alloc[i][j]);
    }
    printf("\t");
    for(int j = 0; j < n; j++) {
        printf("%d ", max[i][j]);
    }
    printf("\t");
    for(int j = 0; j < n; j++) {
        printf("%d ", need[i][j]);
    }
    printf("\n");
}
printf("\nAvailable: ");
for(int i = 0; i < n; i++) {
    printf("%d ", avl[i]);
}
printf("\n");
}

int main() {
printf("Enter the number of processes and resources: ");
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

#include<stdio.h>
#include<stdlib.h>

int main() {
int n, cp, mov = 0, req[50], cp1, index[50], a[50], j = 0;

printf("Enter the current position: ");
scanf("%d", &cp);
cp1 = cp;

printf("Enter the number of requests: ");
scanf("%d", &n);

printf("Enter the request order: ");
for(int i = 0; i < n; i++) {
    scanf("%d", &req[i]);
}

for(int k = 0; k < n; k++) {
    for(int i = 0; i < n; i++) {
        index[i] = abs(cp - req[i]);
    }
    int min = index[0], mini = 0;
    for(int i = 1; i < n; i++) {
        if(min > index[i]) {
            min = index[i];
            mini = i;
        }
    }
    a[j++] = req[mini];
    cp = req[mini];
    req[mini] = 999;
}

printf("Sequence is: ");
printf("%d", cp1);
mov = mov + abs(cp1 - a[0]);
printf(" -> %d", a[0]);
for(int i = 1; i < n; i++) {
    mov = mov + abs(a[i] - a[i - 1]);
    printf(" -> %d", a[i]);
}
printf("\nTotal head movement: %d\n", mov);

return 0;
}
