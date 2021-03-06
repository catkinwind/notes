# Variables make Makefiles simpler

This is an example of using variables.

objects = main.o kbd.o command.o display.o \
          insert.o search.o files.o utils.o

edit : $(objects)
        cc -o edit $(objects)
main.o : main.c defs.h
        cc -c main.c
kbd.o : kbd.c defs.h command.h
        cc -c kbd.c
command.o : command.c defs.h command.h
        cc -c command.c
display.o : display.c defs.h buffer.h
        cc -c display.c
insert.o : insert.c defs.h buffer.h
        cc -c insert.c
search.o : search.c defs.h buffer.h
        cc -c search.c
files.o : files.c defs.h buffer.h command.h
        cc -c files.c
utils.o : utils.c defs.h
        cc -c utils.c
clean :
        rm edit $(objects)

Actually the cc -c foo.c could be ommitted, make will deduce the recipes.
objects = main.o kbd.o command.o display.o \
          insert.o search.o files.o utils.o

edit : $(objects)
        cc -o edit $(objects)

main.o : defs.h
kbd.o : defs.h command.h
command.o : defs.h command.h
display.o : defs.h buffer.h
insert.o : defs.h buffer.h
search.o : defs.h buffer.h
files.o : defs.h buffer.h command.h
utils.o : defs.h

.PHONY : clean
clean :
        rm edit $(objects)


".PHONY" aims to avoid errors as clean could be a file.
A phony target is on that is not really the name of a file; rather it is just a name for a recipe to be executed when you make an explicit request. If no explicit request provided, make will execute phony target ``all''.

all : prog1 prog2 prog3
.PHONY : all

prog1 : prog1.o utils.o
        cc -o prog1 prog1.o utils.o

prog2 : prog2.o
        cc -o prog2 prog2.o

prog3 : prog3.o sort.o utils.o
        cc -o prog3 prog3.o sort.o utils.o


* Another style of Makefile

objects = main.o kbd.o command.o display.o \
          insert.o search.o files.o utils.o

edit : $(objects)
        cc -o edit $(objects)

$(objects) : defs.h
kbd.o command.o files.o : command.h
display.o insert.o search.o files.o : buffer.h

"Whether this is better is a matter of taste; it is more compact, but some people dislike it because they find it clear to put all the information about each target in one place.


# Directory Search

The value of the make variable VPATH specifies a list directories that make should search. the directories are expected to contain prerequisite files that are not in the current directory; however, make uses VPATH as a search list for both prerequisites and targets of rules.

VPATH = src:../headers
specifies a path containing two directories, src and ../headers, which make searches in that *order*.

# $@ and $<

$@ is the dependencies, $< is the target.
