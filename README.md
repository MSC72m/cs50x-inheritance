# Simulate Genetic Inheritance of Blood Type

## Project Description

This project simulates the inheritance of blood types through multiple generations. The program models a family tree, randomly assigns blood type alleles to individuals, and prints out the family tree showing each individual's blood type.

## Table of Contents
- [Installation](#installation)
- [Usage](#usage)
- [Code Explanation](#code-explanation)

## Installation

No special installation is required for this project. Ensure you have a C compiler like `gcc` installed.

## Usage

To compile and run the project, use the following commands:
```bash
make inheritance
./inheritance
The program will generate a family tree with three generations and print each family member's blood type.
```

## Code Explanation
Constants and Structures
``` C
#include <stdbool.h>
#include <stdio.h>
#include <stdlib.h>
#include <time.h>

// Each person has two parents and two alleles
typedef struct person
{
    struct person *parents[2];
    char alleles[2];
} person;

const int GENERATIONS = 3;
const int INDENT_LENGTH = 4;
```
#include <stdbool.h>, #include <stdio.h>, #include <stdlib.h>, and #include <time.h> include necessary libraries for boolean type, standard input/output, memory allocation, and random number generation respectively. The person struct represents an individual, with pointers to two parents and an array of two alleles. GENERATIONS defines the number of generations to simulate. INDENT_LENGTH defines the indentation level for printing the family tree.

Function Prototypes
``` C
person *create_family(int generations);
void print_family(person *p, int generation);
void free_family(person *p);
char random_allele();
These are the prototypes for the functions used in the program.
```
Main Function
``` C
int main(void)
{
    // Seed random number generator
    srand(time(0));

    // Create a new family with three generations
    person *p = create_family(GENERATIONS);

    // Print family tree of blood types
    print_family(p, 0);

    // Free memory
    free_family(p);
}
main function seeds the random number generator with srand(time(0)). It then creates a new family tree with create_family(GENERATIONS), prints the family tree starting from the root person with print_family(p, 0), and finally frees the memory allocated for the family tree with free_family(p).

Create Family Function
c
Copy code
person *create_family(int generations)
{
    // Allocate memory for new person
    person *new_person = malloc(sizeof(person));

    if (new_person == NULL)
    {
        printf("memory allocation failed");
        return NULL;
    }
    // If there are still generations left to create
    if (generations > 1)
    {
        // Create two new parents for current person by recursively calling create_family
        person *parent0 = create_family(generations - 1);
        person *parent1 = create_family(generations - 1);

        // Set parent pointers for current person
        new_person->parents[0] = parent0;
        new_person->parents[1] = parent1;

        // Randomly assign current person's alleles based on the alleles of their parents
        int random_index0 = rand() % 2;
        char random_allele_from_parent0 = parent0->alleles[random_index0];
        int random_index1 = rand() % 2;
        char random_allele_from_parent1 = parent1->alleles[random_index1];
        new_person->alleles[0] = random_allele_from_parent0;
        new_person->alleles[1] = random_allele_from_parent1;
    }
    // If there are no generations left to create
    else
    {
        // Set parent pointers to NULL
        new_person->parents[0] = NULL;
        new_person->parents[1] = NULL;
        // Randomly assign alleles
        new_person->alleles[0] = random_allele();
        new_person->alleles[1] = random_allele();
    }

    // Return newly created person
    return new_person;
}
```
create_family function allocates memory for a new person and handles memory allocation failure. If more generations need to be created, it recursively creates parents and assigns alleles based on the parents' alleles. If no more generations need to be created, it sets parent pointers to NULL and assigns random alleles. Finally, it returns the newly created person.

Free Family Function
``` C
void free_family(person *p)
{
    // Handle base case
    if (p == NULL)
    {
        return;
    }

    // Free parents recursively
    free_family(p->parents[0]);
    free_family(p->parents[1]);

    // Free child
    free(p);
}
```
free_family function frees the memory allocated for each person and their ancestors recursively.

Print Family Function
``` C
void print_family(person *p, int generation)
{
    // Handle base case
    if (p == NULL)
    {
        return;
    }

    // Print indentation
    for (int i = 0; i < generation * INDENT_LENGTH; i++)
    {
        printf(" ");
    }

    // Print person
    if (generation == 0)
    {
        printf("Child (Generation %i): blood type %c%c\n", generation, p->alleles[0], p->alleles[1]);
    }
    else if (generation == 1)
    {
        printf("Parent (Generation %i): blood type %c%c\n", generation, p->alleles[0], p->alleles[1]);
    }
    else
    {
        for (int i = 0; i < generation - 2; i++)
        {
            printf("Great-");
        }
        printf("Grandparent (Generation %i): blood type %c%c\n", generation, p->alleles[0], p->alleles[1]);
    }

    // Print parents of current generation
    print_family(p->parents[0], generation + 1);
    print_family(p->parents[1], generation + 1);
}
```
print_family function handles the base case when the person is NULL. It prints the appropriate level of indentation for the current generation, prints the blood type of the person with different titles depending on the generation, and recursively prints the parents of the current person.

Random Allele Function
``` C
char random_allele()
{
    int r = rand() % 3;
    if (r == 0)
    {
        return 'A';
    }
    else if (r == 1)
    {
        return 'B';
    }
    else
    {
        return 'O';
    }
}
```
random_allele function generates a random number between 0 and 2 and returns 'A' if the random number is 0, 'B' if 1, and 'O' if 2.
