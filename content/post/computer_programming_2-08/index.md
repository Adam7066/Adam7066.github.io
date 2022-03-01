---
slug: "computer_programming_2-08"
title: "程式設計(二)-08：Linux List"
date: 2021-07-13T22:25:50+08:00
tags:
    - C
    - Note
categories:
    - 程式設計(二)
---
- linuxlist.h
```c
#pragma once

struct list_head {
    struct list_head *next, *prev;
};

#define LIST_HEAD_INIT(name) { &(name), &(name) }

#define LIST_HEAD(name) struct list_head name = LIST_HEAD_INIT(name)

/*
 * Insert a new entry between two known consecutive entries.
 *
 * This is only for internal list manipulation where we know
 * the prev/next entries already!
 */
static inline void __list_add(struct list_head *new,
                              struct list_head *prev,
                              struct list_head *next) {
    next->prev = new;
    new->next = next;
    new->prev = prev;
    prev->next = new;
}

/**
 * list_add - add a new entry
 * @new: new entry to be added
 * @head: list head to add it after
 *
 * Insert a new entry after the specified head.
 * This is good for implementing stacks.
 */
static inline void list_add(struct list_head *new, struct list_head *head) {
    __list_add(new, head, head->next);
}


/**
 * list_add_tail - add a new entry
 * @new: new entry to be added
 * @head: list head to add it before
 *
 * Insert a new entry before the specified head.
 * This is useful for implementing queues.
 */
static inline void list_add_tail(struct list_head *new, struct list_head *head) {
    __list_add(new, head->prev, head);
}

/*
 * Delete a list entry by making the prev/next entries
 * point to each other.
 *
 * This is only for internal list manipulation where we know
 * the prev/next entries already!
 */
static inline void __list_del(struct list_head * prev, struct list_head * next) {
    next->prev = prev;
    prev->next = next;
}

/**
 * list_del - deletes entry from list.
 * @entry: the element to delete from the list.
 * Note: list_empty() on entry does not return true after this, the entry is
 * in an undefined state.
 */
static inline void __list_del_entry(struct list_head *entry) {
    if (entry == NULL)
        return;

    __list_del(entry->prev, entry->next);
}

static inline void list_del(struct list_head *entry) {
    __list_del_entry(entry);
    entry->next = NULL;
    entry->prev = NULL;
}

/**
 * list_empty - tests whether a list is empty
 * @head: the list to test.
 */
static inline int list_empty(const struct list_head *head) {
    return head -> next == head;
}

#define offsetof(TYPE, MEMBER) ((size_t)&((TYPE *)0)->MEMBER)

#define container_of(ptr, type, member) ({				\
	void *__mptr = (void *)(ptr);					\
	((type *)(__mptr - offsetof(type, member))); })

/**
 * list_entry - get the struct for this entry
 * @ptr:	the &struct list_head pointer.
 * @type:	the type of the struct this is embedded in.
 * @member:	the name of the list_head within the struct.
 */
#define list_entry(ptr, type, member) \
    container_of(ptr, type, member)

/**
 * list_first_entry - get the first element from a list
 * @ptr:	the list head to take the element from.
 * @type:	the type of the struct this is embedded in.
 * @member:	the name of the list_head within the struct.
 *
 * Note, that list is expected to be not empty.
 */
#define list_first_entry(ptr, type, member) \
    list_entry((ptr)->next, type, member)

/**
 * list_last_entry - get the last element from a list
 * @ptr:	the list head to take the element from.
 * @type:	the type of the struct this is embedded in.
 * @member:	the name of the list_head within the struct.
 *
 * Note, that list is expected to be not empty.
 */
#define list_last_entry(ptr, type, member) \
    list_entry((ptr)->prev, type, member)

/**
 * list_for_each	-	iterate over a list
 * @pos:	the &struct list_head to use as a loop cursor.
 * @head:	the head for your list.
 */
#define list_for_each(pos, head) \
    for (pos = (head)->next; pos != (head); pos = pos->next)

/**
 * list_for_each_prev	-	iterate over a list backwards
 * @pos:	the &struct list_head to use as a loop cursor.
 * @head:	the head for your list.
 */
#define list_for_each_prev(pos, head) \
    for (pos = (head)->prev; pos != (head); pos = pos->prev)
```

- main.c
```c
#include <stdio.h>
#include <stdlib.h>
#include <stdint.h>
#include <time.h>
#include "linuxlist.h"
typedef struct _sCharacter {
    int32_t id;
    char    name[32];
    int32_t hp;
    int32_t mp;
    int32_t exp;
    int32_t atk;
    int32_t def;
    int32_t ats;
    int32_t adf;
    int32_t spd;

    struct list_head list;
} sCharacter;
sCharacter *allocCharacter(int32_t id) {
    sCharacter *newComer = calloc(1, sizeof(sCharacter));
    newComer->id = id;
    newComer->name[0] = rand() % 26 + 'A';
    for(int32_t j = 1 ; j < 6 ; j++)
        newComer->name[j] = rand() % 26 + 'a';
    newComer->hp = rand() % 100 + 1;
    newComer->mp = rand() % 100 + 1;
    newComer->exp = rand() % 100 + 1;
    newComer->atk = rand() % 100 + 1;
    newComer->def = rand() % 100 + 1;
    newComer->ats = rand() % 100 + 1;
    newComer->adf = rand() % 100 + 1;
    newComer->spd = rand() % 100 + 1;

    return newComer;
}
void printCharacter(sCharacter *one) {
    printf("%04d) ", one->id);
    printf("%8s ", one->name);
    for(int32_t *ptr = &(one->hp); ptr <= &(one->spd); ptr++)
        printf("%3d ", *ptr);
    printf("\n");
    return;
}
int main() {
    LIST_HEAD(char_list_head);
    srand(time(0));
    for(int32_t i = 0 ; i < 1000 ; i++) {
        sCharacter *newComer = allocCharacter(i + 1);
        list_add(&(newComer->list), &char_list_head);
    }
    struct list_head *listptr = NULL;
    list_for_each(listptr, &char_list_head) {
        sCharacter *cptr = list_entry(listptr, sCharacter, list);
        printCharacter(cptr);
    }
    /*
    list_for_each_prev(listptr, &char_list_head) {
        sCharacter *cptr = list_entry(listptr, sCharacter, list);
        printCharacter(cptr);
    }
    */
    return 0;
}
```