# hp35s-prime-finder

Check if a number is prime on an HP35s calculator.

Input a number into stack position X, run the program, answer output in stack position Y: 1 indicates prime, 0 indicates not prime, -1 indicates an error.

## Output

```
    N is prime:
    Stack Y: 1
    Stack X: 7

    N is not prime:
    Stack Y: 0
    Stack X: 9

    N is too large:
    Stack Y: -1
    Stack X: 1236987
```

## Source

```
        Program:
    P001    LBL P           // Label program "P".
    P002    STO N           // Store current stack x value in variable N.
    P003    SF 10           // Set flag 10 to disable equation evaluation (to enable string printing).
    P004    GTO P023        // Skip to main program function.
    P005    CLSTK           // "Number is prime" subroutine.
    P006    1               // Put 1 (for true) in stack y.
    P007    RCL N           // Put the original number back in stack x.
    P008    N IS PRIME      // Output message.
    P009    PSE             // Pause for 1 second.
    P010    RTN             // Terminate program.
    P011    CLSTK           // "Number is not prime" subroutine.
    P012    0               // Put 0 (for false) in stack y.
    P013    RCL N           // ...
    P014    N ISNT PRIME    // ...
    P015    PSE             // ...
    P016    RTN             // Terminate program.
    P017    CLSTK           // "Error" subroutine (if root of number is too large).
    P018    -1              // Put -1 (for error) in stack y.
    P019    RCL N           // ...
    P020    N IS TOO BIG    // ...
    P021    PSE             // ...
    P022    RTN             // Terminate Program.
    P023    2               // Main function! Check if N is less than 2...
    P024    RCL N           // ...
    P025    x<y?            // ...
    P026    GTO P011        // If true, enter "Number is not prime" subroutine.
    P027    4               // Else, check if N is less than 4 (N can only be 2, 3, both are primes)...
    P028    RCL N           // ...
    P029    x<y?            // ...
    P030    GTO P005        // If true, enter "Number is prime" subroutine.
    P031    RCL N           // Else, check if N is an even number...
    P032    2               // ...
    P033    RMDR            // Perform modulo via remainder division.
    P034    x=0?            // If result is 0...
    P035    GTO P011        // ...enter "Number is not prime" subroutine.
    P036    RCL N           // Else, calculate squareroot of N for upper limit of divisor check...
    P037    SQRT(x)         // ...
    P038    1               // Add 1 to squareroot.
    P039    +               // ...
    P040    1               // ...
    P041    INT/            // Perform floor division make squareroot a whole number.
    P042    STO R           // Store squareroot in variable R.
    P043    999             // For loop past 999 is impossible, check if root is greater than 999.
    P044    RCL R           // ...
    P045    x>y?            // ...
    P046    GTO P017        // If greater than 999, enter "Error" subroutine.
    P047    RCL R           // Format ISG for loop initialization expression in form START_N.END_N-INCREMENT.
    P048    1000            // ...                           e.g. if start=3, end(root)=17, inc=2: "3.01702".
    P049    /               // ...
    P050    1               // ...
    P051    +               // ...
    P052    0.00002         // ...
    P053    +               // ...
    P054    STO R           // Store init expression in variable R.
    P055    ISG R           // Begin ISG loop with expression.
    P056    GTO P058        // If loop evaluates to true, test for divisors with current R value...
    P057    GTO P005        // Else, enter "Number is prime" subroutine.
    P058    RCL R           // Utilize floor division to extract current increment value.
    P059    1               // ...
    P060    INT/            // ...
    P061    STO I           // Store current increment value in variable I.
    P062    RCL N           // Test N % I == 0 via remainder division.
    P063    RCL I           // ...
    P064    RMDR            // ...
    P065    x=0?            // If number divides evenly...
    P066    GTO P011        // ...enter "Number is not prime" subroutine.
    P067    GTO P055        // Else, increment I value and restart loop.
```

Note that the hardware is capable of determining the primality of very large numbers, however it can take an extremely long time due to the slow speed of the processor. I've imposed a limit (the maximum value that an ISG loop can go up to) on the root of the input number, which will result in an error code -1 if execution is attempted with too large a number.
