CXXFLAGS=-std=c++0x -Wall -g -F /Library/Frameworks 
TESTFLAGS=-I/usr/local/include -L/usr/local/lib -lgtest -lgtest_main -L. -lemu -lpthread
LIBRARY=libemu.a

TEST_SCRIPT=tests.sh

SOURCES=$(filter-out %_test.cpp, $(wildcard *.cpp)) x86_base.cpp
OBJECTS=$(addsuffix .o, $(basename $(SOURCES)))

TEST_SOURCES=$(wildcard *_test.cpp)
TEST_BINARIES=$(basename $(TEST_SOURCES))

GENERATED_FILES=x86_base.cpp x86_base.h

all: $(SOURCES) $(LIBRARY) tests 
    
clean:
	rm -f *.o $(TEST_BINARIES) $(GENERATED_FILES) $(TEST_SCRIPT) $(LIBRARY)
	rm -rf *.dSYM

# Generated code.
x86_base.cpp x86_base.h: generate.py generator/x86_base.cpp.template generator/x86_base.h.template
	./generate.py

# Static library.
$(LIBRARY): $(OBJECTS) 
	ar ru $(LIBRARY) $(OBJECTS)

# Tests.
tests: $(LIBRARY) $(TEST_BINARIES)
	@echo "#!/bin/bash" > $(TEST_SCRIPT)
	@$(foreach BIN, $(TEST_BINARIES), echo ./$(BIN) >> $(TEST_SCRIPT);)
	@chmod a+x $(TEST_SCRIPT)

$(TEST_BINARIES): %: %.cpp $(LIBRARY)
	g++ $@.cpp -o $@ $(CXXFLAGS) $(TESTFLAGS) 

