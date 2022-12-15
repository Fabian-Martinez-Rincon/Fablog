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

### Indice
- [Like](#like)
- [Solamente durante un año especifico](#solamente-durante-un-año-especifico)



### Like

```sql
SELECT *
FROM Cliente
WHERE (apellido LIKE "%ez")
```



### Solamente durante un año especifico

```sql
SELECT c.nombre, c.apellido, c.DNI, c.telefono, c.direccion
FROM Cliente c 
	INNER JOIN Factura f ON (c.idCliente = f.idCliente)
WHERE (f.fecha BETWEEN "2020-1-1" and "2020-12-31")
```

