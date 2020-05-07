---
layout: post
title: Generador de nombres
date: 2020-05-07 00:00:00 -0600
categories: pyhton
tags: python
author: Manuel Jamher
---

Mas de una vez he requerido tener un set de datos que cuente con RFC y CURP validos, y aunque muchas veces he buscado soluciones no he encontrado una que me deje satisfecho es por eso que he decido generar  un pequeño codigo que me permita generar "n"  numero de registros que tengan todos los datos que pueda tener una persona, este codigo estara disponible en github y espero mejorar la coherencia de los datos generados y de este modo iniciar uan serie de publicaciones regulares, empecemos

### Establecer los datos que requerimos

Declaramos dos variables una para el sexo con los valores H y M para poder seleccionar si es hombre o mujer, además agregamos un arreglo de los estados de la república y un valor extra por si es extranjero
```python
// Persona.py
    var_sexo = "HM"
    arr_estados = [["AGUASCALIENTES", "AS"],["BAJA CALIFORNIA", "BC"],["BAJA CALIFORNIA SUR", "BS"],["CAMPECHE", "CC"],["COAHUILA", "CL"],["COLIMA", "CM"],["CHIAPAS", "CS"],["CHIHUAHUA", "CH"],["DISTRITO FEDERAL", "DF"],["DURANGO", "DG"],["GUANAJUATO", "GT"],["GUERRERO", "GR"],["HIDALGO", "HG"],["JALISCO", "JC"],["MÉXICO", "MC"],["MICHOACÁN", "MN"],["MORELOS", "MS"],["NAYARIT", "NT"],["NUEVO LEÓN", "NL"],["OAXACA", "OC"],["PUEBLA", "PL"],["QUERÉTARO", "QT"],["QUINTANA ROO", "QR"],["SAN LUIS POTOSÍ", "SP"],["SINALOA", "SL"],["SONORA", "SR"],["TABASCO", "TC"],["TAMAULIPAS", "TS"],["TLAXCALA", "TL"],["VERACRUZ", "VZ"],["YUCATÁN", "YN"],["ZACATECAS", "ZS"],["NACIDO EN EL EXTRANJERO", "NE"]]
```

Buscaremos generar los siguientes datos para las personas

```python
// Persona.py
        self.sexo //Determina si el sexo de la persona aleatoria, basado en la variable var_sexo definido previamente
        intEstado //Deleccionaremos un estado al azar del array arr_estados
        self.estado // Tomaremos el nombre completo del estado escogido
        self.code_estado //Tomaremos la abreviatura del estado escogido
        self.nombre // El nombre de la persona
        self.paterno // El apellido paterno
        self.materno // El apellido materno
        self.yyyy // Año de nacimiento, para ser coherente se elige entre 1965 y 18 años antes del día actual
        self.mm // Mes de nacimiento
        self.dd // Día de nacimiento, por ahora eligimos solo entre el día 1 y 28 de los meses
        self.base // Con los datos previos generamos la parte que usaremos para el curp y el rfc
        self.curp // curp con los datos generados
        self.rfc // rfc con los datos generados
```

El codigo queda de la siguiente manera:
```python
// Persona.py
        self.sexo = random.choice(sexo)
        intEstado = random.randint(1, 32)
        self.estado = estados[intEstado][0]
        self.code_estado = estados[intEstado][1]
        self.nombre = Cadenas.generar(1)
        self.paterno = Cadenas.generar(0)
        self.materno = Cadenas.generar(0)
        self.yyyy = str(random.randint(1965,date.today().year - 18))
        mm = str(random.randint(1, 12))
        if( len(mm) < 2 ):
            mm = "0" + mm
        self.mm = mm
        dd = str(random.randint(1, 28))
        if( len(dd) < 2 ):
            dd = "0" + dd
        self.dd = dd
        self.base = Cadenas.definirBase(self)
        self.curp = Cadenas.crearCurp(self)
        self.rfc = Cadenas.creaRFC(self)

```
 
Ahora definiremos 3 variables, con el fin de darle una mayor naturalidad a los nombres generados, tendremos constantes, vocales y letras que pueden repetirse
```python
// Cadenas.py
	consonantes = "BCDFGHJKLMNPQRSTVWXYZ"
	vocales = "AEIOU"
	repetir = "LRH"
```

