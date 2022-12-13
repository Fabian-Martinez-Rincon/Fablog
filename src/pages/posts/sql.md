---
description: Conocimientos sobre sql
public: true
layout: ../../layouts/BlogPost.astro
title: Codigos de SQL que utilizo con frecuencia
createdAt: 1663635792232
updatedAt: 1663635814121
tags:
  - sql
heroImage: /posts/sql.jpg
---

- [Web para Practica sql](https://sqliteonline.com/)

### Indice

Cliente(idCliente, nombre, apellido, DNI, telefono, direccion)
Factura (nroTicket, total, fecha, hora,idCliente (fk))
Detalle(nroTicket, idProducto, cantidad, preciounitario)
Producto(idProducto, descripcion, precio, nombreP, stock)

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
   idCliente (fk) INTEGER
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