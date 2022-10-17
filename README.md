# C-Programming
# main.c 
#include "AssignmentHeader.h"
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

int main(){
    int choices;
    while(1){
        printf("Hai there!\n");
        printf("***************************\n");
        printf("Personal Task Management\n \t1. Add New Task\n \t2. View Tasks\n \t3. Update Task\n \t4. Delete Task\n \t5. Exit\n\nEnter option : ");
        scanf("%d", &choices);
        switch (choices){
        case 1:
            Add_Task();
            break;
        case 2:
            View_Tasks();
            break;
        case 3:
            Update_Task();
            break;
        case 4:
            Delete_Task();
            break;
        case 5:
            printf("Thank you! Good luck on your daily tasks! \nHope to see you soon!\n");
            exit(0);
        default:
            printf("Invalid choice! Please enter your choice again!\n");
            break;
}
}
}


#Header.c
#include "AssignmentHeader.h"
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

struct view {
    int counter;
    int priority;
};

// This is to check the number of lines in the text file //
int check(){
    int counter = 0;
    char chr;
    FILE * Task_Handler = fopen("Task_Database.txt", "r");
    while ((chr=fgetc(Task_Handler))!= EOF){
        if(chr == '\n')
            counter++;
    }
    fclose(Task_Handler);
    return counter;
}

void Add_Task(){
    char Task_Name[50], Task_Category[50], Task_Deadline[50], Task_Priority[50];
    printf("Please enter the task name.\nTask Name: ");
    scanf("%s", &Task_Name);
    printf("Please enter the task category.\nTask Category: ");
    scanf("%s", &Task_Category);
    printf("Please enter the task deadline in the format MM/DD/YY.\nTask Deadline: ");
    scanf("%s", &Task_Deadline);
    printf("Please enter the task priority from 1 to 4.\n1 for Very Urgent and 4 for Not Urgent\nTask Priority: ");
    scanf("%s", &Task_Priority);
    FILE * Task_Handler = fopen("Task_Database.txt", "a");
    fprintf(Task_Handler, "%s\n%s\n%s\n%s\n", Task_Name, Task_Category, Task_Deadline, Task_Priority);
    fclose(Task_Handler);
}


void View_Tasks(){
    int choices, num,k, j;
    char sen[100];
    char Task_List[100][5][100];
    num = check();
    printf("How would you want to view your tasks?\n");
    printf("\t1. View All Task\n \t2. Task Priority\n\n");
    printf("Enter choice : ");
    scanf("%d", &choices);
    FILE * Task_Handler = fopen("Task_Database.txt", "r");
    for (k=0; k<(num/4); k++){
        for(j=0; j<4; j++){
                while(fgets(sen, sizeof(sen), Task_Handler)){
                        strcpy(Task_List[k][j], sen);
                        break;
                }
        }
    }

    switch (choices){
    case 1 :
        View_All_Tasks(num, Task_List);
        break;
    case 2 :
        printf("Would you want to view in :\n \t1. Ascending Order\n \t2. Descending Order\n\nEnter choice : ");
        scanf("%d", &choices);
        if (choices == 1){
            Ascending_Order(num, Task_List);
            break;
        }
        else if (choices == 2){
            Descending_Order(num, Task_List);
            break;
        }
    }
}

// use 3-D array to view the tasks would be easier //
void View_All_Tasks(int num, char Task_List[100][5][100]){
    int k, j;
    printf("The following would be all tasks.\n\n");
    printf("The format would be as below : \n\n1)\tTask Name\n2)\tTask Category\n3)\tTask Deadline\n4)\tTask Priority\n\n");
    for (k=0; k<(num/4); k++){
        printf("\n");
        printf("[%d]", k+1);
        for(j=0; j<4; j++){
            printf("\t%s", Task_List[k][j]);
        }
    }
    printf("\n");
    return 0;
}

void Ascending_Order(int num, char Task_List[100][5][100]){
    struct view v;
    v.counter = 0;
    v.priority = 1;
    int j;
    int k;
    int val;
    printf("The following would be the tasks.\n\n");
    printf("The format would be as below : \n\n1)\tTask Name\n2)\tTask Category\n3)\tTask Deadline\n4)\tTask Priority\n\n");
    while (v.counter < (num/4)){
        for (k=0; k<(num/4); k++){
            sscanf(Task_List[k][3], "%d", &val);
            if (val == v.priority){
                printf("[%d]", v.counter + 1);
                for (j=0; j<4; j++){
                    printf("\t%s", Task_List[k][j]);
                }
                printf("\n");
                v.counter++;
            }
        }
        v.priority++;
    }
}

void Descending_Order(int num, char Task_List[100][5][100]){
    struct view v;
    v.priority = 4;
    v.counter = 0;
    int j;
    int k;
    int val;
    printf("The following would be the tasks.\n\n");
    printf("The format would be as below : \n\n1)\tTask Name\n2)\tTask Category\n3)\tTask Deadline\n4)\tTask Priority\n\n");
    while (v.counter < (num/4)){
        for (k=0; k<(num/4); k++){
            sscanf(Task_List[k][3], "%d", &val);
            if (val == v.priority){
                printf("[%d]", v.counter + 1);
                for (j=0; j<4; j++){
                    printf("\t%s", Task_List[k][j]);
                }
                printf("\n");
                v.counter++;
            }
        }
        v.priority--;
    }
}

