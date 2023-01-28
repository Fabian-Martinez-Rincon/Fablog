---
description: Conocimientos sobre sql
public: true
layout: ../../layouts/BlogPost.astro
title: Codigos de SQL que utilizo con frecuencia
createdAt: 1663635792232
updatedAt: 1663635814121
tags:
  - sql
heroImage: /posts/sql3.jpg
---


## Para la siguientes tablas

- **Cliente**(idCliente, nombre, apellido, DNI, telefono, direccion)
- **Factura** (nroTicket, total, fecha, hora,idCliente (fk))
- **Detalle**(nroTicket, idProducto, cantidad, preciounitario)
- **Producto**(idProducto, descripcion, precio, nombreP, stock)

## Links de ayuda

- [Web para Practica sql](http://www.sqliteonline.net/)
- [Insertar valores para empezar](https://github.com/Fabian-Martinez1/Fablog/blob/main/src/pages/posts/files/dbd.md)
- [Base de datos para testear](https://github.com/Fabian-Martinez1/Fablog/blob/main/src/pages/posts/files/sql.json)


Para poder trabajar con los datos de manera más comoda
podemos insertar la base de datos que dejare
en el siguiente [link](https://github.com/Fabian-Martinez1/Fablog/blob/main/src/pages/posts/files/sql.json) y como resultado nos quedara las siguientes tablas:

![image](https://user-images.githubusercontent.com/55964635/207497718-948a1202-5294-4ffc-8ce4-095a06023fdb.png)

<br>

## Practica de SQL

---

## Tabla 1

- **Cliente**(**idCliente**, nombre, apellido, DNI, telefono, direccion)
- **Factura** (**nroTicket**, total, fecha, hora,idCliente (fk))
- **Detalle**(**nroTicket, idProducto**, cantidad, preciounitario)
- **Producto**(**idProducto**, descripcion, precio, nombreP, stock)

## Consignas Ejercicio 1

- [1. Listar datos personales de clientes cuyo apellido comience con el string ‘Pe’. Ordenar por DNI](#ejercicio-11)
- [2. Listar nombre, apellido, DNI, teléfono y dirección de clientes que realizaron compras solamente durante 2017.](#ejercicio-12)
- [3. Listar nombre, descripción, precio y stock de productos vendidos al cliente con DNI:45789456, pero que no fueron vendidos a clientes de apellido ‘Garcia’.](#ejercicio-13)
- [4. Listar nombre, descripción, precio y stock de productos no vendidos a clientes que tengan teléfono con característica: 221 (La característica está al comienzo del teléfono). Ordenar por nombre.](#ejercicio-14)
- [5. Listar para cada producto: nombre, descripción, precio y cuantas veces fué vendido. Tenga en cuenta que puede no haberse vendido nunca el producto.](#ejercicio-15)
- [6. Listar nombre, apellido, DNI, teléfono y dirección de clientes que compraron los productos con nombre ‘prod1’ y ‘prod2’ pero nunca compraron el producto con nombre ‘prod3’.](#ejercicio-16)
- [7. Listar nroTicket, total, fecha, hora y DNI del cliente, de aquellas facturas donde se haya comprado el producto ‘prod38’ o la factura tenga fecha de 2019.](#ejercicio-17)
- [8. Agregar un cliente con los siguientes datos: nombre:’Jorge Luis’, apellido:’Castor’, DNI:40578999, teléfono:221-4400789, dirección:’11 entre 500 y 501 nro:2587’ y el id de cliente: 500002. Se supone que el idCliente 500002 no existe.](#ejercicio-18)
- [9. Listar nroTicket, total, fecha, hora para las facturas del cliente ´Jorge Pérez´ donde no haya comprado el producto ´Z´.](#ejercicio-19)
- [10. Listar DNI, apellido y nombre de clientes donde el monto total comprado, teniendo en cuenta todas sus facturas, supere $10.000.000.](#ejercicio-110)

### Ejercicio 1.1

```sql
SELECT *
    FROM Cliente
WHERE Apellido LIKE "Pe%"
```
### Ejercicio 1.2
```sql
SELECT C.nombre, C.apellido, C.DNI, C.telefono, C.direccion
    FROM Cliente C
    INNER JOIN Factura F ON (C.idCliente = F.idCliente)
WHERE Year(F.fecha) = 2017 
except (
SELECT C.nombre, C.apellido, C.DNI, C.telefono, C.direccion
    FROM Cliente C
    INNER JOIN Factura F ON (C.idCliente = F.idCliente)
WHERE not Year(F.fecha) = 2017
)
```
### Ejercicio 1.3
```sql
SELECT P.nombreP, P.descripcion, P.precio, P.stock
    FROM Producto P
    INNER JOIN Detalle D ON (P.idProducto = D.idProducto)
    INNER JOIN Factura F ON (D.nroTicket = F.nroTicket)
    INNER JOIN Cliente C ON (F.idCliente = C.idCliente)
WHERE C.dni = 45789456 
except (
SELECT P.nombreP, P.descripcion, P.precio, P.stock
    FROM Producto P
    INNER JOIN Detalle D ON (P.idProducto = D.idProducto)
    INNER JOIN Factura F ON (D.nroTicket = F.nroTicket)
    INNER JOIN Cliente C ON (F.idCliente = C.idCliente)
WHERE C.apellido = 'Garcia'
)
```
### Ejercicio 1.4
```sql
SELECT  P.nombreP, P.descripcion, P.precio, P.stock
    FROM Producto P
except (
SELECT  P.nombreP, P.descripcion, P.precio, P.stock
    FROM Producto P
    INNER JOIN Detalle D ON (P.idProducto = D.idProducto)
    INNER JOIN Factura F ON (D.nroTicket = F.nroTicket)
    INNER JOIN Cliente C ON (F.idCliente = C.idCliente)
WHERE C.telefono = '221%'
)
ORDER BY P.nombreP
```
### Ejercicio 1.5
```sql
SELECT P.nombreP, P.descripcion, P.precio, SUM(D.cantidad)
    FROM Producto P
    LEFT JOIN Detalle D ON(P.idProducto = D.idProducto)
WHERE NOT NULL D.cantidad 
```
### Ejercicio 1.6
```sql
(SELECT C.nombre, C.apellido, C.DNI, C.telefono, C.direccion
    FROM Cliente C
    INNER JOIN FACTURA F ON (C.idCliente = F.idCliente)
    INNER JOIN DETALLE D ON (F.nroTicket = D.nroTicket)
    INNER JOIN PRODUCTO P ON (D.idProducto = P.idProducto)
WHERE P.nombreP = 'prod1'
INTERSECT(
SELECT C.nombre, C.apellido, C.DNI, C.telefono, C.direccion
    FROM Cliente C
    INNER JOIN FACTURA F ON (C.idCliente = F.idCliente)
    INNER JOIN DETALLE D ON (F.nroTicket = D.nroTicket)
    INNER JOIN PRODUCTO P ON (D.idProducto = P.idProducto)
WHERE P.nombreP = 'prod2'
))
EXCEPT(
SELECT C.nombre, C.apellido, C.DNI, C.telefono, C.direccion
    FROM Cliente C
    INNER JOIN FACTURA F ON (C.idCliente = F.idCliente)
    INNER JOIN DETALLE D ON (F.nroTicket = D.nroTicket)
    INNER JOIN PRODUCTO P ON (D.idProducto = P.idProducto)
WHERE P.nombreP = 'prod3'
)


```
### Ejercicio 1.7
```sql
SELECT F.nroTicket, F.total, F.fecha, F.hora, C.DNI
    FROM Factura F
    INNER JOIN Cliente C on (F.idCliente = C.idCliente)
    INNER JOIN Detalle D on (F.nroTicket = D.nroTicket)
    INNER JOIN Producto P on (D.idProducto = P.idProducto)
WHERE P.nombreP = 'prod38'
UNION (
SELECT F.nroTicket, F.total, F.fecha, F.hora, C.DNI
    FROM Factura F
    INNER JOIN Cliente C on (F.idCliente = C.idCliente)
    INNER JOIN Detalle D on (F.nroTicket = D.nroTicket)
    INNER JOIN Producto P on (D.idProducto = P.idProducto)
WHERE YEAR(F.fecha) = 2019
)
```
### Ejercicio 1.8
```sql
INSERT
INTO Cliente(
idCliente, nombre, apellido, DNI, telefono, direccion
)VALUES(
500002, 'Jorge Luis', 'Castor', '40578999', '221-4400789', '11 entre 500 y 501 nro:2587'
)

```
### Ejercicio 1.9
```sql
SELECT F.nroTicket, F.total, F.fecha, F.hora
    FROM Factura F
    INNER JOIN Cliente C ON (F.idCliente = C.idCliente)
WHERE C.nombre = 'Jorge Pérez'
EXCEPT (
SELECT F.nroTicket, F.total, F.fecha, F.hora
    FROM Cliente C
    INNER JOIN Factura F ON (C.idCliente = F.idCliente)
    INNER JOIN Detalle D ON (C.idCliente = F.idCliente)
    INNER JOIN Producto P ON (C.idCliente = F.idCliente)
WHERE C.nombre = 'Jorge Pérez'
)
```
### Ejercicio 1.10
```sql
SELECT C.DNI, C.apellido, C.nombre
    FROM Cliente C
    INNER JOIN Factura F ON (C.idCliente = F.idCliente)
GROUP BY C.DNI, C.apellido, C.nombre
HAVING SUM(F.total) > 10000000
```

---

## Tabla 2

- **AGENCIA** (**RAZON_SOCIAL**, dirección, telef, e-mail)
- **CIUDAD** (**CODIGOPOSTAL**, nombreCiudad, añoCreación)
- **CLIENTE** (**DNI**, nombre, apellido, teléfono, dirección)
- **VIAJE**( **FECHA,HORA,DNI**, cpOrigen(fk), cpDestino(fk), razon_social(fk), descripcion)
//cpOrigen y cpDestino corresponden a la ciudades origen y destino del viaje

## Consignas Ejercicio 2

- [1. Listar razón social, dirección y teléfono de agencias que realizaron viajes desde la ciudad de ‘La Plata’ (ciudad origen) y que el cliente tenga apellido ‘Roma’. Ordenar por razón social y luego por teléfono.](#ejercicio-21)
- [2. Listar fecha, hora, datos personales del cliente, ciudad origen y destino de viajes realizados en enero de 2019 donde la descripción del viaje contenga el String ‘demorado’.](#ejercicio-22)
- [3. Reportar información de agencias que realizaron viajes durante 2019 o que tengan dirección de mail que termine con ‘@jmail.com’.](#ejercicio-23)
- [4. Listar datos personales de clientes que viajaron solo con destino a la ciudad de ‘Coronel Brandsen’](#ejercicio-24)
- [5. Informar cantidad de viajes de la agencia con razón social ‘TAXI Y’ realizados a ‘Villa Elisa’.](#ejercicio-25)
- [6. Listar nombre, apellido, dirección y teléfono de clientes que viajaron con todas las agencias.](#ejercicio-26)
- [7. Modificar el cliente con DNI: 38495444 actualizando el teléfono a: 221-4400897.](#ejercicio-27)
- [8. Listar razon_social, dirección y teléfono de la/s agencias que tengan mayor cantidad de viajes realizados.](#ejercicio-28)
- [9. Reportar nombre, apellido, dirección y teléfono de clientes con al menos 10 viajes.](#ejercicio-29)
- [10. Borrar al cliente con DNI 40325692.](#ejercicio-210)

### Ejercicio 2.1
```sql
SELECT A.RAZON_SOCIAL, A.dirección, A.telef
    FROM AGENCIA A
    INNER JOIN VIAJE V ON (A.RAZON_SOCIAL = V.razon_social)
    INNER JOIN CLIENTE C ON (V.DNI = C.DNI)
    INNER JOIN CIUDAD CI ON (V.cpOrigen = CI.CODIGOPOSTAL)
WHERE CI.nombreCiudad = 'La Plata' AND C.apellido = 'Roma'
ORDER BY A.RAZON_SOCIAL, A.telef
```
### Ejercicio 2.2
```sql
SELECT V.FECHA, V.HORA, C.DNI , C.nombre, C.apellido, C.teléfono, C.dirección, ORIGEN.nombreCiudad, DESTINO.nombreCiudad
    FROM VIAJE V
    INNER JOIN CLIENTE C ON (V.DNI = C.DNI)
    INNER JOIN CIUDAD ORIGEN ON (V.cpOrigen = ORIGEN.CODIGOPOSTAL)
    INNER JOIN CIUDAD DESTINO ON (V.cpDestino = DESTINO.CODIGOPOSTAL)
WHERE (V.fecha > 1-1-2019) AND (V.fecha < 1-2-2019) AND (V.descripcion LIKE "%demorado" )
```
### Ejercicio 2.3
```sql
SELECT A.RAZON_SOCIAL , A.dirección, A.telef, A.e-mail
    FROM AGENCIA A
    INNER JOIN VIAJE V ON (A.RAZON_SOCIAL = V.razon_social)
WHERE (YEAR(V.FECHA) = 2019) OR (A.e-mail LIKE '%@jmail.com')
```
### Ejercicio 2.4
```sql
SELECT C.DNI , C.nombre, C.apellido, C.teléfono, C.direccion
    FROM CLIENTE C
    INNER JOIN VIAJE V ON (C.DNI = V.DNI)
    INNER JOIN CIUDAD DESTINO ON (V.cpDestino = DESTINO.CODIGOPOSTAL)
WHERE DESTINO.nombreCiudad = "Coronel Brandsen"
EXCEPT(
SELECT C.DNI , C.nombre, C.apellido, C.teléfono, C.direccion
    FROM CLIENTE C
    INNER JOIN VIAJE V ON (C.DNI = V.DNI)
    INNER JOIN CIUDAD DESTINO ON (V.cpDestino = DESTINO.CODIGOPOSTAL)
WHERE NOT (DESTINO.nombreCiudad = "Coronel Brandsen")
)

```
### Ejercicio 2.5
```sql
SELECT COUNT(*) AS CANTIDAD_VIAJES
    FROM VIAJE V
    INNER JOIN AGENCIA A ON (V.razon_social = A.RAZON_SOCIAL)
WHERE (A.RAZON_SOCIAL = 'TAXI') AND ()
```
### Ejercicio 2.6
```sql

```
### Ejercicio 2.7
```sql

```
### Ejercicio 2.8
```sql

```
### Ejercicio 2.9
```sql

```
### Ejercicio 2.10
```sql

```

---

## Tabla 3

- **Club**=(**codigoClub**, nombre, anioFundacion, codigoCiudad(FK))
- **Ciudad**=(**codigoCiudad**, nombre)
- **Estadio**=(**codigoEstadio**, codigoClub(FK), nombre, direccion)
- **Jugador**=(**DNI**, nombre, apellido, edad, codigoCiudad(FK))
- **ClubJugador**=(**codigoClub**, DNI, desde, hasta)

## Consignas Ejercicio 3

- [1. Reportar nombre y anioFundacion de aquellos clubes de la ciudad de La Plata que no poseen estadio.](#ejercicio-31)
- [2. Listar nombre de los clubes que no hayan tenido ni tengan jugadores de la ciudad de Berisso.](#ejercicio-32)
- [3. Mostrar DNI, nombre y apellido de aquellos jugadores que jugaron o juegan en el club Gimnasia y Esgrima La PLata.](#ejercicio-33)
- [4. Mostrar DNI, nombre y apellido de aquellos jugadores que tengan más de 29 años y hayan jugado o juegan en algún club de la ciudad de Córdoba.](#ejercicio-34)
- [5. Mostrar para cada club, nombre de club y la edad promedio de los jugadores que juegan actualmente en cada uno.](#ejercicio-35)
- [6. Listar para cada jugador: nombre, apellido, edad y cantidad de clubes diferentes en los que jugó. (incluido el actual)](#ejercicio-36)
- [7. Mostrar el nombre de los clubes que nunca hayan tenido jugadores de la ciudad de Mar del Plata.](#ejercicio-37)
- [8. Reportar el nombre y apellido de aquellos jugadores que hayan jugado en todos los clubes.](#ejercicio-38)
- [9. Agregar con codigoClub 1234 el club “Estrella de Berisso” que se fundó en 1921 y que pertenece a la ciudad de Berisso. Puede asumir que el codigoClub 1234 no existe en la tabla Club.](#ejercicio-39)

### Ejercicio 3.1
```sql

```
### Ejercicio 3.2
```sql

```
### Ejercicio 3.3
```sql

```
### Ejercicio 3.4
```sql

```
### Ejercicio 3.5
```sql

```
### Ejercicio 3.6
```sql

```
### Ejercicio 3.7
```sql

```
### Ejercicio 3.8
```sql

```
### Ejercicio 3.9
```sql

```

---


## Tabla 4

- **PERSONA** = (**DNI**, Apellido, Nombre, Fecha_Nacimiento, Estado_Civil, Genero)
- **ALUMNO** = (**DNI**, Legajo, Año_Ingreso)
- **PROFESOR** = (**DNI**, Matricula, Nro_Expediente)
- **TITULO** = (**Cod_Titulo**, Nombre, Descripción)
- **TITULO-PROFESOR** = (**Cod_Titulo, DNI**, Fecha)
- **CURSO** = (**Cod_Curso**, Nombre, Descripción, Fecha_Creacion, Duracion)
- **ALUMNO-CURSO** = (**DNI, Cod_Curso, Año**, Desempeño, Calificación)
- **PROFESOR-CURSO** = (**DNI, Cod_Curso, Fecha_Desde**, Fecha_Hasta)

## Consignas Ejercicio 4

- [1. Listar DNI, legajo y apellido y nombre de todos los alumnos que tegan año ingreso inferior a 2014.](#ejercicio-41)
- [2. Listar DNI, matricula, apellido y nombre de los profesores que dictan cursos que tengan más 100 horas de duración. Ordenar por DNI](#ejercicio-42)
- [3. Listar el DNI, Apellido, Nombre, Género y Fecha de nacimiento de los alumnos inscriptos al curso con nombre “Diseño de Bases de Datos” en 2019.](#ejercicio-43)
- [4. Listar el DNI, Apellido, Nombre y Calificación de aquellos alumnos que obtuvieron una calificación superior a 9 en los cursos que dicta el profesor “Juan Garcia”. Dicho listado deberá estar ordenado por Apellido.](#ejercicio-44)
- [5. Listar el DNI, Apellido, Nombre y Matrícula de aquellos profesores que posean más de 3 títulos. Dicho listado deberá estar ordenado por Apellido y Nombre.](#ejercicio-45)
- [6. Listar el DNI, Apellido, Nombre, Cantidad de horas y Promedio de horas que dicta cada profesor. La cantidad de horas se calcula como la suma de la duración de todos los cursos que dicta.](#ejercicio-46)
- [7. Listar Nombre, Descripción del curso que posea más alumnos inscriptos y del que posea menos alumnos inscriptos durante 2019.](#ejercicio-47)
- [8. Listar el DNI, Apellido, Nombre, Legajo de alumnos que realizaron cursos con nombre conteniendo el string ‘BD’ durante 2018 pero no realizaron ningún curso durante 2019.](#ejercicio-48)
- [9. Agregar un profesor con los datos que prefiera y agregarle el título con código: 25.](#ejercicio-49)
- [10. Modificar el estado civil del alumno cuyo legajo es ‘2020/09’, el nuevo estado civil es divorciado.](#ejercicio-410)
- [11. Dar de baja el alumno con DNI 30568989. Realizar todas las bajas necesarias para no dejar el conjunto de relaciones en estado inconsistente](#ejercicio-411)

### Ejercicio 4.1
```sql

```
### Ejercicio 4.2
```sql

```
### Ejercicio 4.3
```sql

```
### Ejercicio 4.4
```sql

```
### Ejercicio 4.5
```sql

```
### Ejercicio 4.6
```sql

```
### Ejercicio 4.7
```sql

```
### Ejercicio 4.8
```sql

```
### Ejercicio 4.9
```sql

```
### Ejercicio 4.10
```sql

```
### Ejercicio 4.11
```sql

```

---

## Tabla 5

- **Localidad**(**CodigoPostal**, nombreL, descripcion, #habitantes)
- **Arbol**(**nroArbol**, especie, años, calle, nro, codigoPostal(fk))
- **Podador**(**DNI**, nombre, apellido, telefono,fnac,codigoPostalVive(fk))
- **Poda**(**codPoda**,fecha, DNI(fk),nroArbol(fk))


## Consignas Ejercicio 5

- [1. Listar especie, años, calle, nro. y localidad de árboles podados por el podador ‘Juan Perez’ y por el podador ‘Jose Garcia’.](#ejercicio-51)
- [2. Reportar DNI, nombre, apellido, fnac y localidad donde viven podadores que tengan podas durante 2018.](#ejercicio-52)
- [3. Listar especie, años, calle, nro y localidad de árboles que no fueron podados nunca](#ejercicio-53)
- [4. Reportar especie, años,calle, nro y localidad de árboles que fueron podados durante 2017 y no fueron podados durante 2018.](#ejercicio-54)
- [5. Reportar DNI, nombre, apellido, fnac y localidad donde viven podadores con apellido terminado con el string ‘ata’ y que el podador tenga al menos una poda durante 2018. Ordenar por apellido y nombre.](#ejercicio-55)
- [6. Listar DNI, apellido, nombre, teléfono y fecha de nacimiento de podadores que solo podaron árboles de especie ‘Coníferas’.](#ejercicio-56)
- [7. Listar especie de árboles que se encuentren en la localidad de ‘La Plata’ y también en la localidad de ‘Salta’.](#ejercicio-57)
- [8. Eliminar el podador con DNI: 22234566.](#ejercicio-58)
- [9. Reportar nombre, descripción y cantidad de habitantes de localidades que tengan menos de 100 árboles.](#ejercicio-59)

### Ejercicio 5.1
```sql

```
### Ejercicio 5.2
```sql

```
### Ejercicio 5.3
```sql

```
### Ejercicio 5.4
```sql

```
### Ejercicio 5.5
```sql

```
### Ejercicio 5.6
```sql

```
### Ejercicio 5.7
```sql

```
### Ejercicio 5.8
```sql

```
### Ejercicio 5.9
```sql

```

---

## Tabla 6

- **Técnico** (**codTec**, nombre, especialidad) // técnicos
- **Repuesto** (**codRep**, nombre, stock, precio) // repuestos
- **RepuestoReparacion** (**nroReparac, codRep**, cantidad, precio) //repuestos utilizados en reparaciones.
- **Reparación** (**nroReparac**, codTec, precio_total, fecha) //reparaciones realizadas

## Consignas Ejercicio 6

- [1. Listar todos los repuestos, informando el nombre, stock y precio. Ordenar el resultado por precio.](#ejercicio-61)
- [2. Listar nombre, stock, precio de repuesto que participaron en reparaciones durante 2019 y además no participaron en reparaciones del técnico ‘José Gonzalez’.](#ejercicio-62)
- [3. Listar el nombre, especialidad de técnicos que no participaron en ninguna reparación. Ordenar por nombre ascendentemente.](#ejercicio-63)
- [4. Listar el nombre, especialidad de técnicos solo participaron en reparaciones durante 2018.](#ejercicio-64)
- [5. Listar para cada repuesto nombre, stock y cantidad de técnicos distintos que lo utilizaron. Si un repuesto no participó en alguna reparación igual debe aparecer en dicho listado.](#ejercicio-65)
- [6. Listar nombre y especialidad del técnico con mayor cantidad de reparaciones realizadas y el técnico con menor cantidad de reparaciones.](#ejercicio-66)
- [7. Listar nombre, stock y precio de todos los repuestos con stock mayor a 0 y que dicho repuesto no haya estado en reparaciones con precio_total superior a 10000.](#ejercicio-67)
- [8. Proyectar precio, fecha y precio total de aquellas reparaciones donde se utilizó algún repuesto con precio en el momento de la reparación mayor a $1000 y menor a $5000.](#ejercicio-68)
- [9. Listar nombre, stock y precio de repuestos que hayan sido utilizados en todas las reparaciones](#ejercicio-69)
- [10. Listar fecha, técnico y precio total de aquellas reparaciones que necesitaron al menos 10 repuestos distintos.](#ejercicio-610)

### Ejercicio 6.1
```sql

```
### Ejercicio 6.2
```sql

```
### Ejercicio 6.3
```sql

```
### Ejercicio 6.4
```sql

```
### Ejercicio 6.5
```sql

```
### Ejercicio 6.6
```sql

```
### Ejercicio 6.7
```sql

```
### Ejercicio 6.8
```sql

```
### Ejercicio 6.9
```sql

```
### Ejercicio 6.10
```sql

```


## Tabla 7
- **Banda**(**codigoB**, nombreBanda, genero_musical, año_creacion)
- **Integrante**(**DNI**, nombre, apellido,dirección,email, fecha_nacimiento,codigoB(fk))
- **Escenario**(**nroEscenario**, nombre _ escenario, ubicación,cubierto, m2, descripción)
- **Recital**(**fecha,hora,nroEscenario**, codigoB (fk))

## Consignas Ejercicio 7

- [1. Listar DNI, nombre, apellido,dirección y email de integrantes nacidos entre 1980 y 1990 y hayan realizado algún recital durante 2018.](#ejercicio-71)
- [2. Reportar nombre, género musical y año de creación de bandas que hayan realizado recitales durante 2018, pero no hayan tocado durante 2017 .](#ejercicio-72)
- [3. Listar el cronograma de recitales del dia 04/12/2018. Se deberá listar: nombre de la banda que ejecutará el recital, fecha, hora, y el nombre y ubicación del escenario correspondiente.](#ejercicio-73)
- [4. Listar DNI, nombre, apellido,email de integrantes que hayan tocado en el escenario con nombre ‘Gustavo Cerati’ y en el escenario con nombre ‘Carlos Gardel’.](#ejercicio-74)
- [5. Reportar nombre, género musical y año de creación de bandas que tengan más de 8 integrantes.](#ejercicio-75)
- [6. Listar nombre de escenario, ubicación y descripción de escenarios que solo tuvieron recitales con género musical rock and roll. Ordenar por nombre de escenario](#ejercicio-76)
- [7. Listar nombre, género musical y año de creación de bandas que hayan realizado recitales en escenarios cubiertos durante 2018.// cubierto es true, false según corresponda](#ejercicio-77)
- [8. Reportar para cada escenario, nombre del escenario y cantidad de recitales durante 2018.](#ejercicio-78)
- [9. Modificar el nombre de la banda ‘Mempis la Blusera’ a: ‘Memphis la Blusera’.](#ejercicio-79)

### Ejercicio 7.1
```sql

```
### Ejercicio 7.2
```sql

```
### Ejercicio 7.3
```sql

```
### Ejercicio 7.4
```sql

```
### Ejercicio 7.5
```sql

```
### Ejercicio 7.6
```sql

```
### Ejercicio 7.7
```sql

```
### Ejercicio 7.8
```sql

```
### Ejercicio 7.9
```sql

```

---

## Tabla 8

- **Equipo**(**codigoE**, nombreE, descripcionE)
- **Integrante** (**DNI**, nombre, apellido,ciudad,email, telefono,codigoE(fk))
- **Laguna**(**nroLaguna**, nombreL, ubicación,extension, descripción)
- **TorneoPesca**(**codTorneo**, fecha,hora,nroLaguna(fk), descripcion)
- **Inscripcion**(**codTorneo,codigoE**,asistio, gano)// asistio y gano son true o false según corresponda

## Consignas Ejercicio 8

- [1. Listar DNI, nombre, apellido y email de integrantes que sean de la ciudad ‘La Plata’ y estén inscriptos en torneos a disputarse durante 2019.](#ejercicio-81)
- [2. Reportar nombre y descripción de equipos que solo se hayan inscripto en torneos de 2018.](#ejercicio-82)
- [3. Listar DNI, nombre, apellido,email y ciudad de integrantes que asistieron a torneos en la laguna con nombre ‘La Salada, Coronel Granada’ y su equipo no tenga inscripciones a torneos a disputarse en 2019.](#ejercicio-83)
- [4. Reportar nombre, y descripción de equipos que tengan al menos 5 integrantes.Ordenar por nombre y descripción.](#ejercicio-84)
- [5. Reportar nombre, y descripción de equipos que tengan inscripciones en todas las lagunas.](#ejercicio-85)
- [6. Eliminar el equipo con código:10000.](#ejercicio-86)
- [7. Listar nombreL, ubicación,extensión y descripción de lagunas que no tuvieron torneos.](#ejercicio-87)
- [8. Reportar nombre, y descripción de equipos que tengan inscripciones a torneos a disputarse durante 2019, pero no tienen inscripciones a torneos de 2018.](#ejercicio-88)
- [9. Listar DNI, nombre, apellido, ciudad y email de integrantes que ganaron algún torneo que se disputó en la laguna con nombre: ‘Laguna de Chascomús’.](#ejercicio-89)

### Ejercicio 8.1
```sql

```
### Ejercicio 8.2
```sql

```
### Ejercicio 8.3
```sql

```
### Ejercicio 8.4
```sql

```
### Ejercicio 8.5
```sql

```
### Ejercicio 8.6
```sql

```
### Ejercicio 8.7
```sql

```
### Ejercicio 8.8
```sql

```
### Ejercicio 8.9
```sql

```

---

## Tabla 9

- **Proyecto**(**codProyecto**, nombrP,descripcion, fechaInicioP, fechaFinP, fechaFinEstimada,DNIResponsable, equipoBackend, equipoFrontend) //DNIResponsable corresponde a un empleado, equipoBackend y equipoFrontend corresponden a un equipo
- **Equipo**(**codEquipo**, nombreE, descripcionTecnologias,DNILider)//DNILider corresponde a un empleado
- **Empleado**(**DNI**,nombre, apellido, telefono, direccion, fechaIngreso)
- **Empleado_Equipo**(**codEquipo ,DNI, fechaInicio**, fechaFin,descripcionRol)

## Consignas Ejercicio 9

- [1. Listar nombre, descripción, fecha de inicio y fecha de fin de proyectos ya finalizados que no fueron terminados antes de la fecha de fin estimada.](#ejercicio-91)
- [2. Listar DNI, nombre, apellido, telefono, dirección y fecha de ingreso de empleados que no son, ni fueron responsables de proyectos. Ordenar por apellido y nombre.](#ejercicio-92)
- [3. Listar DNI, nombre, apellido, teléfono y dirección de líderes de equipo que tenga más de un equipo a cargo.](#ejercicio-93)
- [4. Listar DNI, nombre, apellido, teléfono y dirección de todos los empleados que trabajan en el proyecto con nombre ‘Proyecto X’. No es necesario informar responsable y líderes.](#ejercicio-94)
- [5. Listar nombre de equipo y datos personales de líderes de equipos que no tengan empleados asignados y trabajen con tecnología ‘Java’.](#ejercicio-95)
- [6. Modificar nombre, apellido y dirección del empleado con DNI: 40568965 con los datos que desee.](#ejercicio-96)
- [7. Listar DNI, nombre, apellido, teléfono y dirección de empleados que son responsables de proyectos pero no han sido líderes de equipo.](#ejercicio-97)
- [8. Listar nombre de equipo y descripción de tecnologías de equipos que hayan sido asignados como equipos frontend y backend.](#ejercicio-98)
- [9. Listar nombre, descripción, fecha de inicio, nombre y apellido de responsables de proyectos a finalizar durante 2019.](#ejercicio-99)

### Ejercicio 9.1
```sql

```
### Ejercicio 9.2
```sql

```
### Ejercicio 9.3
```sql

```
### Ejercicio 9.4
```sql

```
### Ejercicio 9.5
```sql

```
### Ejercicio 9.6
```sql

```
### Ejercicio 9.7
```sql

```
### Ejercicio 9.8
```sql

```
### Ejercicio 9.9
```sql

```

---

## Tabla 10

- **Vehiculo** = (**patente**, modelo, marca, peso, km)
- **Camion** = (**patente**, largo, max_toneladas, cant_ruedas, tiene_acoplado)
- **Auto** = (**patente**, es_electrico, tipo_motor)
- **Service** = (**fecha, patente**, km_service, observaciones, monto)
- **Parte** = (**cod_parte**, nombre, precio_parte)
- **Service_Parte** = (**fecha, patente, cod_parte**, precio)

## Consignas Ejercicio 10

- [1. Listar todos los datos de aquellos camiones que tengan entre 4 y 8 ruedas, y que hayan realizado algún service en los últimos 365 días. Ordenar por patente, modelo y marca.](#ejercicio-101)
- [2. Listar los autos que hayan realizado el service “cambio de aceite” antes de los 13.000 km o hayan realizado el service “inspección general” que incluya la parte “filtro de combustible”.](#ejercicio-102)
- [3. Listar nombre y precio de todas las partes que aparezcan en más de 30 service que hayan salido (partes) más de $4.000.](#ejercicio-103)
- [4. Dar de baja todos los camiones con más de 250.000 km.](#ejercicio-104)
- [5. Listar el nombre y precio de aquellas partes que figuren en todos los service realizados en el corriente año](#ejercicio-105)
- [6. Listar todos los autos cuyo tipo de motor sea eléctrico. Mostrar información de patente, modelo , marca y peso.](#ejercicio-106)
- [7. Dar de alta una parte, cuyo nombre sea “Aleron” y precio $&400.](#ejercicio-107)
- [8. Dar de baja todos los services que se realizaron al auto con patente ‘AWA564’.](#ejercicio-108)
- [9. Listar todos los vehículos que hayan tenido services durante el 2018.](#ejercicio-109)

### Ejercicio 10.1
```sql

```
### Ejercicio 10.2
```sql

```
### Ejercicio 10.3
```sql

```
### Ejercicio 10.4
```sql

```
### Ejercicio 10.5
```sql

```
### Ejercicio 10.6
```sql

```
### Ejercicio 10.7
```sql

```
### Ejercicio 10.8
```sql

```
### Ejercicio 10.9
```sql

```

---

## Tabla 11

- **Box** = (**nroBox**,m2, ubicación, capacidad, ocupacion) //ocupación es un numérico indicando cantidad de mascotas en el box actualmente, capacidad es una descripción.
- **Mascota** = (**codMascota**,nombre, edad, raza, peso, telefonoContacto)
- **Veterinario** = (**matricula**, CUIT, nombYAp, direccion, telefono)
- **Supervision** = (**codMascota,nroBox, fechaEntra**, fechaSale?, matricula(fk), descripcionEstadia) //fechaSale tiene valor null si la mascota está actualmente en el box

## Consignas Ejercicio 11

- [1. Listar para cada veterinario cantidad de supervisiones realizadas con fecha de salida (fechaSale) durante enero de 2020. Indicar matricula, CUIT, nombre y apellido, dirección, teléfono y cantidad de supervisiones.](#ejercicio-111)
- [2. Listar CUIT, matricula, nombre, apellido,dirección y teléfono de veterinarios que no tengan mascotas bajo supervisión actualmente.](#ejercicio-112)
- [3. Listar nombre, edad, raza, peso y teléfono de contacto de mascotas fueron atendidas por el veterinario ‘Oscar Lopez’. Ordenar por nombre y raza de manera ascendente.](#ejercicio-113)
- [4. Modificar nombre y apellido al veterinario con matricula: ‘MP 10000’, deberá llamarse: ‘Pablo Lopez’.](#ejercicio-114)
- [5. Listar nombre, edad, raza, peso de mascotas que tengan supervisiones con el veterinario con matricula : ‘MP 1000’ y con el veterinario con matricula: ‘MN 4545’.](#ejercicio-115)
- [6. Listar nroBox, m2, ubicación, capacidad y nombre de mascota para supervisiones con fecha de entrada (fechaEntra) durante 2020.](#ejercicio-116)

### Ejercicio 11.1
```sql

```
### Ejercicio 11.2
```sql

```
### Ejercicio 11.3
```sql

```
### Ejercicio 11.4
```sql

```
### Ejercicio 11.5
```sql

```
### Ejercicio 11.6
```sql

```

---

## Tabla 12

Modelo Físico
- **Barberia** = (**codBarberia**, razon_social, direccion, telefono)
- **Cliente** = (**nroCliente**,DNI, nombYAp, direccionC, fechaNacimiento, celular)
- **Barbero** = (**codEmpleado**,DNIB, nombYApB, direccionB, telefonoContacto, mail)
- **Atencion** = (**codEmpleado ,Fecha,hora**,codBarberia(fk), nroCliente(fk),descTratamiento, valor)

## Consignas Ejercicio 12

- [1. Listar DNI, nombYAp, direccionC, fechaNacimiento y celular de clientes que no tengan atención durante 2020.](#ejercicio-121)
- [2. Listar para cada barbero cantidad de atenciones que realizaron durante 2018. Listar DNIB, nombYApB, direccionB, telefonoContacto, mail y cantidad de atenciones.](#ejercicio-122)
- [3. Listar razón social, dirección y teléfono de barberias que tengan atenciones para el cliente con DNI:22283566 . Ordenar por razón social y dirección ascendente.](#ejercicio-123)
- [4. Listar DNIB, nombYApB, direccionB, telefonoContacto y mail de barberos que tengan atenciones con valor superior a 5000.](#ejercicio-124)
- [5. Listar DNI, nombYAp, direccionC, fechaNacimiento y celular de clientes que tengan atenciones en la barbería con razón social: ‘Corta barba’ y también se hayan atendido en la barbería con razón social: ‘Barberia Barbara’.](#ejercicio-125)
- [6. Eliminar el cliente con DNI: 22222222.](#ejercicio-126)



### Ejercicio 12.1
```sql

```
### Ejercicio 12.2
```sql

```
### Ejercicio 12.3
```sql

```
### Ejercicio 12.4
```sql

```
### Ejercicio 12.5
```sql

```
### Ejercicio 12.6
```sql

```

---

## Tabla 13

Modelo Físico
- **Club**(**IdClub**,nombreClub,ciudad)
- **Complejo**(**IdComplejo**,nombreComplejo, IdClub(fk))
- **Cancha**(**IdCancha**,nombreCancha,IdComplejo(fk))
- **Entrenador**(**IdEntrenador**, nombreEntrenador,fechaNacimiento, direccion)
- **Entrenamiento**(**IdEntrenamiento**, fecha, IdEntrenador(fk), IdCancha(fk))


## Consignas Ejercicio 13

- [1- Listar nombre, fecha nacimiento y dirección de entrenadores que hayan tenido entrenamientos durante 2020.](#ejercicio-131)
- [2- Listar para cada cancha del complejo “Complejo 1” , la cantidad de entrenamientos que se realizaron durante el 2019. Informar nombre de la cancha y cantidad de entrenamientos.](#ejercicio-132)
- [3- Listar los complejos donde haya realizado entrenamientos el entrenador “Jorge Gonzalez”. Informar nombre de complejo, ordenar el resultado de manera ascendente.](#ejercicio-133)
- [4- Listar nombre , fecha de nacimiento y dirección de entrenadores que hayan entrenado en la cancha “Cancha 1” y en la Cancha “Cancha 2”.](#ejercicio-134)
- [5- Listar todos los clubes en los que entrena el entrenador “Marcos Perez”. Informar nombre del club y ciudad.](#ejercicio-135)
- [6- Eliminar los entrenamientos del entrenador ‘Juan Perez’](#ejercicio-136)

### Ejercicio 13.1
```sql

```
### Ejercicio 13.2
```sql

```
### Ejercicio 13.3
```sql

```
### Ejercicio 13.4
```sql

```
### Ejercicio 13.5
```sql

```
### Ejercicio 13.6
```sql

```

---

## Tabla 14

Modelo Físico
- **Cine** (**idCine**, nombreC, direccion)
- **Sala** (**nroSala**, nombreS, descripción, capacidad,idCine(fk))
- **Pelicula** (**idPeli**, nombre, descripción, genero)
- **Funcion** (**nroFuncion**, nroSala(fk), idPeli(fk), fecha, hora, ocupación)//ocupación indica cantidad de espectadores de la función

## Consignas Ejercicio 14

- [1- Listar nombre, descripción y género de películas con funciones durante 2020.](#ejercicio-141)
- [2- Listar para cada Sala cantidad de espectadores que asistieron durante 2020. Indicar nombre de la sala, nombre del cine y total de espectadores.](#ejercicio-142)
- [3- Listar nombre de cine y dirección para los cines que tienen o tuvieron función para la película ‘Relic’. Ordenar por nombre de Cine y dirección desc.](#ejercicio-143)
- [4- Listar nombre ,descripción y género de películas que tienen función en la sala con nombre ‘Sala Lola Membrives’ o tienen función en el cine con nombre ‘Gran Rex’.](#ejercicio-144)
- [5- Listar nombre del cine y dirección de cines que tengan salas con capacidad superior a los 300 espectadores.](#ejercicio-145)
- [6- Agregar un cine con nombre cine ‘Cine Ricardo Darin’, dirección: ‘calle 2 nro 1900, La Plata’ e idCine: 5000, asuma que no existe dicho id.](#ejercicio-146)


### Ejercicio 14.1
```sql

```
### Ejercicio 14.2
```sql

```
### Ejercicio 14.3
```sql

```
### Ejercicio 14.4
```sql

```
### Ejercicio 14.5
```sql

```
### Ejercicio 14.6
```sql

```


