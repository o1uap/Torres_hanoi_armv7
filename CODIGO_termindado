.global _start
_start:
		
		mov r0, #7  // se guarda el numero de discos en R0
		bl hanoi
		1:b 1b               

hanoi:                      //funcion que soluciona el problema, guarda los registros necesarios y establece valores iniciales y llama a la funcion 'torre inicial'
    push {r1-r3, r7, r9-r12, lr}
	mov r1, #0
	mov r2, #0
	mov r3, #0			
	mov r9, #1			
	mov r10, #3	
	mov r11, #2	
	mov r7, #0			
	ldr r12, =0x1000
 	 
	 bl torreinicial
	 
	 POP {r1-r3, r7, r9-r12, lr}
	 mov pc, lr
	   
torreinicial:                     //Se encarga de establecer el estado inicial de cada torre 
		push {r0,r7,lr}
		ldr r1, =0x12345678
		mov r7, #4
		mvn r0, r0
		add r0, r0, #9
		mul r0, r0, r7
		lsr r1, r1, r0
		pop {r0, r7}
		bl guardar_estado
		pop {lr}
if:                             //funcion que se encarga de ir comparando r0 con 1 si es igual salta funcion then
		cmp r0, #1
		beq then
	
		push {r0, r9-r11, lr}
		sub r0, r0, #1
		add r11, r11, r10
		sub r10, r11, r10
		sub r11, r11, r10
		bl if
		pop {r0, r9-r11, lr}
		
		push {lr}
		cmp r9, #2	
		bl ordenar
		pop {lr}
		
		push {r0, r9-r11, lr}
		sub r0, r0, #1
		add r9, r9, r10
		sub r10, r9, r10
		sub r9, r9, r10
		bl if
		pop {r0, r9-r11, lr}
		mov pc, lr
		
then:                                      //Parte del bucle recursivo if, se ejecuta si el valor de r0 es 1 
	
	    push {lr}
		cmp r9, #2	
		bl ordenar
		pop {lr}
		mov pc, lr
		
ordenar:                          //Se encarga de ordenar los discos 
 
        push {r4, r5, r10}
		movgt r4, r3
		moveq r4, r2
		movlt r4, r1
		ldr r5, =0xffffffff
		mov r10, #0
		push {r4}
loop:                            //sub funcion de ordenar que sirve para contar el numero discos de la torre mas pequeña
	    ands r4, r5
    	lslne r5, #4
	    addne r10, #1
	    bne loop

exit_loop:                    //saca el ultimo valor de r4 y lo compara con r7 para determinar si se debe llamar a la función 'cololcar_disco' o 'ordenar'
        pop {r4}
	    cmp r7, #1
	    beq colocar_disco
	
	    sub r10, #1
	    lsl r10, #2
	    mov r5, #0xf
	    lsl r5, r10
	    mvn r5, r5
	    and r4, r5
	    cmp r9,#2
	    movgt r3, r4
	    moveq r2, r4
	    movlt r1, r4
	    pop {r4, r5, r10}
	    mov r7, #1
	    cmp r11, #2
	    b ordenar
	
colocar_disco:                     //sirve para colocar un disco en la torre correspondiente 
        lsl r10, #2 
        lsl r5, r0, r10
	    orr r4, r5
		cmp r11, #2 
		movgt r3, r4
		moveq r2, r4
		movlt r1, r4
		pop {r4, r5, r10}
		mov r7, #0	
			
guardar_estado:                  //guarda el estado actual de cada torre en cada paso 
		str r1, [r12], #4
		str r2, [r12], #4
		str r3, [r12], #8
	    mov pc, lr
