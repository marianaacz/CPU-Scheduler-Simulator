#include <stdio.h>
#include <stdlib.h>
#include <stdbool.h>
#include <string.h>

int main (int argc, char **argv) {
        int i=0;
        int max = 100;
        int clock = 0;
        int startArray[max];//start time
        int durationArray[max]; //burst time
        int numberProcesses = 0;
        double waitingTime = 0;  //waiting time array
        int turnAroundTime = 0; //turn around array
        double avgWaitingTime = 0;  //av waiting timee
        double responseTime = 0;
        double avgTAT = 0;
        //double avgResponse = 0;
        bool done[max];
        int c;
        FILE *input = argc > 1 ? fopen (argv[1],"r") : stdin;
        if (!input) {
                fprintf (stderr, "error: file open failed '%s'.\n", argv[1]);
                return 1;
        }

        while (i < max && fscanf (input, " %d %d", &startArray[i], &durationArray[i]) == 2){
                numberProcesses++;
                i++;
        }
        //FIRST COME FIRST SERVE
        for(c = 0; c < numberProcesses; c++){
                if (startArray[c] > clock){
                        clock = startArray[c];
                        waitingTime += (startArray[c] - clock);
                        turnAroundTime += durationArray[c] + (startArray[c] - clock);
                }
                else{ //clock > startArray[c]
                        waitingTime += clock-startArray[c];
                        turnAroundTime += durationArray[c] + (clock - startArray[c]);
                }
                clock += durationArray[c];
        printf("First Come First Serve");
        avgWaitingTime = (waitingTime*1.0 / numberProcesses);
        avgTAT = (turnAroundTime*1.0 / numberProcesses);
        printf("\nAvg. Resp.: %.2f", avgWaitingTime);
        printf( " Avg. T.A.: %.2f", avgTAT); 
        printf(" Avg. Wait: %.2f", avgWaitingTime);
        printf("\n");


        //SHORTEST JOB FIRST 
        waitingTime = 0;
        clock = 0;
        turnAroundTime = 0;
        memset(done, false, sizeof(bool)*max);
        int small = -1;
        int finished = 0;
/*      for(int i = 0; i < numberProcesses; i++){
                printf("%d %d %d %d  \n", i , startArray[i], durationArray[i], done[i]);
        }*/
        while(finished < numberProcesses){
                small = -1;
                for(int i= 0; i < numberProcesses; i++){
                        if (!done[i] && startArray[i]<= clock){
                                if(small == -1 || durationArray[i] < durationArray[small]){
                                        small = i;
                                }
                        }
                }
                if (small == -1){
                        ++clock;
                }
                else {
                        waitingTime += clock-startArray[small];
                        turnAroundTime += durationArray[small] + (clock - startArray[small]);
                        finished++;
                        clock += durationArray[small];
                        done[small] = true;
                }
        }
        avgWaitingTime = (waitingTime / numberProcesses);
        avgTAT = (turnAroundTime / numberProcesses);
        printf("\nShortest Job First");
        printf("\nAvg. Resp.: %.2f", avgWaitingTime);
        printf( " Avg. T.A.: %.2f", avgTAT); 
        printf(" Avg. Wait: %.2f", avgWaitingTime);
        printf("\n");

        //SHORTEST REMAINING TIE FIRST 
        int endTime;
        int smallest;
        int remain=0;
        waitingTime = 0;
        turnAroundTime = 0;
        int sumTAT = 0;
        int sumWait = 0;
        clock = 0;
        int rArray[max];
        memcpy(rArray, durationArray, sizeof(int)*max);

        responseTime = 0;
        for(clock =0; remain != numberProcesses; clock++){
                smallest = -1;
                for(i = 0; i < numberProcesses; i++){
                        if(startArray[i] <= clock && (smallest == -1 || rArray[i] < rArray[smallest]) && rArray[i]>0){
                                smallest = i;
                        }
                }
                if(smallest == -1){
                         continue;
                }
                if (rArray[smallest] == durationArray[smallest]){
                        responseTime += (clock - startArray[smallest]);
                }
                rArray[smallest]--;
                if(rArray[smallest] == 0){
                        remain++;
                        endTime = clock+1;
                        sumWait += endTime - durationArray[smallest] - startArray[smallest];
                        sumTAT += endTime - startArray[smallest];
                }
        }

        printf("\nShortest Remaining Job First");
        avgWaitingTime = (sumWait*1.0 / numberProcesses);
        avgTAT = (sumTAT*1.0 / numberProcesses);
        double avgRtime = (responseTime*1.0 / numberProcesses);
        printf("\nAvg. Resp.: %.2f", avgRtime);
        printf( " Avg. T.A.: %.2f", avgTAT); 
        printf(" Avg. Wait: %.2f", avgWaitingTime);
        printf("\n");
        
        return 0;
}



