# See Makefile-commented for explanation
CFLAGS = -Wall -g
CC     = gcc $(CFLAGS)

collatz_main : collatz_funcs.o collatz_main.o
	$(CC) -o collatz_main collatz_funcs.o collatz_main.o
	@echo collatz_main is ready

collatz_main.o : collatz_main.c collatz.h
	$(CC) -c collatz_main.c

collatz_funcs.o : collatz_funcs.c collatz.h
	$(CC) -c $<


clean:
	rm -f collatz_main test_collatz *.o

clean-tests:
	@echo Removing temporary test files
	rm -f *.o test_collatz test-results

############################################################
# Testing Targets
test : test-cmd test-commando 

test-setup : 			# sets permissions and creates some files for tests
	@chmod u+x testy test_standardize_pids test-data/table.sh test-data/stuff/table.sh
	@touch test-data/stuff/empty

# program that tests functions in cmd.c and cmdcol.c
test_cmd : test_cmd.c cmd.c cmdcol.c commando.h 
	gcc -Wall -Werror -g -o $@ $^

test-cmd : test_cmd test-setup
	./testy test_cmd.org $(testnum)

test-commando : commando test-setup
	./testy test_commando.org $(testnum)

# clean up th testing files
clean-tests :
	rm -rf test_cmd test-results/


############################################################
# 'make zip' to create p1-code.zip for submission
AN=p1
SHELL  = /bin/bash
CWD    = $(shell pwd | sed 's/.*\///g')
zip : clean clean-tests
	rm -f $(AN)-code.zip
	cd .. && zip "$(CWD)/$(AN)-code.zip" -r "$(CWD)"
	@echo Zip created in $(AN)-code.zip
	@if (( $$(stat -c '%s' $(AN)-code.zip) > 10*(2**20) )); then echo "WARNING: $(AN)-code.zip seems REALLY big, check there are no abnormally large test files"; du -h $(AN)-code.zip; fi
	@if (( $$(unzip -t $(AN)-code.zip | wc -l) > 256 )); then echo "WARNING: $(AN)-code.zip has 256 or more files in it which may cause submission problems"; fi

