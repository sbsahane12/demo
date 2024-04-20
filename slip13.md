#include<stdio.h>
#include<stdlib.h>

#define TRUE 1
#define FALSE 0

int m,n,max[10][10],alloc[10][10],avl[10],need[10][10],finish[10],i,j;

void computeneed() {
    for(i=0;i<m;i++)
        for(j=0;j<n;j++)
            need[i][j]=max[i][j]-alloc[i][j];
}

int isfeasible(int pno) {
    int cnt=0;
    for(j=0;j<n;j++)
        if(need[pno][j]<=avl[j])
            cnt++;
    if(cnt==n)
        return TRUE;
    else
        return FALSE;
}

void checksystem() {
    int ans[m],cnt=0,flag;
    for(i=0;i<m;i++)
        finish[i]=FALSE;
    while(1) {
        flag=FALSE;
        for(i=0;i<m;i++)
            if(!finish[i]) {
                printf("\n trying for p%d",i);
                if(isfeasible(i)) {
                    flag=TRUE;
                    printf("\n process p%d granted resources\n",i);
                    finish[i]=TRUE;
                    ans[cnt++]=i;
                    for(j=0;j<n;j++)
                        avl[j]=avl[j]+alloc[i][j];
                } else
                    printf("\nprocess p%d cannot be granted resources\n",i);
            }
        if(flag==FALSE)
            break;
    }
    flag=TRUE;
    for(i=0;i<m;i++)
        if(finish[i]==FALSE)
            flag=FALSE;
    if(flag==TRUE) {
        printf("\nSystem is in safe state\n");
        printf("\nSafe sequence is as follows\n");
        for(i=0;i<cnt;i++)
            printf("p%d\t",ans[i]);
    } else
        printf("\nSystem is not in safe state\n");
}

void acceptdata(int x[10][10]) {
    int i,j;
    for(i=0;i<m;i++) {
        printf("p%d\n",i);
        for(j=0;j<n;j++) {
            printf("%c:",65+j);
            scanf("%d",&x[i][j]);
        }
    }
}

void acceptavailability() {
    int i;
    for(i=0;i<n;i++) {
        printf("%c:",65+i);
        scanf("%d",&avl[i]);
    }
}

void displaydata() {
    int i,j;
    printf("\n\tallocation\t\tmax\t\tneed\n");
    printf("\t");
    for(i=0;i<m;i++) {
        for(j=0;j<n;j++)
            printf("%4c",65+j);
        printf("\t");
    }
    for(i=0;i<m;i++) {
        printf("\n p%d\t",i);
        for(j=0;j<n;j++)
            printf("%4d",alloc[i][j]);
        printf("\t");
        for(j=0;j<n;j++)
            printf("%4d",max[i][j]);
        printf("\t");
        for(j=0;j<n;j++)
            printf("%4d",need[i][j]);
    }
    printf("\n available\n");
    for(j=0;j<n;j++)
        printf("%4d",avl[j]);
}

int main() {
    printf("\n enter the no. of processes and resources");
    scanf("%d %d",&m,&n);
    printf("\n enter the allocation\n");
    acceptdata(alloc);
    printf("\n enter the max limit\n");
    acceptdata(max);
    printf("\n enter the availability\n");
    acceptavailability();
    computeneed();
    displaydata();
    checksystem();
    return 0;
}

#include<stdio.h>
#include<stdlib.h>

int main() {
    int queue[20],n,head,i,j,k,seek=0,max,diff,temp,queue1[20],queue2[20],temp1=0,temp2=0;
    float avg;
    printf("Enter the max range of disk\n");
    scanf("%d",&max);
    printf("Enter the initial head position\n");
    scanf("%d",&head);
    printf("Enter the size of queue request\n");
    scanf("%d",&n);
    printf("Enter the queue of disk positions to be read\n");
    for(i=1;i<=n;i++) {
        scanf("%d",&temp);
        if(temp>=head) {
            queue1[temp1]=temp;
            temp1++;
        } else {
            queue2[temp2]=temp;
            temp2++;
        }
    }
    for(i=0;i<temp1-1;i++) {
        for(j=i+1;j<temp1;j++) {
            if(queue1[i]>queue1[j]) {
                temp=queue1[i];
                queue1[i]=queue1[j];
                queue1[j]=temp;
            }
        }
    }
    for(i=0;i<temp2-1;i++) {
        for(j=i+1;j<temp2;j++) {
            if(queue2[i]<queue2[j]) {
                temp=queue2[i];
                queue2[i]=queue2[j];
                queue2[j]=temp;
            }
        }
    }
    for(i=1,j=0;j<temp1;i++,j++)
        queue[i]=queue1[j];
    queue[i]=max;
    for(i=temp1+2,j=0;j<temp2;i++,j++)
        queue[i]=queue2[j];
    queue[i]=0;
    queue[0]=head;
    for(j=0;j<=n+1;j++) {
        diff=abs(queue[j+1]-queue[j]);
        seek+=diff;
        printf("Disk head moves from %d to %d with %d\n",queue[j],queue[j+1],diff);
    }
    printf("Total seek time is %d\n",seek);
    avg=seek/(float)n;
    printf("Average seek time is %f\n",avg);
    return 0;
}