void Update_Task(){
    int choices, num,k, j, index,index2, val;
    char sen[100];
    char Task_List[100][5][100];
    char temp[100];
    char (*ptr) [5][100]= Task_List;
    char str1[100];
    char TaskName[100];
    char TaskCategory[100];
    char TaskDeadline[100];
    char TaskPriority[100];
    printf("Please enter task name!\nEnter Task Name : ");
    scanf("%s", &temp);
    char *ptr2 = &temp;
    num = check();
    FILE * Task_Handler = fopen("Task_Database.txt", "r");
    for (k=0; k<(num/4); k++){
        for(j=0; j<4; j++){
                while(fgets(sen, sizeof(sen), Task_Handler)){
                        strcpy(Task_List[k][j], sen);
                        break;
                }
        }
    }
    while (1){
        for (k=0; k<(num/4); k++){
            for (j=0; j<4; j++){
                sscanf(ptr[k][j], "%s", &str1);
                val = strcmp(ptr2, str1);
                if (val == 0){
                    index = k;
                    printf("\nWhat would you want to edit?\n \t1. Task Name\n \t2. Task Category\n \t3. Task Deadline\n \t4. Task Priority\n\nEnter choice : ");
                    scanf("%d", &choices);
                    switch (choices){
                    case 1 :
                        printf("Enter New Task Name : ");
                        index2 = choices - 1;
                        scanf("%s", &ptr[k][j]);
                        printf("Data updated! The new task name is %s.\n", ptr[k][j]);
                        break;
                    case 2 :
                        printf("Enter New Task Category : ");
                        index2 = choices - 1;
                        scanf("%s", &ptr[k][j + 1]);
                        printf("Data updated! The new task category is %s.\n", ptr[k][j + 1]);
                        break;
                    case 3 :
                        printf("Enter New Task Deadline in the format MM/DD/YY : ");
                        index2 = choices - 1;
                        scanf("%s", &ptr[k][j + 2]);
                        printf("Data updated! The new task deadline is %s.\n", ptr[k][j + 2]);
                        break;
                    case 4 :
                        printf("Enter New Task Priority: ");
                        index2 = choices - 1;
                        scanf("%s", &ptr[k][j + 3]);
                        printf("Data updated! The new task priority is %s.\n", ptr[k][j + 3]);
                        break;
                    default:
                        printf("Invalid choice! Please insert your choice again!\n");
                        break;
                    }
                }
                else if (val != 0){
                    break;
                }
        fclose(Task_Handler);
        fopen("Task_Database.txt", "w+");
        for (k=0; k<(num/4); k++){
            for (j=0; j<4; j++){
                if (k == index && j == index2){
                    fprintf(Task_Handler, "%s\n", ptr[k][j]);
                }
                else {
                    fprintf(Task_Handler, "%s", ptr[k][j]);
                }
            }
        }
        fclose(Task_Handler);
        return(0);
            }
        }
    }
}

// sscanf is used because it cannot compare values in array with string //
void Delete_Task(){
    int num;
    int k;
    int j;
    int val;
    int confirmation;
    int index;
    int index2;
    char sen[100];
    char temp[100];
    char str1[100];
    char Task_List[100][5][100];
    char (*ptr) [5][100]= Task_List;
    num = check();
    printf("Please enter task name that you would like to delete!\nEnter Task Name : ");
    scanf("%s", &temp);
    char *ptr2 = &temp;
    FILE * Task_Handler = fopen("Task_Database.txt", "r");
    for (k=0; k<(num/4); k++){
        for(j=0; j<4; j++){
                while(fgets(sen, sizeof(sen), Task_Handler)){
                        strcpy(Task_List[k][j], sen);
                        break;
                }
        }
    }
    fclose(Task_Handler);
    fopen("Task_Database.txt", "w");
    while (1){
        for (k=0; k<(num/4); k++){
            for (j=0; j<4; j++){
                sscanf(ptr[k][j], "%s", &str1);
                val = strcmp(ptr2, str1);
                if (val == 0){
                    j += 4;
                }
                else {
                    fprintf(Task_Handler, "%s", ptr[k][j]);
                }
            }
        }
    printf("Task deleted successfully!\n");
    fclose(Task_Handler);
    break;
    }
}


#AssignmentHeader.h
#ifndef ASSIGNMENTHEADER_H_INCLUDED
#define ASSIGNMENTHEADER_H_INCLUDED
int main();
int check();
void Add_Task();
void View_Tasks();
void View_All_Tasks(int num, char Task_List[100][5][100]);
void Ascending_Order(int num, char Task_List[100][5][100]);
void Descending_Order(int num, char Task_List[100][5][100]);
void Update_Task();
void Delete_Task();
#endif // ASSIGNMENTHEADER_H_INCLUDED
