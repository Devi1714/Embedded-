#include <stdio.h>
#include <stdlib.h>


typedef struct {
    char name[30];
    int roll;
    int* marks; 
    int numSubjects;
    int flags; 
} Student;

void calculateAverage(Student* students, int numStudents);
void findTopper(Student* students, int numStudents);
void displaySubjectWisePerformance(Student* students, int numStudents);
void setFlags(Student* student);


void calculateAverage(Student* students, int numStudents) {
    for (int i = 0; i < numStudents; i++) {
        int sum = 0;
        for (int j = 0; j < students[i].numSubjects; j++) {
            sum += students[i].marks[j];
        }
        printf("Average marks of %s: %.2f\n", students[i].name, (float)sum / students[i].numSubjects);
    }
}

void findTopper(Student* students, int numStudents) {
    int maxMarks = 0;
    char topperName[30];
    for (int i = 0; i < numStudents; i++) {
        int sum = 0;
        for (int j = 0; j < students[i].numSubjects; j++) {
            sum += students[i].marks[j];
        }
        if (sum > maxMarks) {
            maxMarks = sum;
            strcpy(topperName, students[i].name);
        }
    }
    printf("Topper: %s with total marks: %d\n", topperName, maxMarks);
}
void displaySubjectWisePerformance(Student* students, int numStudents) {
    for (int j = 0; j < students[0].numSubjects; j++) {
        printf("Subject %d performance:\n", j + 1);
        for (int i = 0; i < numStudents; i++) {
            printf("%s: %d\n", students[i].name, students[i].marks[j]);
        }
    }
}


void setFlags(Student* student) {
    int sum = 0;
    for (int i = 0; i < student->numSubjects; i++) {
        sum += student->marks[i];
    }
    float average = (float)sum / student->numSubjects;
    if (average >= 70) {
        student->flags |= 1 << 0; 
    }
    if (average >= 40) {
        student->flags |= 1 << 1; 
    } else {
        student->flags |= 1 << 2; 
    }
}

int main() {
    int numStudents, numSubjects;
    printf("Enter number of students: ");
    scanf("%d", &numStudents);
    printf("Enter number of subjects: ");
    scanf("%d", &numSubjects);
    Student* students = (Student*)malloc(numStudents * sizeof(Student));

    for (int i = 0; i < numStudents; i++) {
        printf("Enter name of student %d: ", i + 1);
        scanf("%s", students[i].name);
        printf("Enter roll number of student %d: ", i + 1);
        scanf("%d", &students[i].roll);
        students[i].numSubjects = numSubjects;
        students[i].marks = (int*)malloc(numSubjects * sizeof(int));
        for (int j = 0; j < numSubjects; j++) {
            printf("Enter marks of subject %d for %s: ", j + 1, students[i].name);
            scanf("%d", &students[i].marks[j]);
        }
        setFlags(&students[i]);
    }

    calculateAverage(students, numStudents);
    findTopper(students, numStudents);
    displaySubjectWisePerformance(students, numStudents);


    for (int i = 0; i < numStudents; i++) {
        free(students[i].marks);
    }
    free(students);

    return 0;
}
