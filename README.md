# Project-7
Student report card management 
#include <stdio.h>
#include <string.h>

struct Student {
    int roll;
    char name[50];
    float marks[5];
    float total;
    float percentage;
    char grade;
};

void calculate(struct Student *s) {
    s->total = 0;
    for (int i = 0; i < 5; i++)
        s->total += s->marks[i];

    s->percentage = s->total / 5;

    if (s->percentage >= 90) s->grade = 'A';
    else if (s->percentage >= 75) s->grade = 'B';
    else if (s->percentage >= 60) s->grade = 'C';
    else if (s->percentage >= 45) s->grade = 'D';
    else s->grade = 'F';
}

void addStudent() {
    FILE *fp = fopen("students.txt", "ab");
    struct Student s;

    printf("Enter Roll Number: ");
    scanf("%d", &s.roll);

    printf("Enter Name: ");
    scanf("%s", s.name);

    printf("Enter marks of 5 subjects:\n");
    for (int i = 0; i < 5; i++) {
        printf("Subject %d: ", i + 1);
        scanf("%f", &s.marks[i]);
    }

    calculate(&s);

    fwrite(&s, sizeof(s), 1, fp);
    fclose(fp);

    printf("\nStudent added successfully!\n");
}

void displayAll() {
    FILE *fp = fopen("students.txt", "rb");
    struct Student s;

    if (!fp) {
        printf("No records found!\n");
        return;
    }

    printf("\n--- All Students ---\n");

    while (fread(&s, sizeof(s), 1, fp)) {
        printf("\nRoll: %d\nName: %s\nTotal: %.2f\nPercentage: %.2f\nGrade: %c\n",
               s.roll, s.name, s.total, s.percentage, s.grade);
    }

    fclose(fp);
}

void searchStudent() {
    FILE *fp = fopen("students.txt", "rb");
    struct Student s;
    int roll, found = 0;

    printf("Enter roll number to search: ");
    scanf("%d", &roll);

    while (fread(&s, sizeof(s), 1, fp)) {
        if (s.roll == roll) {
            printf("\n--- Student Found ---\n");
            printf("Roll: %d\nName: %s\nTotal: %.2f\nPercentage: %.2f\nGrade: %c\n",
                   s.roll, s.name, s.total, s.percentage, s.grade);
            found = 1;
            break;
        }
    }

    if (!found)
        printf("No student found with this roll number.\n");

    fclose(fp);
}

int main() {
    int choice;

    while (1) {
        printf("\n--- Student Report Card System ---\n");
        printf("1. Add Student\n");
        printf("2. Display All Students\n");
        printf("3. Search Student\n");
        printf("4. Exit\n");
        printf("Enter choice: ");
        scanf("%d", &choice);

        switch (choice) {
            case 1: addStudent(); break;
            case 2: displayAll(); break;
            case 3: searchStudent(); break;
            case 4: return 0;
            default: printf("Invalid choice! Try again.\n");
        }
    }
}
