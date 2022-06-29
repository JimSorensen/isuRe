SOURCES = hello.cpp 
DEPS=$(SOURCES:.cpp=.o)
OBJ = $(addprefix $(BUILD_DIR)/, $(SOURCES:.cpp=.o))
DEPS=$(addprefix $(BUILD_DIR)/, $(SOURCES:.cpp=.d))
EXE = program
CXXFLAGS=-I.

ifeq (${ARCH}, host)
CXX = g++ -pthread
BUILD_DIR =./Build/host
BIN_DIR =./bin/host
endif

ifeq (${ARCH},target)
CXX = arm-rpizw-g++
BUILD_DIR = ./build/target
BIN_DIR = ./bin/target
endif

$(BIN_DIR)/$(EXEC): $(DEPS) $(OBJ)
	mkdir -p $(BIN_DIR)
	$(CXX) $(CXXFLAGS) -o $@ $(OBJ)

$(BUILD_DIR)/%.d: %.cpp
	mkdir -p $(BUILD_DIR)
	$(CXX) -MT$(@:.d=.o) -MM $(CXXFLAGS) $^ > $@

$(BUILD_DIR)/%.o: %.cpp
	mkdir -p $(BUILD_DIR)
	$(CXX) -c $< -o $@ $(CXXFLAGS)

format: $(SOURCES:.cpp=.format)

%.format: %.cpp
	@echo "Formatting file '$<'"...
	@clang-format -i $<
	$echo "" > $@

clean:
	-rm -rf bin/
	-rm -rf build/

help:
	@echo "$(SOURCES)"

ifeq ($(MAKECMDGOALS),clean)
-include $(DEPS)
endif