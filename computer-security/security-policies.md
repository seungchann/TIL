# Computer Security  
## Software Policies  

### 1. Security Policies  
#### 1) Definition  
* **Security**  
  * the application and enforcement of policies through defense mechanisms over data and resources  
* **Security Policies**  
  * specify what we want to enforce  
* **Defense mechanisms**  
  * specify how we enforce the policy  
  * i.e., an implementation / instance of a policy  
* Example  
  * **Data Execution Prevention (DEP)** is a mechanism that enforces a **Code Integrity policy** by guaranteeing each page of physical memory in a processes address space is either writable or executable but never both.  

#### 2) Goals  
* Know the core security policies, their domains, and differences  
  * Isolation  
  * Least privilege  
  * Fault compartments  
  * Type safety  
  * Memory safety  
* Understand how type safety and memory safety apply to low level programming languages  
* Be able to give examples on how these policies can be violated  

#### References  
* [Isolation, Least privilege, Fault compartments]()  

### 2. Type Safety  
* Operations on the object always being compatible with the object's type  

### 3. Type Casting & illegal Down Casting  
#### 1) Upcasting  
* converting a child pointer (reference) to a parent class pointer  
#### 2) Downcasting  
* converting a parent pointer (reference) to a child class pointer  
* This may be illegal when a parent pointer points to child's parent object  

### 4. Bug & Vulnerability  
#### 1) Bug  
  * When a system isn't behaving as it's designed to behave  
  * A memory safety violation is a bug.  

#### 2) Vulnerability  
  * A bug who's input can be attacker-controlled is a vulnerability  

### 5. Memory Safety  
> Only accessing their intended referents  
* Memory safety is a general property that can apply to a program, a runtime environment, or a programming language  
* Memory safety prohibits  
  * buffer overflows  
  * NULL pointer dereferences  
  * use after free  
  * use of uninitialized memory  
  * or double frees  
* A program is memory safe, if **all possible executions of that program** are memory safe.  
* A runtime environment is memory safe, if **all runnable programs** are memory safe.  
* A programming language is memory safe, if **all expressible programs** are memory safe.  
* Memory unsafe languages like C/C++ do not enforce memory safety.  
  * data accesses can occur through stale / illegal pointers.  

### 6. Requirements for memory unsafety (C/C++ view)  
> Memory safety violations rely on two conditions  
* Pointer goes out of bounds or becomes dangling  
  * dangling pointer
    * A pointer pointing to a memory location that has been deleted (or freed)  
  * The pointer is dereferenced (used for read or write)  
    * memory dereferenced  
      * getting the value that is stored in the memory location pointed by the pointer  

### 7. Property: Spatial memory safety  
* A property that ensures that all memory dereferences are within bounds of their pointer's valid objects.  
* An object's bounds are defined when the object is allocated.  
* Any computed pointer to that object inherits the bounds of the object.  
* Any pointer arithmetic can only result in a pointer inside the same object.  
* Pointers that point outside of their associated object may not be dereferenced.  * Dereferencing such illegal pointers results in **a spatial memory safety error** and undefined behavior.  

### 8. Spatial memory safety violation  
```c
char *ptr = malloc(24);
for (int i = 0; i < 26; ++i) {
  ptr[i] = i+0x41;
}
```
* Classic buffer overflow  
* Array is sequentially accessed past its allocated length  

### 9. Property: Temporal memory safety  
* A property that ensures that all memory dereferences are valid at the time of the dereference.  
  * i.e., the pointed-to object is the same as when the pointer was created.  
* When an object is freed, the underlying memory is no longer associated to the object and the pointer is no longer valid.  
* Dereferencing such as invalid pointer results in **a temporal memory safety error** and undefined behavior.  

### 10. Temporal memory safety violation  
```c
// 1
char *ptr = malloc(26);
free(ptr);
for (int i = 0; i < 26; ++i) {
  ptr[i] = i+0x41
}
```
```c++
std::vector<int> v { 10, 11 };
int *vptr = &v[1]; // Points *into* 'v'.
v.push_back(12);
std::cout << *vptr; // Bug (use-after-free)
```

