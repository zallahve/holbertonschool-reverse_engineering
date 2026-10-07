# Static Analysis

## 0. Extracting and Analyzing Strings
The flag is built byte by byte on the stack in check_flag(), so strings alone does not show it.
It was recovered by disassembling check_flag with objdump.
