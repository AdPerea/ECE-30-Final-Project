////////////////////////
//                    //
// Project Submission //
//                    //
////////////////////////

// Partner1: Adrian Perea Adams, A17133531

////////////////////////
//                    //
//       main         //
//                    //
////////////////////////

    // Print Input Array
    lda x0, arr1        // x0 = &list1
    lda x1, arr1_length // x1 = &list1_length
    ldur x1, [x1, #0]   // x1 = list1_length
    bl printList

    // Test Swap Function
    bl printSwapNumbers // print the original values
    lda x0, swap_test   // x0 = &swap_test[0]
    addi x1, x0, #8     // x1 = &swap_test[1]
    bl Swap             // Swap(&swap_test[0], &swap_test[1])
    bl printSwapNumbers // print the swapped values

    // Test GetNextGap Function
    addi x0, xzr, #1    // x0 = 1
    bl GetNextGap       // x0 = GetNextGap(1) = 0
    putint x0           // print x0
    addi x1, xzr, #32   // x1 = ' '
    putchar x1          // print x1

    addi x0, xzr, #6    // x0 = 6
    bl GetNextGap       // x0 = GetNextGap(6) = 3
    putint x0           // print x0
    addi x1, xzr, #32   // x1 = ' '
    putchar x1          // print x1

    addi x0, xzr, #7    // x0 = 7
    bl GetNextGap       // x0 = GetNextGap(7) = 4
    putint x0           // print x0
    addi x1, xzr, #10   // x1 = '\n'
    putchar x1          // print x1


    // Test inPlaceMerge Function
    lda x0, merge_arr_length // x1 = &merge_arr1_length
    ldur x0, [x0, #0]        // x0 = merge_arr1_length
    bl GetNextGap            // x0 = GetNextGap(merge_arr1_length)
    addi x2, x0, #0          // x2 = x0 = gap
    lda x0, merge_arr        // x0 = &merge_arr1
    lda x3, merge_arr_length // x3 = &merge_arr1_length
    ldur x3, [x3, #0]        // x3 = merge_arr1_length
    subi x3, x3, #1          // x3 = x3 - 1     to get the last element
    lsl x3, x3, #3           // x3 = x3 * 8 <- convert length to bytes
    add x1, x3, x0           // x1 = x3 + x0 <- x1 = &merge_arr1[0] + length in bytes
    bl inPlaceMerge          // inPlaceMerge(&merge_arr1[0], &merge_arr1[0] + length in bytes, gap)
    lda x0, merge_arr
    lda x1, merge_arr_length // x1 = &merge_arr1_length
    ldur x1, [x1, #0]        // x1 = list1_length
    bl printList             // print the merged list


    // Test MergeSort Function
    lda x0, arr1            // x0 = &merge_arr1
    lda x2, arr1_length     // x2 = &merge_arr1_length
    ldur x2, [x2, #0]       // x2 = merge_arr1_length
    subi x2, x2, #1         // x2 = x2 - 1     to get the last element
    lsl x2, x2, #3          // x2 = x2 * 8 <-- convert length to bytes
    add x1, x2, x0          // x1 = x2 + x0 <-- x1 = &merge_arr1[0] + length in bytes
    bl MergeSort            // inPlaceMerge(&merge_arr1[0], &merge_arr1[0] + length in bytes, gap)
    lda x1, arr1_length     // x1 = &list1_length
    ldur x1, [x1, #0]       // x1 = list1_length
    bl printList            // print the merged list


    // [BONUS QUESTION] Binary Search Extension
    // load the sorted array's start and end indices
    lda x0, arr1            // x0 = &merge_arr1
    lda x2, arr1_length     // x2 = &merge_arr1_length
    ldur x2, [x2, #0]       // x2 = merge_arr1_length
    subi x2, x2, #1         // x2 = x2 - 1     to get the last element
    lsl x2, x2, #3          // x2 = x2 * 8 <-- convert length to bytes
    add x1, x2, x0          // x1 = x2 + x0 <-- x1 = &merge_arr1[0] + length in bytes

    stop

////////////////////////
//                    //
//        Swap        //
//                    //
////////////////////////
Swap:
    // input:
    //     x0: the address of the first value
    //     x1: the address of the second value

    ldur x9,  [x0, #0]       // Copy first value into temp variable x9 = *x0
    ldur x10, [x1, #0]       // Copy second value into temp variable x10 = *x1
    stur x9,  [x1, #0]       // Store first value at address of the second value.
    stur x10, [x0, #0]       // Store second value at address of first value.

    br lr                    // Return from function

////////////////////////
//                    //
//     GetNextGap     //
//                    //
////////////////////////
GetNextGap:
    // input:
    //     x0: The previous value for gap

    // output:
    //     x0: the updated gap value

    subis xzr, x0, #1       // If gap > 1 compute next gap
    b.gt ComputeGap         // Compute next gap
    addi x0, xzr, #0        // Else return 0
    br lr                   // Return from function
ComputeGap:  
    andi x9, x0, #1         // Set temp variable x10 = 1 if gap is odd or x10 = 0 if gap is even
    add x0, x0, x9          // Set x0 = gap + 1 if gap is odd or x0 = gap if gap is even
    lsr x0, x0, #1          // Compute updated gap value x0 = (gap + gap&1)/2
    br lr                   // Return from function


////////////////////////
//                    //
//    inPlaceMerge    //
//                    //
////////////////////////
inPlaceMerge:
    // input:
    //    x0: The address of the starting element of the first sub-array.
    //    x1: The address of the last element of the second sub-array.
    //    x2: The gap used in comparisons for shell sorting

    addi x12, x0, #0        // Save copy of starting address
    addi x13, x1, #0        // Save copy of end address
    addi x14, lr, #0        // Save return link return
ipmMainLoop:
    subis xzr, x2, #1       // If gap < 1 return from function
    b.lt exitIPM            // Return from function
    lsl x11, x2, #3         // x11 = x2 * 8 <- convert length to bytes
    add x1, x0, x11         // Set x1 = left + gap    
ipmInnerLoop:
    ldur x9, [x0, #0]       // get a[left]
    ldur x10, [x1, #0]      // get a[left + gap]
    subs xzr, x9, x10       // if a[left] <= a[left + gap] skip swap
    b.le ipmOuterLoop       // Skip swap
    bl Swap                 // swap a[left] with a[left + gap]
ipmOuterLoop:
    // Increment x0 and x1
    addi x0, x0, #8         // left++
    add x1, x0, x11         // (left++) + gap 
    // Check if new x1 = (left++) + gap is out of bounds of the array
    subs xzr, x1, x13       // If (left++) + gap <= end re-enter inner loop
    b.le ipmInnerLoop       // enter inner loop
    // Else get new gap value
    addi x0, x2, #0         // Set input x0 = x2 (gap value)
    bl GetNextGap           // Get next gap value
    addi x2, x0, #0         // Set x2 to new gap value
    addi x0, x12, #0        // Reset x0 = address of starting element
    addi x1, x13, #0        // Reset x1 = address of last element
    b ipmMainLoop           // Continue
exitIPM:
    addi lr, x14, #0        // Retrieve saved link return
    br lr                   // Return from function


////////////////////////
//                    //
//      MergeSort     //
//                    //
////////////////////////
MergeSort:
    // input:
    //     x0: The starting address of the array.
    //     x1: The ending address of the array

      // save stack pointer prior to sorting
    addi x5, sp, #0
    // save return address of caller
    addi x6, lr, #0
msMainLoop:
    // store first and last
    subi sp, sp, #32        // allocate stack frame
    stur fp, [sp, #0]       // save old frame pointer
    addi fp, sp, #24        // set new frame pointer
    stur x1, [fp, #-16]     // save the current end address
    stur x0, [fp, #0]       // save the current start address
    // calculate mid
    lsr x9, x0, #3          // x9 = x0/8 <- convert bytes to index
    lsr x10, x1, #3         // x10 = x1/8 <- convert bytes to index
    add x10, x9, x10        // compute start index + end index
    lsr x10, x10, #1        // compute mid (x9 = x9/2)
    lsl x10, x10, #3        // x9 = x9 * 8 <- convert index to bytes
    // store mid
    stur x10, [fp, #-8]     // save the current mid address

    // if first == mid:
    subs xzr, x0, x1
    b.eq msSubLoop          // reenter subloop
    // else:
    addi x1, x10, #0        // last = mid
    b msMainLoop            // continue main loop
msSubLoop:
    // Compute initial gap value and set x0 = gap
    lsr x0, x0, #3          // x0 = x0/8 <- convert byte address to index
    lsr x10, x1, #3         // x10 = x1/3 <- convert byte address to end index
    sub x0, x10, x0         // x0 = end index - start index
    addi x0, x0, #1         // x0 = x0 + 1
    bl GetNextGap           // call GetNextGap function to retrieve the next gap
    addi x2, x0, #0         // save the new gap in x2
    ldur x0, [fp, #0]       // restore x0 = start index
    // Perform in place merge
    bl inPlaceMerge         // call inPlaceMerge function
    ldur x9, [fp, #-16]     // Temporarily save the current end address
    // Pop Stack
    ldur fp, [fp, #-24]     // Pop previous frame pointer off stack
    addi sp, sp, #32        // Deallocate memory
    // Check if stack is empty
    subs xzr, sp, x5        // if current sp points to saved sp (prior to stack population), stack is empty. Return from function.
    b.eq exitMergeSort      // Return from function
    ldur x0, [fp, #0]       // Restore start address
    ldur x1, [fp, #-16]     // Restore end address

    // Check if current end address == previous end address:
    subs xzr, x1, x9        // if current end address == previous end address repeat subloop
    b.eq msSubLoop          // repeat subloop
    // else:
    ldur x10, [fp, #-8]     // restore middle address
    addi x0, x10, #8        // set start address = middle + 1
    // else:
    b msMainLoop            // continue main loop
exitMergeSort:
    addi lr, x6, #0         // retrieve original return address
    br lr                   // return from function

////////////////////////
//                    //
//     printList      //
//                    //
////////////////////////

printList:
    // x0: start address
    // x1: length of array
    addi x3, xzr, #32       // x3 = ' '
    addi x4, xzr, #10       // x4 = '\n'
printList_loop:
    subis xzr, x1, #0       // if (x1 == 0) break
    b.eq printList_loopEnd  // break
    subi x1, x1, #1         // x1 = x1 - 1
    ldur x2, [x0, #0]       // x2 = x0->val
    putint x2               // print x2
    addi x0, x0, #8         // x0 = x0 + 8
    putchar x3              // print x3 ' '
    b printList_loop        // continue
printList_loopEnd:
    putchar x4              // print x4 '\n'
    br lr                   // return


////////////////////////
//                    //
//  helper functions  //
//                    //
////////////////////////
printSwapNumbers:
    lda x2, swap_test   // x0 = &swap_test
    ldur x0, [x2, #0]   // x1 = swap_test[0]
    ldur x1, [x2, #8]   // x2 = swap_test[1]
    addi x3, xzr, #32   // x3 = ' '
    addi x4, xzr, #10   // x4 = '\n'
    putint x0           // print x1
    putchar x3          // print ' '
    putint x1           // print x2
    putchar x4          // print '\n'
    br lr               // return
