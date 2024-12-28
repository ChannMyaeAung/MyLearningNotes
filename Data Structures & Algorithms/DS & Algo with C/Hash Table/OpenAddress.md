### C Code Example (Open Addressing) (Simpler)

#### Program Code

`practice.h`

```C
#define MAX_NAME 256
#define TABLE_SIZE 10

// any pointer that see this value will be treated as deleted
//  so any value I am looking for can't be in that node
#define DELETED_NODE (person *)(0xFFFFFFFFFFFFFFFFUL)
// same as defining DELETED_NODE as (person *)-1 for portability

typedef struct
{
    char name[MAX_NAME];
    int age;
} person;

unsigned int hash(char *name);
void init_hash_table();
void print_table();
bool hash_table_insert(person *p);

person *hash_table_lookup(char *name);
person *hash_table_delete(char *name);
```

`functions.c`

```C
person *hash_table[TABLE_SIZE];

unsigned int hash(char *name){
    int length = strnlen(name, MAX_NAME);
    unsigned int hash_value = 0;
    for(int i = 0; i < length; i++){
        hash_value += name[i];
        hash_value = (hash_value * name[i]) % TABLE_SIZE;
    }
    return hash_value;
}

// Initialize hash table by setting all slots to NULL
void init_hash_table(){
    for(int i = 0; i < TABLE_SIZE; i++){
        hash_table[i] = NULL;
    }
}

void print_table(){
    printf("Start\n");
    
    for(int i = 0; i < TABLE_SIZE; i++){
        if(hash_table[i] == NULL){
            printf("\t%i\t---\n", i);
        }else if(hash_table[i] == DELETED_NODE){
            printf("\t%i\t---<deleted>\n", i);
        }else{
            printf("\t%i\t%s\n", i, hash_table[i]->name);
        }
    }
    
    printf("End\n");
}

bool hash_table_insert(person *p){
    if(p == NULL){
        return false;
    }
    
    // get a random hash value between 0 and TABLE_SIZE and set it to index
    int index = hash(p->name);
    
    // Linear probing
    for(int i = 0; i < TABLE_SIZE; i++){
        int try = (i + index) % TABLE_SIZE;
        
        // Look for empty space to fill in the value to address collisions
        if(hash_table[try] == NULL || hash_table[try] == DELETED_NODE){
            hash_table[try] = p;
            return true;
        }
    }
    return false;
}

person *hash_table_lookup(char *name){
    int index = hash(name);
    
    for(int i = 0; i < TABLE_SIZE; i++){
        int try = (i + index) % TABLE_SIZE;
        if(hash_table[try] == NULL){
            return NULL;
        }
        
        // continue the lookup if we find a deleted node
        if(hash_table[try] == DELETED_NODE){
            continue;
        }
        
        if(strncmp(hash_table[try]->name, name, MAX_NAME) == 0){
            // return the matched name
            return hash_table[try];
        }
    }
    return NULL;
}

person *hash_table_delete(char *name){
    int index = hash(name);
    
    for(int i = 0; i < TABLE_SIZE; i++){
        int try = (i + index) % TABLE_SIZE;
        if(hash_table[try] == NULL){
            return NULL;
        }
        if(hash_table[try] == DELETED_NODE){
            continue;
        }
        if(strncmp(hash_table[try]->name, name, MAX_NAME) == 0){
            person *tmp = hash->table[try];
            hash->table[try] = DELETED_NODE;
            return tmp;
        }
    }
    return NULL;
}
```

`practice.c`

```C
int main()
{
    init_hash_table();
    print_table();
    person jacob = {.name = "Jacob", .age = 25};
    person kate = {.name = "Kate", .age = 28};
    person sarah = {.name = "Sarah", .age = 32};
    person penc = {.name = "PENC", .age = 24};
    person john = {.name = "John", .age = 27};
    person moon = {.name = "Moon", .age = 26};
    person johnson = {.name = "Johnson", .age = 19};
    person joseph = {.name = "Joseph", .age = 21};
    person steve = {.name = "Steve", .age = 22};
    person ember = {.name = "Ember", .age = 56};

    hash_table_insert(&jacob);
    hash_table_insert(&kate);
    hash_table_insert(&sarah);
    hash_table_insert(&penc);
    hash_table_insert(&john);
    hash_table_insert(&moon);
    hash_table_insert(&johnson);
    hash_table_insert(&joseph);
    hash_table_insert(&steve);
    hash_table_insert(&ember);
    print_table();

    person *tmp = hash_table_lookup("PENC");
    if (tmp == NULL)
    {
        printf("Not found\n");
    }
    else
    {
        printf("Found %s\n", tmp->name);
    }

    tmp = hash_table_lookup("Jacob");
    if (tmp == NULL)
    {
        printf("Not found\n");
    }
    else
    {
        printf("Found %s\n", tmp->name);
    }

    hash_table_delete("Kate");
    tmp = hash_table_lookup("Kate");
    if (tmp == NULL)
    {
        printf("Not found\n");
    }
    else
    {
        printf("Found %s\n", tmp->name);
    }

    print_table();
    return 0;
}
```



#### Program Output

```shell
chan@CMA:~/C_Programming/practice$ ./practice
Start
	0	---
	1	---
	2	---
	3	---
	4	---
	5	---
	6	---
	7	---
	8	---
	9	---
End
Start
	0	John
	1	Kate
	2	Jacob
	3	PENC
	4	Sarah
	5	Moon
	6	Johnson
	7	Joseph
	8	Steve
	9	Ember
End
Found PENC
Found Jacob
Not found
Start
	0	John
	1	---<deleted>
	2	Jacob
	3	PENC
	4	Sarah
	5	Moon
	6	Johnson
	7	Joseph
	8	Steve
	9	Ember
End

```

