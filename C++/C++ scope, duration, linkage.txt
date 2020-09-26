C++ learnings

# initialization

- copy initialization     int name = 1
- direct initialization   int name(1)
- unfirom initialization  int name{1}

# variable properties

- scope => determines where a variable is accessible 
  - local scope/ block scope
  - global scope /file scope
- duration => when its created or destroyed
  - automatic duration / static duration / dynamic duration
- linkage => whether multiple instances of an identifier refers to the same identifier or not
  - no linkage
    - normal local variables / user defined types in blocks 
  - internal linkage
    - internal variable (static variable), can be used anywhere in the file they are defined but not outside
    - const variables declared outside of a block are assumed to be internal.  
  - external linkage 
    - can be used both within the file and as well as other files (extern keyword)
    - By default, non-const variables declared outside of a block are assumed to be external
	
	
// Uninitialized definition:
int g_x;        // defines uninitialized global variable (external linkage)
static int g_x; // defines uninitialized static variable (internal linkage)
const int g_x;  // not allowed: const variables must be initialized
 
// Forward declaration via extern keyword:
extern int g_z;       // forward declaration for global variable defined elsewhere
extern const int g_z; // forward declaration for const global variable defined elsewhere
 
// Initialized definition:
int g_y(1);        // defines initialized global variable (external linkage)
static int g_y(1); // defines initialized static variable (internal linkage)
const int g_y(1);  // defines initialized static variable (internal linkage)
 
// Initialized definition w/extern keyword:
extern int g_w(1);       // defines initialized global variable (external linkage, extern keyword is redundant in this case)
extern const int g_w(1); // defines initialized const global variable (external linkage)

# static duration variables
- when static is applied to local variables, its duration goes from automatic duration to static duration

# summary
- Functions and types don’t have scope or duration

