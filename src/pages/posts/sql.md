---
description: Conocimientos sobre sql
public: true
layout: ../../layouts/BlogPost.astro
title: Codigos de SQL que utilizo con frecuencia
createdAt: 1663635792232
updatedAt: 1663635814121
tags:
  - sql
heroImage: /posts/sql2.jpg
---

## Para la siguientes tablas

- **Cliente**(idCliente, nombre, apellido, DNI, telefono, direccion)
- **Factura** (nroTicket, total, fecha, hora,idCliente (fk))
- **Detalle**(nroTicket, idProducto, cantidad, preciounitario)
- **Producto**(idProducto, descripcion, precio, nombreP, stock)
- [Web para Practica sql](http://www.sqliteonline.net/)

<br>

Para poder trabajar con los datos de manera más comoda
podemos insertar la base de datos que dejare
en el siguiente links y como resultado nos quedara lo siguiente:

![image](https://user-images.githubusercontent.com/55964635/207398864-73e8d6c0-6306-4d0f-a598-7e10febbee94.png)

<br>

### Indice
- [Insertar Valores en la tabla]()

<br>





### Borrar



Antes de insertar, para tener limpio el trabajo, eliminamos las tablas 
existentes con: 



```sql
DROP TABLE demo;
```

<br>

## Creamos las tablas



```sql
  CREATE TABLE Cliente (
    idCliente INTEGER,
    nombre VARCHAR(255),
    apellido VARCHAR(255),
    DNI VARCHAR(255),
    telefono VARCHAR(255),
    direccion VARCHAR(255)
  );
  CREATE TABLE Factura (
    nroTicket INTEGER,
    total INTEGER,
    fecha DATE,
    hora TIME,
    idCliente INTEGER
  );

  CREATE TABLE Detalle (
    nroTicket INTEGER,
    idProducto INTEGER,
    cantidad INTEGER,
    preciounitario INTEGER
  );

  CREATE TABLE Producto (
    idProducto INTEGER,
    descripcion VARCHAR(255),
    precio INTEGER,
    nombreP VARCHAR(255),
    stock INTEGER
  );

```

<br>

## Insertamos valores en las tablas creadas

```sql
  INSERT INTO Cliente 
  (idCliente, nombre, apellido, DNI, telefono, direccion) 
  VALUES
  (1, 'Juan', 'Pérez', '12345678', '987654321', 'Calle Falsa 123'),
  (2, 'María', 'Gómez', '23456789', '876543219', 'Calle Falsa 456'),
  (3, 'Carlos', 'Rodríguez', '34567890', '765432198', 'Calle Falsa 789'),
  (4, 'Ana', 'Sánchez', '45678901', '654321987', 'Calle Falsa 321'),
  (5, 'Sofía', 'Martínez', '56789012', '543219876', 'Calle Falsa 654')


  INSERT INTO Factura 
  (nroTicket, total, fecha, hora, idCliente) 
  VALUES
  (1, 10.50, '2022-12-13', '12:00:00', 1),
  (2, 20.00, '2022-12-13', '12:30:00', 2),
  (3, 30.75, '2022-12-13', '13:00:00', 3),
  (4, 40.25, '2022-12-13', '13:30:00', 4),
  (5, 50.10, '2022-12-13', '14:00:00', 5)


  INSERT INTO Detalle 
  (nroTicket, idProducto, cantidad, preciounitario) 
  VALUES
  (1, 1, 2, 5.25),
  (1, 2, 3, 3.50),
  (2, 3, 1, 10.00),
  (2, 4, 2, 7.50),
  (3, 5, 3, 4.25),
  (3, 6, 1, 12.00),
  (4, 7, 2, 6.00),
  (4, 8, 1, 9.50),
  (5, 9, 3, 3.75),
  (5, 10, 2, 5.50)


  INSERT INTO Producto 
  (idProducto, descripcion, precio, nombreP, stock) 
  VALUES
  (1, 'Lápiz de grafito', 1.50, 'Lápiz', 100),
  (2, 'Borrador de goma', 0.50, 'Borrador', 200),
  (3, 'Cuaderno de rayas', 3.50, 'Cuaderno', 150),
  (4, 'Pegamento escolar', 2.00, 'Pegamento', 120),
  (5, 'Tijera de plástico', 4.50, 'Tijera', 90),
  (6, 'Marcadores de colores', 6.50, 'Marcadores', 80),
  (7, 'Pintura acrílica', 3.75, 'Pintura', 70),
  (8, 'Lápices de colores', 2.50, 'Lápices de colores', 60),
  (9, 'Sacapuntas', 0.75, 'Sacapuntas', 50),
  (10, 'Grapadora', 5.00, 'Grapadora', 40)
```