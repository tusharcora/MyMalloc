# MyMalloc

A custom implementation of dynamic memory allocation functions (`malloc` and `free`) in C, demonstrating low-level memory management principles and debugging capabilities.

**Author:** Tushar Cora Suresh

---

## Overview

MyMalloc is an educational implementation of memory allocation functions that provides:
- Custom `malloc()` and `free()` functionality
- Error detection and reporting with file/line information
- Performance testing suite
- Simulated heap memory management

This project demonstrates understanding of:
- Memory management fundamentals
- Pointer arithmetic and manipulation
- Metadata structures for tracking allocations
- Fragmentation handling
- Best-fit/first-fit allocation strategies

---

## Project Structure

```
MyMalloc/
├── mymalloc.c      # Core implementation of malloc/free
├── mymalloc.h      # Header file with macros and function declarations
├── memgrind.c      # Comprehensive test suite
├── Makefile        # Build configuration
└── memgrind        # Compiled executable
```

### File Descriptions

**mymalloc.c**
- Implements the core memory allocation and deallocation logic
- Manages a simulated heap using a static character array
- Tracks allocated and free memory blocks using metadata structures
- Handles memory coalescing to reduce fragmentation

**mymalloc.h**
- Defines the `malloc` and `free` macros to include file and line information
- Function prototypes for `mymalloc()` and `myfree()`
- Constant definitions for heap size and metadata structures

**memgrind.c**
- Contains five distinct stress tests for the allocator
- Measures performance and correctness under various scenarios
- Reports timing and error information

**Makefile**
- Compiles the project into the `memgrind` executable
- Links `mymalloc.c` and `memgrind.c`
- Provides clean target for removing build artifacts

---

## Features

### Core Functionality

1. **Custom Memory Allocation**
   - `malloc(size)` - Allocates requested bytes and returns a pointer
   - Returns NULL on failure or if size is 0
   - Includes file and line number tracking via macro expansion

2. **Memory Deallocation**
   - `free(ptr)` - Releases previously allocated memory
   - Validates pointer before freeing
   - Detects common errors (double-free, invalid pointers)

3. **Error Detection**
   - Invalid pointer detection
   - Double-free prevention
   - Out-of-bounds detection
   - Provides file and line number for error reporting

4. **Memory Management**
   - Simulated heap using static array
   - Metadata tracking for each allocation
   - Memory coalescing to reduce fragmentation
   - Best-fit or first-fit allocation strategy

### Error Handling

The implementation detects and reports:
- Freeing addresses not obtained from malloc
- Freeing addresses within allocated chunks (not the start)
- Double-free attempts
- Memory corruption
- Allocation failures when heap is exhausted

Errors include file name and line number for debugging.

---

## Building and Running

### Compilation

```bash
make
```

This produces the `memgrind` executable.

### Running Tests

```bash
./memgrind
```

The test suite automatically runs five stress tests and reports results.

### Cleaning Build Artifacts

```bash
make clean
```

---

## Test Suite

The `memgrind.c` program includes five comprehensive tests:

### Test 1: Rapid Allocation/Deallocation
**Description:** Repeatedly allocates 1 byte and immediately frees it (120 times in a loop).

**Purpose:** Evaluates overhead and performance of the allocator for minimal allocations with immediate deallocation.

**What it tests:**
- Allocation/deallocation cycle efficiency
- Metadata overhead handling
- Memory reuse

---

### Test 2: Sequential Operations
**Description:** Allocates 1 byte and frees it within a single iteration, repeated 120 times.

**Purpose:** Measures allocator overhead for small allocations in rapid succession.

**What it tests:**
- Single-iteration allocation performance
- Memory state consistency
- Repeated allocation from the same memory region

---

### Test 3: Random Allocation Pattern
**Description:** Randomly chooses between allocating and freeing memory over 120 operations.

**Purpose:** Simulates realistic dynamic memory usage with unpredictable allocation patterns.

**What it tests:**
- Fragmentation handling
- Allocator behavior under non-sequential patterns
- Memory coalescing effectiveness
- Robustness with varied allocation/deallocation orders

---

### Test 4: Large Block Allocation
**Description:** Allocates memory blocks close to the maximum heap size (accounting for metadata overhead).

**Purpose:** Assesses efficiency when handling large memory requests.

**What it tests:**
- Large allocation handling
- Near-capacity behavior
- Edge case performance
- Proper space calculation with metadata

---

### Test 5: Variable Size Allocations
**Description:** Manages memory across a range of different allocation sizes.

**Purpose:** Tests allocator performance with diverse memory requirements.

**What it tests:**
- Multi-size allocation handling
- Fragmentation with varied sizes
- Memory utilization efficiency
- Fit algorithm effectiveness

---

## Implementation Details

### Memory Layout

The allocator uses a simulated heap implemented as a static character array. Each allocation includes:

```
[Metadata][User Data]
```

**Metadata structure** typically contains:
- Size of the allocation
- Status (allocated/free)
- Pointer to next block (for linked list traversal)

### Allocation Strategy

The implementation likely uses one of:
- **First Fit:** Returns the first free block large enough
- **Best Fit:** Returns the smallest free block that fits the request
- **Worst Fit:** Returns the largest free block

### Memory Coalescing

When memory is freed, adjacent free blocks are merged to reduce fragmentation.

---

## Usage Example

```c
#include "mymalloc.h"

int main() {
    // Allocate memory
    int *ptr = malloc(sizeof(int) * 10);
    
    if (ptr == NULL) {
        // Allocation failed
        return 1;
    }
    
    // Use the memory
    ptr[0] = 42;
    
    // Free the memory
    free(ptr);
    
    return 0;
}
```

The macros automatically expand to include file and line information:
```c
mymalloc(sizeof(int) * 10, __FILE__, __LINE__)
myfree(ptr, __FILE__, __LINE__)
```

---

## Performance Considerations

- **Metadata Overhead:** Each allocation includes metadata, reducing available space
- **Fragmentation:** Repeated allocations/deallocations can fragment memory
- **Search Time:** Finding suitable free blocks takes O(n) time in worst case
- **Coalescing:** Merging free blocks adds overhead but improves long-term performance

---

## Educational Value

This project demonstrates:
1. **Low-level memory management** without OS syscalls
2. **Pointer arithmetic** and type casting
3. **Metadata structures** for tracking allocations
4. **Linked list** traversal for free block management
5. **Error detection** and debugging techniques
6. **Performance testing** and benchmarking

---

## Limitations

- Fixed heap size (no dynamic expansion)
- No thread safety (single-threaded only)
- Simulated heap in program memory (not actual OS heap)
- Limited to testing environment (not for production use)

---

## Future Enhancements

Potential improvements could include:
- Thread-safe allocation with mutex locks
- Multiple allocation strategies (switchable)
- Memory usage statistics and visualization
- Heap expansion capabilities
- Memory leak detection
- Allocation profiling
