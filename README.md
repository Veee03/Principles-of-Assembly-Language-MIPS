# Principles-of-Assembly-Language-MIPS



###### A2 Stage 2 Part A Starter Code plus my code ########

.data # this .data section is where you can simulate stored data in random access memory 

	array: .word 5, 2, 6, 7, 1 # you can write any label which ends in a colon such as 'array:' <- PUT YOUR STUDENT NUMBERS HERE
	length: .word 5 # <--- THIS IS THE NUMBER OF ELEMENTS IN THE ARRAY
	
	
.text #this section is where you write your main script

	main:
		la $t0, array # la is load address base address  <---- loads the base ADDRESS of the array into $t0
		li $t1, 0     # i=0 
		lw $t2, length #store length 5 into $t2
		li $t3, 0 #sum = 0
 		
############### COMPLETE YOUR CODE BELOW THIS LINE #######			
			loop: bge $t1, $t2, loop_done   ## if i(counter) is >=  reaches 5 exit loop              
		  lw $t6, 0($t0)                  ## get current array element at address $t0 into $t6
			add $t3, $t3, $t6               ## updating sum, add current number to running total: sum = sum + current number
      bge $t6, $t4, not_min           ##checking for new min, if  current number >= to current min, skip to not_min 
      move $t4, $t6                   ## new min found, update min to the register 
      not_min:
      ble $t6, $t5, not_max           ## for new max we do, if current number is <= current max, skip to not_max 
      move $t5, $t6                   ## otherwise ^ new max found and new max in updated to register
      not_max:

      addi $t0, $t0 4                 ## moving array pointer to next element : add 4 bytes (size of one word)
      addi $t1, $t1 1                 ## this part is a incremented loop counter: i = i +1
      j loop                          ## jump back to start of the loop to process next element 

    loop_done:                        ## after loop is done calculate average
      div $t3, $t2                    ## divide sum (in $t3) by length (in $t2)
      mflo $t7                        ## moves from LO: get quotient (interger result) into $t7 for average



    li $t1, 0          # i = 0 (counter)
    li $t2, 0          # sum = 0
    li $t3, 9999       # min = big number to start
    li $t4, -1         # max = very small number to start

loop:
    beq $t1, 5, end     # if i == 5, break

    mul $t5, $t1, 4     # t5 = i * 4 (byte offset)
    add $t6, $t0, $t5   # t6 = address of array[i]
    lw $t7, 0($t6)      # t7 = array[i]

    add $t2, $t2, $t7   # sum += array[i]

    # Check min
    blt $t7, $t3, newmin
    j checkmax

newmin:
    move $t3, $t7       # min = array[i]

checkmax:
    bgt $t7, $t4, newmax
    j next

newmax:
    move $t4, $t7       # max = array[i]

next:
    addi $t1, $t1, 1    # i++
    j loop

end:

## Calculate Average
    li $t8, 5
    div $t2, $t8        # divide sum by 5
    mflo $t9            # $t9 now has the average

################COMPLETE YOUR CODE ABOVE THIS LINE #######
		
		
		#terminate program 
		li $v0 10     #### THIS IS A GOOD WAY TO END THE PROGRAM BY PUTTING THE VALUE 10 INTO $v0 AND USING SYSCALL
		syscall

