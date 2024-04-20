#include<stdio.h>
#define true 1
#define false 0

int m, n, max[10][10], alloc[10][10], avl[10], need[10][10], finish[10], i, j;

void computeneed() {
    for (i = 0; i < m; i++)
        for (j = 0; j < n; j++)
            need[i][j] = max[i][j] - alloc[i][j];
}

int isfeasible(int pno) {
    int cnt = 0;
    for (j = 0; j < n; j++)
        if (need[pno][j] <= avl[j])
            cnt++;
    if (cnt == n)
        return 1;
    else
        return 0;
}

void checksystem() {
    int ans[m], cnt = 0, flag;
    for (i = 0; i < m; i++)
        finish[i] = false;
    while (true) {
        flag = false;
        for (i = 0; i < m; i++)
            if (!finish[i]) {
                printf("\nTrying for p%d", i);
                if (isfeasible(i)) {
                    flag = true;
                    printf("\nProcess p%d granted resources\n", i);
                    finish[i] = true;
                    ans[cnt++] = i;
                    for (j = 0; j < n; j++)
                        avl[j] = avl[j] + alloc[i][j];
                } else
                    printf("\nProcess p%d cannot be granted resources\n", i);
            }
        if (flag == false)
            break;
    }
    flag = true;
    for (i = 0; i < m; i++)
        if (finish[i] == 0)
            flag = false;
    if (flag == 1) {
        printf("\nSystem is in safe state\n");
        printf("\nSafe sequence is as follows\n");
        for (i = 0; i < cnt; i++)
            printf("p%d\t", ans[i]);
    } else
        printf("\nSystem is not in safe state\n");
}

void acceptdata(int x[10][10]) {
    int i, j;
    for (i = 0; i < m; i++) {
        printf("p%d\n", i);
        for (j = 0; j < n; j++) {
            printf("%c: ", 65 + j);
            scanf("%d", &x[i][j]);
        }
    }
}

void acceptavailability() {
    int i;
    for (i = 0; i < n; i++) {
        printf("%c: ", 65 + i);
        scanf("%d", &avl[i]);
    }
}

void displaydata() {
    int i, j;
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
    printf("\nAvailable\n");
    for (j = 0; j < n; j++)
        printf("%4d", avl[j]);
}

int main() {
    printf("\nEnter the number of processes and resources: ");
    scanf("%d %d", &m, &n);
    printf("\nEnter the allocation:\n");
    acceptdata(alloc);
    printf("\nEnter the max limit:\n");
    acceptdata(max);
    printf("\nEnter the availability:\n");
    acceptavailability();
    computeneed();
    displaydata();
    checksystem();
    return 0;
}
