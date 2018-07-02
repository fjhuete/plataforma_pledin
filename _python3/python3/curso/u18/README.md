---
title: "Ejercicios de repetitivas"
permalink: /cursos/python3/curso/u18/index.html
---

Pedir un número por teclado y mostrar la tabla de multiplicar

[Solución con while](ejercicio1.py)

	#!/usr/bin/env python
	numero = int(input("Número:"))
	cont = 1
	while (cont<11):
		print ("%d * %d = %d" % (cont, numero, cont * numero))
		cont += 1

[Solución con for](ejercicio2.py)

	#!/usr/bin/env python
	numero = int(input("Número:"))
	for cont in range(1,11):
		print ("%2d * %d = %2d" % (cont, numero, cont * numero))

Crea una aplicación que pida un número y calcule su factorial (El factorial de un número es el producto de todos los enteros entre 1 y el propio número y se representa por el número seguido de un signo de exclamación. Por ejemplo 5! = 1x2x3x4x5=120

[Solución](ejercicio3.py)

	#!/usr/bin/env python
	num=int(input("Número:"))
	fact=1
	for i in range(2,num+1):
		fact*=i
	print("El resultado es %d" % fact)

Crea una aplicación que permita adivinar un número. En primer lugar la aplicación solicita un número entero por teclado. A continuación va pidiendo números y va respondiendo si el número a adivinar es mayor o menor que el introducido. El programa termina cuando se acierta el número.

[Solución](ejercicio4.py)

	#!/usr/bin/env python
	secreto=int(input("Número secreto:"))
	num=int(input("Número:"))
	while num!=secreto:
	    if num>secreto:
	        print("El número es menor")
	    else:
	        print("El número es mayor")
	    num=int(input("Número:"))
	print ("Has acertado")

Programa que muestre la tabla de multiplicar de los números 1,2,3,4 y 5.

[Solución](ejercicio5.py)

	#!/usr/bin/env python
	for numero in range(1,6):
		for cont in range(1,11):
			print ("%2d * %d = %2d" % (cont, numero, cont * numero))
		print()

Escribe un programa que diga si un número introducido por teclado es o no primo. Un número primo es aquel que sólo es divisible entre él mismo y la unidad.

[Solución](ejercicio6.py)

	#!/usr/bin/env python
	num=int(input("Número:"))
	primo = True
	for cont in range(2,num):
	    if num%cont==0:
	        primo=False
	        break
	if primo:
	    print("Es primo")
	else:
	    print("No es primo")

