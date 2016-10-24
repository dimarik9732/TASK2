
CFLAGS = -Wall -g
CC = gcc
SHELL = /bin/bash

clean: 
	rm -f *.o wordsplit

%.o: %.c %.h
	$(CC) $(CFLAGS) -c $< -o $@

wordsplit: wordsplit.c util.o
	$(CC) $(CFLAGS) $^ -o $@

countd: countd.c util.o
	$(CC) $(CFLAGS) $^ -o $@

all: wordsplit.c countd.c util.o
	$(CC) $(CFLAGS) wordsplit.c util.o -o wordsplit
	$(CC) $(CFLAGS) countd.c util.o -o countd

linesort: ../task1/linesort.c
	$(CC) $(CFLAGS) $^ -o $@

data: books Makefile
	cd texts; wget -o log -nc `cat ../$<`; cd ..
	cat texts/*txt > data

ifneq (clean, $(MAKECMDGOALS))
-include deps.mk
endif

deps.mk: util.c hash.c
	$(CC) -MM $^ > $@

dict: data wordsplit countd linesort Makefile
	time cat $< | ./wordsplit -l | ./linesort -m | ./countd | sort -nr > $@

dict_std: data Makefile
	time cat $< | tr "[:upper:]" "[:lower:]" | tr -cs "[:alpha:]" "[\n*]" | sort  | uniq -ci | sort -nr > $@

freq_dict: freq_dict.c hash.o util.o 
	$(CC) $(CFLAGS) freq_dict.c hash.o util.o -o $@

freq_dict.txt: data freq_dict Makefile
	time cat $< | ./freq_dict -l | sort -nr > $@
clean: rm -f *.o *.txt
