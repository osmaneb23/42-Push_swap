# 🔄 Push_swap - Stack Sorting Algorithm

Welcome to my **Push_swap** repository! This project is part of the **42 curriculum**, where I implemented an efficient algorithm to sort data in a stack with a restricted set of operations. This project deepened my understanding of sorting algorithms, algorithmic complexity, and data structure optimization.

---

## **✅ Project Validation**
- **Validated on:** July 18, 2024
- **Final Score:** 125/100
  - Achieved **bonus part** 🎉

---

## **📜 Project Overview**
The goal of this project was to create a program that sorts integers using two stacks and a limited set of operations, with the objective of finding the most efficient sorting solution possible. The challenge lies not just in sorting the numbers, but in doing so with the fewest possible operations.

### **Key Requirements:**
- Work with exactly two stacks: stack A (containing the initial integers) and stack B (initially empty)
- Sort the integers in stack A in ascending order using only the allowed operations
- Output the sequence of operations that sorts the stack
- Handle error cases properly (non-integer inputs, duplicates, integers out of range)
- Meet performance benchmarks:
  - Sort 100 random numbers in fewer than 700 operations
  - Sort 500 random numbers in fewer than 5500 operations

### **Allowed Operations:**
- **sa**: Swap the first 2 elements at the top of stack A
- **sb**: Swap the first 2 elements at the top of stack B
- **ss**: Execute sa and sb simultaneously
- **pa**: Push the top element from stack B to stack A
- **pb**: Push the top element from stack A to stack B
- **ra**: Rotate stack A (first element becomes last)
- **rb**: Rotate stack B (first element becomes last)
- **rr**: Execute ra and rb simultaneously
- **rra**: Reverse rotate stack A (last element becomes first)
- **rrb**: Reverse rotate stack B (last element becomes first)
- **rrr**: Execute rra and rrb simultaneously

### **Restrictions:**
- The project had to be coded in C
- Had to follow the 42 Norm (coding style guidelines)
- No memory leaks allowed
- No global variables
- Only allowed to use these functions:
  - read, write, malloc, free, exit
  - My own Libft and ft_printf
- Had to implement a Makefile with required rules

---

## **🛠️ Implementation Details**

### **🧠 Turkish Sorting Algorithm**
I implemented the Turkish sorting algorithm, which is particularly efficient for this problem:
- **Indexing**: First assigns indices (0 to n-1) to values in ascending order
- **Cost-Based Selection**: For each element in stack B, calculates the "cost" to move it to its correct position in stack A
- **Target Calculation**: Identifies the optimal position in stack A for each element in stack B
- **Optimal Rotation**: Uses combined operations (rr/rrr) when rotating both stacks in the same direction
- **Pre-sorting**: Keeps a small number of elements in stack A initially to optimize the sorting process

### **⚙️ Key Optimization Techniques**
- Calculating the most efficient rotation direction for each stack
- Combining rotations of both stacks when possible to reduce operations
- Selecting the element with the lowest total cost to move next
- Special handling for small sets of numbers (3-5 elements)
- Intelligent target position calculation to minimize future moves

### **⚠️ Error Handling**
- Robust input validation to detect:
  - Non-integer inputs
  - Integers exceeding MAX/MIN_INT
  - Duplicate numbers
  - Proper formatting of arguments
- Clear error messages on standard error output

---

## **📂 Project Structure**
```
push_swap/
│── src/                            # Source files for mandatory part
│   │── costs.c                     # Cost calculation for moves
│   │── error.c                     # Error handling functions
│   │── free.c                      # Memory management
│   │── initialisation.c            # Stack initialization
│   │── main.c                      # Entry point
│   │── operations.c                # Basic stack operations
│   │── operations2.c               # Additional stack operations
│   │── operations3.c               # Combined stack operations
│   │── push_swap.h                 # Header file with function prototypes
│   │── rotates.c                   # Rotation operations
│   │── sort.c                      # Main sorting algorithms
│   │── split.c                     # String splitting for parsing
│   │── split_utils.c               # Utilities for string parsing
│   │── targets.c                   # Target position calculations
│   │── utils.c                     # General utility functions
│   │── utils_index.c               # Index-related utilities
│   └── utils_nodes.c               # Linked list node utilities
│── checker_bonus/                  # Bonus part implementation
│   │── checker_bonus.h             # Checker header file
│   │── get_next_line_bonus.c       # Line reading for instruction input
│   │── get_next_line_utils_bonus.c # Utilities for get_next_line
│   │── main_bonus.c                # Entry point for checker
│   └── Makefile                    # Compilation for bonus part
└── Makefile                        # Main compilation instructions
```