Lo primero que debemos de hacer es definir el tamaño de las silabas, de forma aleatoria las silabas pueden ser de 2 o 3 caracteres, tambien definiremos un random entre 0 y 1 para determinar si la silaba comienza con una vocal o una consonante, en caso de ser de 3 letras agregamos una vocal al azar con esto logramos palabras como 'AL' de 'ALBERTO' o 'AR' de 'ARTURO' o bien 'ARA' de 'ARACELY', para la parte de iniciar con una constante se busca algo similar
```python
// Cadenas.py
def silaba(cls,letras = random.randint(2, 3), const = consonantes, vcls = vocales, rpt = repetir):
		if(1 == random.randint(0, 1)):
			silaba = random.choice(vcls) + random.choice(const)
			if(3==letras):
				silaba = silaba + random.choice(vcls)
		else:
			silaba = random.choice(const)
			if(3==letras and 1 == random.randint(0, 1)):
				silaba = silaba + random.choice(rpt) + random.choice(vcls)
			elif(3==letras):
				silaba = silaba + random.choice(vcls) + random.choice(const)
			else:
				silaba = silaba + random.choice(vcls)
		return silaba
```

Ahora generamos palabras la logica es basicamente la misma pero ahora definimos la variable corto, que nos indica si es un nombre o un apellido, si la variable corto es true(1) genera una palabra mas corta, buscando algo como 'MARTIN' o 'MARTINEZ', algo similar en otros idiomas como 'JHON' y 'JHONSON', con la variable de 'limite' controlamos las probabilidades de contener un espacio para nombres como 'MA DE LUZ' y apellidos como 'DE LA CRUZ' 
```python
// Cadenas.py
	def generar(cls,corto):
		cadena_generada = ""
		limite = 20
		if( 1 == corto ):
			limite = 10
		prob_espacio = random.randint(0,limite)

		if( 1 == prob_espacio or 12 == prob_espacio or 18 == prob_espacio):
			cadena_generada = cls.silaba() + " " + cls.silaba() + cls.silaba()
		elif( 6 == prob_espacio or 13 == prob_espacio or 15 == prob_espacio):
			cadena_generada = cls.silaba() + " " + cls.silaba() + cls.silaba() + cls.silaba()
		elif( 1 == corto and random.randint(1,40)%2 == 0):
			cadena_generada = cls.silaba() + cls.silaba()
		elif( ( 1 != corto and random.randint(1,40)%2 == 0) or ( 1 == corto )):
			cadena_generada = cls.silaba() + cls.silaba() + cls.silaba()
		else:
			cadena_generada = cls.silaba() + cls.silaba() + cls.silaba() + cls.silaba()
		return cadena_generada
```

La parte del CURP y RFC nos apegamos a los patrones definidos, por lo que no explicaremos eso en esta ocasión, todo el codigo anterior lo metemos en un clase para ser ejecutar todo y tambien exportamos lo que generamos en csv para poder manipular el set de datos generado
```python
// Consola
C:\>genera_datos.py
Dime cuantos nombres generar: 5
Escribiendo archivo, espere por favor
archivo generado
```
El resultado es el siguiente

```python
// csv Generado
dia,mes,anio,nombres,paterno,materno,sexo,curp,rfc
09,04,1986,BIUKLO,NOREOX,ED IKNEUK,M,NXEB860409MYNRKK59,NXEB860409SL5
25,09,1977,ZOUJ,HIELUJ,ITTOIY,M,HJIZ770925MQTLTJ12,HJIZ770925XR5
07,06,1989,UQEH,EDFIKA,FE IBUCIF,M,EAFU890607MQRFBH60,EAFU890607XD5
02,09,1993,DOWORA,AVWUTO,DU RUUH,M,AODD930902MNEWRW02,AODD930902KT6
28,07,1967,VI EWAN,QOSIUL,MI WUIF,M,QLMV670728MCSSWW07,QLMV670728ST9

```
En proximas publicaciones mejoraremos la coherencia de los nombres, el codigo completo se encuentra en [github](https://github.com/soyjamher/codigo-blog/tree/master/genera-nombres)

Espero les sirva.

<div class="fb-comments" data-href="https://soyjamher.github.io/blog/Generador-de-nombres/" data-numposts="5" data-width=""></div>