      .MODEL SMALL

.DATA  
      menu1 DB "Select an operation (1-7): $", 0
    operation DB "Operation selected: $", 0
    result_msg DB "Result: $", 0
    newline DB 0Dh, 0Ah, 0
     temp DW ?
   
    res DW ?
    
  
   
    
    ;Messages
    THANK DB 0AH, 0AH, 0AH, 0DH, "                          Thank you for using our  application $"
    INVALID DB 0AH, 0AH, 0DH, " Invalid input. Please choose a different option.$"
    CHOOSE DB 0AH, 0AH, 0DH, " Enter option: $"
    SUCCESS DB 0AH, 0AH, 0DH, " RESULT $ "
    
  
    ;Withdraw
    num1 DB 0AH, 0AH, 0DH, " Enter number1 $ " 
     num2 DB 0AH, 0AH, 0DH, " Enter number2 $ "
    
    INVALID_INPUT DB 0AH, 0AH, 0DH, " INVALID INPUT"
    
   
    
    
    ;Digit place
    TH DW 10000
    T DW 10 
    

.CODE
    MAIN PROC
    MOV AX, DATA
    MOV DS, AX
    
   
   
   
                
    
    ;Display the menu
    MENU:    
          MOV AH, 09H
          LEA DX, menu1
          INT 21H
          
         
       
          
    CHOOSE_OPTION: MOV AH, 09H
                   LEA DX, CHOOSE
                   INT 21H
                   MOV AH, 01H
                   INT 21H 
                   
                   CMP AL, 49
                   JE do_add
                   CMP AL, 50
                   JE do_sub
                   CMP AL, 51
                   JE do_mul
                   CMP AL, 52
                   JE do_div
                    CMP AL, 53
                   JE do_neg
                  
                    CMP AL, 54
                   JE do_inc
                    CMP AL, 55
                   JE do_dec
                   
                   cmp al,56
                   JE Exit
                   jmp INP_ERROR
                   
   
    
    do_add:
                         
              MOV AH, 09H
              LEA DX, num1
              INT 21H
              CALL INPUT_4DIGIT_NUM
                push bx  
                   MOV AH, 09H
              LEA DX, num2
              INT 21H
              CALL INPUT_4DIGIT_NUM
                 
              pop ax
              add ax,bx 
              mov res,ax
               mov temp,1
              CALL SUCCESS_MSG
              JMP CHOOSE_OPTION
              
             
      do_sub:
                         
              MOV AH, 09H
              LEA DX, num1
              INT 21H
              CALL INPUT_4DIGIT_NUM
                push bx 
                   MOV AH, 09H
              LEA DX, num2
              INT 21H
              CALL INPUT_4DIGIT_NUM
                 
              pop res
              cmp res,bx 
              JL l3
                       
              sub res,bx
              mov temp,1
              JMP l2 
              
              l3:
                   
                   mov temp,0
                  xchg res,bx
                  sub res,bx
                   
                
              l2:
            
              CALL SUCCESS_MSG
              JMP CHOOSE_OPTION 
      do_mul:
                         
              MOV AH, 09H
              LEA DX, num1
              INT 21H
              CALL INPUT_4DIGIT_NUM
              
                push bx
                 MOV AH, 09H
              LEA DX, num2
              INT 21H
              CALL INPUT_4DIGIT_NUM
                 
              pop ax
              cmp ax,0
              JL l5
              cmp bx,0 
              JL l5
              JMP l6
              l5:
              mov temp,0
              l6:
              mov temp,1 
              mul bx
              mov res,ax
               
            
              CALL SUCCESS_MSG
              JMP CHOOSE_OPTION 
        do_div:
                         
                         
              MOV AH, 09H
              LEA DX, num1
              INT 21H
              CALL INPUT_4DIGIT_NUM
              
                push bx
                 MOV AH, 09H
              LEA DX, num2
              INT 21H
              CALL INPUT_4DIGIT_NUM
                 
              pop ax
              cmp ax,0
              JL l7
              cmp bx,0 
              JL l7
              JMP l8
              l7:
              mov temp,0
              l8:
              mov temp,1 
              div bx
              mov res,ax
               
            
              CALL SUCCESS_MSG
              JMP CHOOSE_OPTION 
        do_neg:
                         
              MOV AH, 09H
              LEA DX, num1
              INT 21H
              CALL INPUT_4DIGIT_NUM
              ;neg bx
              mov res,bx
              mov temp,0
             
                
            
              CALL SUCCESS_MSG
              JMP CHOOSE_OPTION      
          
        do_inc:
                         
              
            MOV AH, 09H
              LEA DX, num1
              INT 21H
              CALL INPUT_4DIGIT_NUM
              inc bx
              mov res,bx 
              mov temp,1
             
                
            
              CALL SUCCESS_MSG
              JMP CHOOSE_OPTION      
          
        do_dec:
                         
               MOV AH, 09H
              LEA DX, num1
              INT 21H
              CALL INPUT_4DIGIT_NUM
              dec bx
              mov res,bx
             
                
            
              CALL SUCCESS_MSG
              JMP CHOOSE_OPTION      
                   
  
    ;Exit the application
    EXIT: MOV AH, 09H
          LEA DX, THANK
          INT 21H
          MOV AH, 4CH
          INT 21H
          
    ;Invalid input
    INP_ERROR: MOV AH, 09H
               LEA DX, INVALID
               INT 21H
               JMP CHOOSE_OPTION
               
    ;Procedure to input a 4-digit number
   ;Procedure to input a 4-digit number
     ;Procedure to input a 4-digit number
    INPUT_4DIGIT_NUM PROC
        XOR BX, BX
        MOV CX, 4
	    MOV TH, 1000
        READ_DIGIT: MOV AH, 01H
                    INT 21H
		            CMP AL, 0DH        ; Check if the user pressed Enter
		            JE ENTER_PRESSED   ; Jump if Enter was pressed
                    SUB AL, '0'
                    MOV AH, 0
                    MUL TH
                    ADD BX, AX
		            ;PUSH AX
		            MOV AX, TH
		            DIV T
		            MOV TH, AX       ;th = 100
		            ;POP AX           ;AX = 
                    LOOP READ_DIGIT
	    ENTER_PRESSED:
    	    ; Code to execute when Enter is pressed
    	            MOV AX, BX
    	            CMP CX,0
    	            JE L1
    	LABEL1:      
    	            DIV T
    	            LOOP LABEL1
    	           
        L1:
        MOV BX, AX
    	
        
        RET
    INPUT_4DIGIT_NUM ENDP

    ;Procedure to display a number

     PRINT_NUMBER PROC
    ; Check if the number is negative
    PUSH AX            ; Save AX
    PUSH BX            ; Save BX
    PUSH CX            ; Save CX

    XOR CX, CX         ; Clear CX (used for counting digits)
    MOV BX, 10         ; Base 10 divisor

    CMP temp, 0
    JNE PRINT_POSITIVE
    mov temp,ax ; If AX >= 0, skip the negative handling

    ; Handle negative number
    MOV AH, 02H        ; Print character function
    MOV DL, '-'        ; Set DL to '-' for the negative sign
    INT 21H            ; Print '-'
    NEG AX             ; Convert AX to positive for printing
   mov ax,temp 
PRINT_POSITIVE:
CONVERT_LOOP:
    XOR DX, DX         ; Clear DX
    DIV BX             ; Divide AX by 10, remainder in DX
    PUSH DX            ; Store remainder (digit) on stack
    INC CX             ; Increment digit count
    CMP AX, 0          ; Check if AX is zero
    JNE CONVERT_LOOP   ; Repeat if not

PRINT_DIGITS:
    POP DX             ; Retrieve digit from stack
    ADD DL, '0'        ; Convert to ASCII
    MOV AH, 02H        ; Print character function
    INT 21H            ; Print digit
    LOOP PRINT_DIGITS  ; Repeat for all digits

    POP CX             ; Restore CX
    POP BX             ; Restore BX
    POP AX             ; Restore AX
    RET
PRINT_NUMBER ENDP


    ;Procedure to display a success message
    SUCCESS_MSG PROC
        MOV AH, 09H
        LEA DX, SUCCESS
        INT 21H
        XOR AX, AX
        MOV AX, res
        CALL PRINT_NUMBER
        RET
    SUCCESS_MSG ENDP

END MAIN  


