---
slug: "computer_programming_1-06"
title: "程式設計(一)-06：Makefile"
date: 2020-12-02T21:45:51+08:00
tags:
    - Makefile
    - Note
categories:
    - 程式設計(一)
---
# Makefile for 程設一
```makefile
CC = gcc
CFLAGS = -Wall -Wextra -O2 -std=c11
LDFLAGS = -lm
TARGETS = main01 main02
main01_OBJ = main01.o func01.o
main02_OBJ = main02.o func02.o

.PHONY = all clean

all: $(TARGETS)

.SECONDEXPANSION:
$(TARGETS): $$($$@_OBJ)
	$(CC) $^ -o $@ $(LDFLAGS)

%.o: $@.c

clean:
	-$(RM) $(TARGETS) $(foreach targ,$(TARGETS),$(foreach obj, $($(targ)_OBJ), $(obj)))
```