---

## **🧮 Sorting Algorithm Explained**

### **Small Stack Sorting (2-3 elements)**
For very small stacks, I used hardcoded optimal sequences:
- For 2 elements: Simple comparison and swap if needed
- For 3 elements: Optimized sequence based on the initial configuration (at most 2 operations)

### **Large Stack Sorting with Turkish Algorithm**
For larger stacks, the Turkish algorithm follows these steps:

1. **Indexing**: Convert actual values to indices (0 to n-1) to simplify comparisons
2. **Initial Push**: Keep 2-3 smallest elements in stack A, push others to stack B
3. **Cost Calculation**: For each element in stack B:
   - Calculate cost to bring it to the top of stack B
   - Calculate cost to prepare stack A for insertion
   - Find the total cost considering combined rotations
4. **Element Selection**: Choose the element with lowest total cost 
5. **Execute Moves**: Perform the operations to move this element
6. **Final Rotation**: Rotate stack A until the smallest element is at the top

---

## **📊 Performance Results**
My implementation achieves excellent performance, well within the project requirements:

| Input Size | Average Operations | Requirement |
|------------|------------------- |-------------|
| 3          | 2-3                | -           |
| 5          | 8-12               | -           |
| 100        | ~550-650           | <700        |
| 500        | ~4800-5300         | <5500       |

---

## **🔧 Installation & Usage**

### **📋 System Requirements**
- Unix-based operating system (Linux/macOS)
- C compiler (gcc/cc)
- Make

### **📥 Clone & Compile**
```
# Clone the repository
git clone https://github.com/osmaneb23/42-Push_swap.git
cd 42-Push_swap

# Compile the main program
make

# Compile the checker (bonus)
make bonus
```

### **🚀 Running the Program**

#### **Main Program Usage**
```
# Format: ./push_swap [integers]
./push_swap 4 2 7 1 5

# Using random numbers (bash example)
ARG=$(shuf -i 0-100 -n 10 | tr "\n" " "); ./push_swap $ARG
```

The program will output the sequence of operations needed to sort the stack.

#### **Checker Usage (Bonus)**
```
# Test if sorting works correctly with push_swap output
ARG="4 2 7 1 5"; ./push_swap $ARG | ./checker_bonus/checker $ARG
OK  # If sorting was successful

# Test with specific sequence of operations
./checker_bonus/checker 3 2 1
sa
rra
# Type operations manually, end with Ctrl+D
OK  # If sorting was successful

# Testing with large random datasets
ARG=$(shuf -i 0-999 -n 100 | tr "\n" " "); ./push_swap $ARG | ./checker_bonus/checker $ARG
OK

# Testing with edge cases
./checker_bonus/checker 1 2 3  # Already sorted
# Press Ctrl+D without entering operations
OK

./checker_bonus/checker 3 1 2
sa
ra
# Press Ctrl+D after entering operations
OK
```

---

## **⚠️ Error Handling**
The program provides specific error messages for different issues:
- Non-integer inputs
- Integers exceeding MAX/MIN_INT limits 
- Duplicate numbers
- Improper formatting

Examples:
```
# Error with non-integer input
./push_swap 42 abc 10
Error

# Error with duplicate numbers
./push_swap 1 3 5 1 7
Error

# Error with out-of-range integer
./push_swap 2147483648
Error

# Checker also handles errors with operations
./checker_bonus/checker 1 2 3
abc  # Invalid operation
Error
```

---

## **🔍 Bonus Part: Checker Program**
The checker program validates whether a sequence of operations correctly sorts a stack:

1. It takes the same integer arguments as push_swap
2. It reads operations from standard input (one per line)
3. It applies these operations to the stack
4. It verifies if the stack is sorted after all operations
5. It outputs "OK" if sorted, "KO" if not, or "Error" for invalid inputs/operations

This tool is invaluable for testing the correctness of the sorting algorithm's output.

---

## **🎓 Key Learning Outcomes**
- **Algorithm Efficiency**: Understanding time and space complexity in sorting algorithms
- **Data Structures**: Implementing and optimizing linked lists for stack operations
- **Optimization Techniques**: Finding the balance between code clarity and performance
- **Problem Decomposition**: Breaking down a complex sorting problem into manageable parts
- **Error Management**: Implementing robust error handling and input validation
- **Algorithm Design**: Creating a hybridized Turkish algorithm tailored to specific constraints
- **Testing Strategies**: Developing methods to verify algorithm correctness and efficiency
- **Memory Management**: Ensuring proper allocation and deallocation in C
- **Code Organization**: Structuring a multi-file project with clear responsibilities