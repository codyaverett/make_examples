.PHONY: all clean init executable library

CC = gcc
CFLAGS = -Wall -Wextra
LDFLAGS =

# Project type could be changed per project (executable, library)
TYPE = executable

# Source and build directories
SRC_DIR = src
OBJ_DIR = obj
BIN_DIR = bin
LIB_DIR = lib

# Project name
PROJECT_NAME = my_project

# Sources and objects
SRCS = $(wildcard $(SRC_DIR)/*.c)
OBJS = $(SRCS:$(SRC_DIR)/%.c=$(OBJ_DIR)/%.o)

# Final binary name for executable type
EXEC = $(BIN_DIR)/$(PROJECT_NAME)

# Final library name for library type
LIB = $(LIB_DIR)/lib$(PROJECT_NAME).a

# Targets
all: $(TYPE)

init:
	@mkdir -p $(SRC_DIR) $(OBJ_DIR) $(BIN_DIR) $(LIB_DIR)
	@echo "#include <stdio.h>" > $(SRC_DIR)/main.c
	@echo 'int main(void) { printf("Hello, World!\\n"); return 0; }' >> $(SRC_DIR)/main.c

executable: $(EXEC)

library: $(LIB)

$(EXEC): $(OBJS)
	@mkdir -p $(BIN_DIR)
	$(CC) $(LDFLAGS) $^ -o $@

$(LIB): $(OBJS)
	@mkdir -p $(LIB_DIR)
	ar rcs $@ $^

$(OBJS): $(OBJ_DIR)/%.o : $(SRC_DIR)/%.c
	@mkdir -p $(OBJ_DIR)
	$(CC) $(CFLAGS) -c $< -o $@

clean:
	@$(RM) -r $(OBJ_DIR) $(BIN_DIR) $(LIB_DIR)

help:
	@echo "Available commands:"
	@echo "  init         - Initialize project structure with basic templates"
	@echo "  executable   - Build the project as an executable"
	@echo "  library      - Build the project as a library"
	@echo "  clean        - Clean up build artifacts"
	@echo "  help         - Display this help message"
