#include <stdio.h>
#include <stdlib.h>

#define AGING_FACTOR 1

typedef struct {
    int pid;
    int burst_time;
    int priority;
    int waiting_time;
    int turnaround_time;
} Process;

void calculate_waiting_time(Process processes[], int n) {
    processes[0].waiting_time = 0;
    for (int i = 1; i < n; i++) {
        processes[i].waiting_time = processes[i - 1].waiting_time + processes[i - 1].burst_time;
    }
}

void calculate_turnaround_time(Process processes[], int n) {
    for (int i = 0; i < n; i++) {
        processes[i].turnaround_time = processes[i].waiting_time + processes[i].burst_time;
    }
}

void sort_by_burst_time(Process processes[], int n) {
    for (int i = 0; i < n - 1; i++) {
        for (int j = 0; j < n - 1 - i; j++) {
            if (processes[j].burst_time > processes[j + 1].burst_time) {
                Process temp = processes[j];
                processes[j] = processes[j + 1];
                processes[j + 1] = temp;
            }
        }
    }
}

void sort_by_priority(Process processes[], int n) {
    for (int i = 0; i < n - 1; i++) {
        for (int j = 0; j < n - 1 - i; j++) {
            if (processes[j].priority < processes[j + 1].priority) {
                Process temp = processes[j];
                processes[j] = processes[j + 1];
                processes[j + 1] = temp;
            }
        }
    }
}

void apply_aging(Process processes[], int n) {
    for (int i = 0; i < n; i++) {
        if (processes[i].waiting_time > 0) {
            processes[i].priority += processes[i].waiting_time / AGING_FACTOR;
        }
    }
}

void print_processes(Process processes[], int n) {
    printf("PID\tBurst Time\tPriority\tWaiting Time\tTurnaround Time\n");
    for (int i = 0; i < n; i++) {
        printf("%d\t%d\t\t%d\t\t%d\t\t%d\n", processes[i].pid, processes[i].burst_time, processes[i].priority, processes[i].waiting_time, processes[i].turnaround_time);
    }
}

void simulate_sjf(Process processes[], int n) {
    printf("\n=== Shortest Job First (SJF) ===\n");
    sort_by_burst_time(processes, n);
    calculate_waiting_time(processes, n);
    calculate_turnaround_time(processes, n);
    print_processes(processes, n);
}

void simulate_priority_with_aging(Process processes[], int n) {
    printf("\n=== Priority Scheduling with Aging ===\n");
    apply_aging(processes, n);
    sort_by_priority(processes, n);
    calculate_waiting_time(processes, n);
    calculate_turnaround_time(processes, n);
    print_processes(processes, n);
}

int main() {
    int n;
    printf("Enter number of processes: ");
    scanf("%d", &n);

Process processes[n];
    for (int i = 0; i < n; i++) {
        processes[i].pid = i + 1;
        printf("Enter burst time for process %d: ", processes[i].pid);
        scanf("%d", &processes[i].burst_time);
        printf("Enter priority for process %d: ", processes[i].pid);
        scanf("%d", &processes[i].priority);
    }

simulate_sjf(processes, n);
simulate_priority_with_aging(processes, n);

  
return 0;
}