### 11. Memory safety: Java  
* Replaces pointers with references (no direct memory access)  
* No way to free data, implicit memory reuse after garbage collection  
* Language and runtime system enforce safety.  
* Examples  
```java
object a = new Object();
a = null; // after this, if there is no reference to the object,
// it will be deleted by the garbage collector  
```
```java
if (something) {
  Object o = new Object();
} // as you leave the block, the reference is deleted.  
// Later on, the garbage collector will delete the object itself.
```

### 12. Memory safety: RUST  
#### 1) What is RUST?  
* Multi-paradigm programming language designed for performance and safety  

#### 2) RUST Ownership  
* All allocated memory is "owned" by a unique someone.  
* Ownership can transfer to an other variable.  

#### 3) How RUST prevent memory safety violations?  
* Preventing **dangling pointer**  
  * Rust can prohibit shared mutable aliases  
* Preventing **buffer overflow / over-read**  
  * Rust generally maintains a length field for the object
  * Rust performs boundary checks automatically during runtime  

#### 4) Need to protect Unsafe Rust  
* Rust has a second language hidden inside it that doesn't enforce these memory safety guarantees  
* Works just like regular Rust, but gives us extra superpowers  
  * Dereference a raw pointer  
  * Call an unsafe function or method  
  * Access or modifty a mutable static variable  

### 13. Memory safety definition  
* Memory safety is **violated** if  
  * undefined memory is accessed
  * either out of bounds or deallocated  
* Pointers become capabilities, they allow access to a well-defined region of allocated memory.  
* A pointer becomes a tuple of address, lower bound, upper bound, and validity.  
  * Pointer arithmetic updates the tuple.  
  * Memory allocation updatese validity.  
  * Dereference checks capability.  
* Capabilities are implicitly added and enforced by the compiler.  

### 14. Memory safety: C / C++  
* Two approaches  
  * Remove unsafe features (dialect)  
  * Protect the use of unsafe features (instrumentation)  

#### 1) C / C++ dialects  
* Extend C / C++ with safe pointers, enforce strict safety rules  
* Example: restrict C to safe subset (e.g., Cyclone)  
  * Limit pointer arithmetic, add NULL checks  
  * Use garbage collection for heap and region lifetimes for stack  
  * Tagged unions (restricting conversions)  
  * Normal, never NULL, and fat pointers  
    * A fat pointer consists of address, base, size  
  * Focuses on both spatial and temporal memory safety  

#### 2) C / C++ instrumentation  
* Must track either pointers or allocated memory  
* Check pointer validity when dereferencing  
* Object bsed
  * cannot detect sub-object overflows  
  * overhead for large lookups  
  * Advantage tha meta data is disjoint resulting in good compatibility  
* Fat pointers  
  * can detect sub-object overflows  
  * low compatibility due to inline metadata  

### 15. Memory safety: policy differences  
#### 1) Object-based policy  
* It stores metadata (size, location) for each allocated object (none for pointers)  
* It allows you to check if an access targets a valid object  
* Object-based schemes cannot distinguish between different pointers  
* Lower security for lower overhead  
#### 2) Pointer-based policy  
* It stores metadata for each pointer  
* Pointer-based schemes allow you to verify if each access is correct for each pointer  

### 16. Summary  
* Security policies protect against specific attack vectors  
* Generic policies: isolation, least privileges, compartmentalization  
* Low level policies: memory and type safety  
* Memory and type safety bugs are the root cause of vulnerabilities  
* Memory safety: distinguish between spatial and temporal  
* Type-safe code accesses only the memory locations it is authorized to access  
  * HexType
    * keep per-object type metadata, explicit cast checks  
* memory safety violations  
  * SoftBound
    * spatial memory safety with disjoint pointer metadata  
  * CETS
    * temporal memory safety through versioning  
* SoftBound, CETS and HexType are sanitizers  
  * They trade performance for security during development  
