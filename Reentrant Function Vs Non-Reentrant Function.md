<h1 align="center"> Reentrant Function Vs Non-Reentrant Function </h1>

<h3 align="center"> A reentrant function is a function that can be interrupted in the middle of its execution and safely called again ("re-entered") before its previous invocations are complete. A non-reentrant function, on the other hand, cannot safely be interrupted and re-entered. </h3>

<h3 align="left"> Use Cases </h3>

<p align="left"> --> Reentrant functions are essential in real-time systems, multi-threaded applications, and systems with signal handlers where functions might be interrupted and called again before the previous execution is complete. </p>

<p align="left"> --> Non-reentrant functions are often found in single-threaded applications where concurrent access is not an issue. </p>

<h3 align="left"> Key Differences </h3>

<h4 align="left"> 1) Global and Static Variables: </h4>
<p align="left"> --> Reentrant function: Does not use global or static variables unless access is synchronized. Typically, all state information is passed in via parameters or allocated dynamically. </p>
<p align="left"> --> Non-reentrant function: May use global or static variables without protection mechanisms, leading to potential data corruption if re-entered. </p>

<h4 align="left"> 2) Local Variables: </h4>
<p align="left"> --> Reentrant function: Uses local variables and parameters exclusively for maintaining state. </p>
<p align="left"> --> Non-reentrant function: Local variables are safe since they are on the stack, but the function might still be non-reentrant if it uses non-local state. </p>

<h4 align="left"> 3) Interrupt Safety: </h4>
<p align="left"> --> Reentrant function: Can be safely interrupted and called again, making it suitable for use in signal handlers and multi-threaded applications. </p>
<p align="left"> --> Non-reentrant function: Cannot be safely interrupted and resumed, which can be problematic in multi-threaded or interrupt-driven environments. </p>

<h4 align="left"> 4) System Calls: </h4>
<p align="left"> --> Reentrant function: Avoids system calls that are not reentrant or ensures they are used in a thread-safe manner. </p>
<p align="left"> --> Non-reentrant function: May make system calls that are not reentrant, thus relying on external state that could change unexpectedly. </p>

<h2 align="left"> Examples </h2>

<h3 align="left"> Reentrant Function </h3>

<p align="left"> void increment(int *counter) {  </p>
<p align="left"> (*counter)++; // Safe: uses a pointer to a local variable </p>
<p align="left"> } </p>



<h3 align="left"> Non-reentrant Function </h3>
<p align="left"> int counter = 0; </p>
<p align="left"> void increment() { </p>
<p align="left">     counter++; // Unsafe: uses a global variable </p>
<p align="left"> } </p>


<h2 align="left"> Best Practices </h2>

<h4 align="left"> 1) Avoid global and static variables: Pass all necessary state information through function parameters or dynamically allocated memory. </h4>
<h4 align="left"> 2) Use thread-local storage: If using global or static variables is necessary, consider using thread-local storage to maintain state that is local to each thread. </h4>
<h4 align="left"> 3) Synchronous access: If global or static variables must be used, ensure access is synchronized using mutexes or other synchronization mechanisms. </h4>


<h3 align="center"> In AUTOSAR (AUTomotive Open System ARchitecture), the distinction between reentrant and non-reentrant functions is crucial due to the concurrent and real-time nature of automotive systems. AUTOSAR provides specific guidelines for ensuring that functions are designed to be reentrant where necessary to avoid issues in multi-threaded and interrupt-driven environments. </h3>

<h2 align="left"> Reentrant Functions in AUTOSAR </h2>

<h3 align="left"> Characteristics: </h3>

<h4 align="left"> 1) No Global or Static Variables: Reentrant functions do not use global or static variables. They rely on parameters and local variables to maintain state. </h4>
<h4 align="left"> 2) Stack Usage: Each invocation of a reentrant function has its own stack frame, ensuring no shared state between different invocations. </h4>
<h4 align="left"> 3) Interrupt Safety: Reentrant functions can be safely interrupted and called again, which is essential for interrupt service routines (ISRs) and multi-threaded applications. </h4>

<h2 align="left"> Example: </h2>

<p align="left"> #include "Std_Types.h"                                             </p>
<p align="left"> // A reentrant function in AUTOSAR                                 </p>
<p align="left"> Std_ReturnType CalculateSum(uint16 a, uint16 b, uint16 *result) {  </p>
<p align="left">    if (result == NULL) {                                           </p>
<p align="left">        return E_NOT_OK; // Error if result pointer is NULL         </p>
<p align="left">   }                                                                </p>
<p align="left">    *result = a + b;                                                </p>
<p align="left">   return E_OK;                                                     </p>
<p align="left">}                                                                   </p>


<h2 align="left"> Non-Reentrant Functions in AUTOSAR </h2>  

<h3 align="left"> Characteristics: </h3>

<h4 align="left"> 1) Global or Static Variables: Non-reentrant functions may use global or static variables without protection, leading to potential data corruption if re-entered. </h4>

<h4 align="left"> 2) Single Instance: Designed to be used in a single-threaded context or protected by synchronization mechanisms when used in multi-threaded environments. </h4>


<h2 align="left"> Example: </h2>

<p align="left"> #include "Std_Types.h"
<p align="left"> static uint16 globalCounter = 0;
<p align="left"> // A non-reentrant function in AUTOSAR  </p>
<p align="left"> Std_ReturnType IncrementCounter(void) { </p>
<p align="left">  globalCounter++;                       </p>
<p align="left">     return E_OK;                        </p>
<p align="left"> }                                       </p>


<h2 align="left"> Best Practices in AUTOSAR </h2>

<h3 align="left"> 1) Reentrant Functions: </h3>
<p align="left"> 1- Use for ISRs and in multi-threaded contexts where functions might be interrupted or called simultaneously.</p>
<p align="left"> 2- Ensure no use of global or static variables unless synchronized. </p>

<h3 align="left"> 2) Non-Reentrant Functions: </h3>
<p align="left"> 1- Use in single-threaded contexts or protect using synchronization mechanisms (e.g., mutexes) when shared in multi-threaded environments. </p>
<p align="left"> 2- Carefully manage and document any global or static variables to avoid race conditions. </p>
  
<h3 align="left"> 3) Code Analysis: </h3>
<p align="left"> 1- Perform static analysis to ensure reentrancy properties are maintained. </p>
<p align="left"> 2- Use AUTOSAR compliance tools to check for adherence to guidelines. </p>
