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
    // Sort queue1 - increasing order
    for (i = 0; i < temp1 - 1; i++) {
        for (j = i + 1; j < temp1; j++) {
            if (queue1[i] > queue1[j]) {
                temp = queue1[i];
                queue1[i] = queue1[j];
                queue1[j] = temp;
            }
        }
    }
    // Sort queue2 - increasing order
    for (i = 0; i < temp2 - 1; i++) {
        for (j = i + 1; j < temp2; j++) {
            if (queue2[i] > queue2[j]) {
                temp = queue2[i];
                queue2[i] = queue2[j];
                queue2[j] = temp;
            }
        }
    }
    // Join the two queues
    for (i = 1, j = 0; j < temp1; i++, j++) {
        queue[i] = queue1[j];
    }
    queue[i] = max;
    for (i = temp1 + 2, j = 0; j < temp2; i++, j++) {
        queue[i] = queue2[j];
    }
    queue[i] = 0;
    // Calculate the head movements
    for (j = 0; j <= n + 1; j++) {
        diff = abs(queue[j + 1] - queue[j]);
        seek += diff;
        printf("Disk head moves from %d to %d with seek %d\n", queue[j], queue[j + 1], diff);
    }
    printf("Total seek time is %d\n", seek);
}

