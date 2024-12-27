.PHONY: all clean clean-tests

SOURCES=$(wildcard impl*.nasm)
OBJS=$(SOURCES:%.nasm=build/%.o)
BINARIES=$(OBJS:%.o=%)
PASS=$(BINARIES:build/%=tests/%.passed)

DONE=$(words $(PASS))

all: clean-before tests/task.passed

build:
	mkdir build

$(OBJS): build/%.o:%.nasm | build
	nasm -f elf64 $< -o $@

$(BINARIES): %:%.o tests/test.c | build
	gcc -std=c99 tests/test.c $@.o -o $@

$(PASS): tests/%.passed:build/%
	@echo "Testing $(<:tests/%=%)..."
	@$<
	@touch $@

tests/task.passed: $(PASS)
	@echo $(DONE)/7 implementations passed the tests
	@test 7 -le $(DONE)
	@echo "That's enough! The task is done"
	@touch $@

clean-before:
	@rm -f $(PASS) tests/task.passed

clean: clean-before
	@rm -rf build/
