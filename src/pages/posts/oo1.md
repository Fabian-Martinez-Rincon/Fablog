---
description: Parciales de Objetos
public: true
layout: ../../layouts/BlogPost.astro
title: Programación Orientada a Objetos
createdAt: 1663138523826
updatedAt: 1663138544071
tags:
  - oo1
heroImage: /posts/oo3.jpg
---


## `Parcial Primera Fecha`

Nos contratan de la agencia de recaudación de la provincia de Buenos Aires para hacer un sistema para el cálculo del impuesto que deben pagar los contribuyentes.

El sistoma ofrece la siguiente funcionalidad:

**Dar de alta un contibuyente**: Se provee nombre, dni, email y localidad. El sistema da de alta al contribuyente y lo retorna. El contribuyente no tiene ningún bien a su nombre.

**Dar do alta un Inmueble**: Se provee el número de partida, el valor del lote, el valor de la edificación, y el contribuyente (propietario). El sistema da de alta el inmueble y lo retorna.

**Dar de alta un automotor:** Se provee patente, marca, modelo, fecha de fabricación, valor y el contribuyente (propietario). El sistema da de alta al automotor y lo retorna.

**Dar de alta una embarcación:** se provee patente, nombre, fecha de fabricación, valor y el contribuyente
(propietario). El sistema da de alta la embarcación y la retorna.

**Calcular el impuesto que debe pagar un contribuyente:** dado un contribuyente, se debe calcular cuánto debe pagar de impuestos, según la siguiente especificación:

- Por cada inmueble que posea, el contribuyente debe pagar un 1% del valor del mismo, que se calcula sumando el valor del lote y el valor de la edificación.
- Por cada uno de los otros bienes, se debe pagar un porcentaje del valor de los mismos, en función de su fecha de fabricación.
-  Encaso de **superar los 10 años**, no deben pagar nada.
- En caso contrario, el porcentaje para un automotor es el **5%** mientras que para una embarcación ese porcentaje varía según el valor de la misma. Si éste es menor a **1 millón**, es el **10%**, caso contrario, el **15%**.



**Contribuyentes que más pagan de una localidad**: Dada una localidad y un número N, se debe retornar los N contribuyentes de la localidad recibida que más deben pagar por sus bienes.

Su tarea es diseñar y programar en Java lo que sea necesario para ofrecer la funcionalidad antes descrita. Se espera que entregue lo siguiente:

- Diseño de su solución en un diagrama de clases UML.
- Implementación en Java de la funcionalidad requerida.
- Implemente los tests necesarios para **la funcionalidad de calcular el impuesto**, justificando su elección en base a valores de borde y particiones equivalentes. Considere contribuyentes que solamente pueden ser embarcaciones.



### Notas
- Para calcular los años entre dos fechas puede utilizar la siguiente expresión
  ```java
  ChronoUnit.YEARS.between(fechat, fecha2);
  ```
-   Donde la fecha1 es anterior a fecha2. La expresión retorna la cantidad de años entre ambas fechas.
- Implemente todos los constructores que considere necesarios
- Puede implementar un getter y un setter, y asumir a existencia del resto.

--- 

## Objetos

- [Sistema](#sistema)
- [Contribuyente](#contribuyente)
- [Propiedad \<interface>](#propiedad)
- [Vehiculo \<abstract>](#vehiculo)
- [Inmueble](#inmueble)

---

### Sistema

```java
public class Sistema(){
  private List<Contribuyente> contribuyentes;

  public Sistema(){
    this.contribuyentes = new ArrayList();
  }

  public Contribuyente agregarContribuyente(String nombre, String localidad, String email, int dni){
    Contribuyente nuevo = new Contribuyentes(nombre, localidad, email, dni);
    contribuyentes.add(nuevo);
    return nuevo;
  }

  public Inmueble agregarInmueble(Contribuyente c, int nroPartida, double valorLote, double valorEdificacion){
    Inmueble nuevo = new Inmueble(nroPartida, valorLote, valorEdificacion);
    c.agregarPropiedad(nuevo);
    return nuevo;
  }

  public Automotor agregarAutomotor(Contribuyente c, String patente, LocalDate fechaFabricacion, double valor, String marca, String modelo){
    Automotor nuevo = nuevo new Automotor(patente, fechaFabricacion, valor, marca, modelo);
    c.agregarPropiedad(nuevo);
    return nuevo;
  }

  public Embarcacion agregarEmbarcacion(Contribuyente c, String patente, LocalDate fechaFabricacion, double valor, String nombre){
    Embarcacion nuevo = new Embarcacion(patente, fechaFabricacion, valor, nombre);
    c.agregarPropiedad(nuevo);
    return nuevo;
  }

  public List<Contribuyente> masPagan (String localidad, int N){
    return contribuyentes.stream().
      filter(c->c.viveEn(localidad))
      .sorted((ex1, ex2) -> Double.compare(ex2.calcularImpuesto(), ex1.calcularImpuestos()))
      .limit(N)
      .collect(Collectors.toList());
    //Se ordena de mayor a menor
  }

  public double calcularImpuesto(Contribuyente c){
    return c.calcularImpuesto();
  }

}
```

### Contribuyente
### Propiedad
### Vehiculo

```java
public abstract class Vehiculo implements Propiedad{
  private String patente;
  private LocalDate fechaFabricacion;
  private double valor;
  
  public Vehiculo(String patente, LocalDate fechaFabricacion, double valor){
    this.patente=patente;
    this.fechaFabricacion=fechaFabricacion;
    this.valor=valor;
  }

  public double getValor(){
    return valor;
  }

  abstract public double getPorcentaje();

  public int getAntiguedad(){
    return (int) ChronoUnit.YEARS.between(fechaFabricaciom, LocalDate.now());
  }
  
  public double calcularImpuesto(){
    if (getAntiguedad() > 10){
      return 0;
    }
    return getPorcentaje() * valor;
  }
}
```

### Embarcacion
### Automotor
### Inmueble

