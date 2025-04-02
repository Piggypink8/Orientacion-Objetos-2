public class Pedido {
    private Cliente cliente;
    private List<Producto> productos;
    private String formaPago;

    public Pedido(Cliente cliente, List<Producto> productos, String formaPago) {
        if (!"efectivo".equals(formaPago)
                && !"6 cuotas".equals(formaPago)
                && !"12 cuotas".equals(formaPago)) {
            throw new Error("Forma de pago incorrecta");
        }
        this.cliente = cliente;
        this.productos = productos;
        this.formaPago = formaPago;
    }

    public double getCostoTotal() {
        double costoProductos = 0;	// Replace Loop with Pipeline
        for (Producto producto : this.productos) { // Replace Loop with Pipeline
            costoProductos += producto.getPrecio(); // Replace Loop with Pipeline
        }
        double extraFormaPago = 0;
        if ("efectivo".equals(this.formaPago)) {
            extraFormaPago = 0;
        } else if ("6 cuotas".equals(this.formaPago)) {
            extraFormaPago = costoProductos * 0.2;
        } else if ("12 cuotas".equals(this.formaPago)) {
            extraFormaPago = costoProductos * 0.5;
        }
        int añosDesdeFechaAlta = Period.between(this.cliente.getFechaAlta(),
                LocalDate.now()).getYears();
        
        // Aplicar descuento del 10% si el cliente tiene más de 5 años de antigüedad
        if (añosDesdeFechaAlta > 5) {
            return (costoProductos + extraFormaPago) * 0.9;
        }
        return costoProductos + extraFormaPago;
    }
}

public class Cliente {
    private LocalDate fechaAlta;
    
    public LocalDate getFechaAlta() {
        return this.fechaAlta;
    }
}

public class Producto {
    private double precio;
    
    public double getPrecio() {
        return this.precio;
    }
}

## Refactoring: Replace Loop with Pipeline

public class Pedido {
    private Cliente cliente;
    private List<Producto> productos;
    private String formaPago;

    public Pedido(Cliente cliente, List<Producto> productos, String formaPago) {
        if (!"efectivo".equals(formaPago)
                && !"6 cuotas".equals(formaPago)
                && !"12 cuotas".equals(formaPago)) {
            throw new Error("Forma de pago incorrecta");
        }
        this.cliente = cliente;
        this.productos = productos;
        this.formaPago = formaPago;
    }

    public double getCostoTotal() {
        double costoProductos = this.productos.stream().mapToDouble(producto -> producto.getPrecio()).sum();
        
        double extraFormaPago = 0;	// Replace Conditional with Polymorphism
        if ("efectivo".equals(this.formaPago)) {
            extraFormaPago = 0;
        } else if ("6 cuotas".equals(this.formaPago)) {
            extraFormaPago = costoProductos * 0.2;
        } else if ("12 cuotas".equals(this.formaPago)) {
            extraFormaPago = costoProductos * 0.5;
        } // Replace Conditional with Polymorphism
        int añosDesdeFechaAlta = Period.between(this.cliente.getFechaAlta(),
                LocalDate.now()).getYears();
        
        // Aplicar descuento del 10% si el cliente tiene más de 5 años de antigüedad
        if (añosDesdeFechaAlta > 5) {
            return (costoProductos + extraFormaPago) * 0.9;
        }
        return costoProductos + extraFormaPago;
    }
}

public class Cliente {
    private LocalDate fechaAlta;
    
    public LocalDate getFechaAlta() {
        return this.fechaAlta;
    }
}

public class Producto {
    private double precio;
    
    public double getPrecio() {
        return this.precio;
    }
}

## Refactoring: Replace Conditional with Polymorphism

public class Pedido {
    private Cliente cliente;
    private List<Producto> productos;
    private FormaPago formaPago;

    public Pedido(Cliente cliente, List<Producto> productos, FormaPago formaPago) {
        if (!"efectivo".equals(formaPago)
                && !"6 cuotas".equals(formaPago)
                && !"12 cuotas".equals(formaPago)) {
            throw new Error("Forma de pago incorrecta");
        }
        this.cliente = cliente;
        this.productos = productos;
        this.formaPago = formaPago;
    }

    public double getCostoTotal() {
        double costoProductos = this.productos.stream().mapToDouble(producto -> producto.getPrecio()).sum();
        
        double extraFormaPago = costoProductos * this.FormaPago.calcularFormaPago();

        int añosDesdeFechaAlta = Period.between(this.cliente.getFechaAlta(),	// Move Method, Extract Method
                LocalDate.now()).getYears();
        
        // Aplicar descuento del 10% si el cliente tiene más de 5 años de antigüedad
        if (añosDesdeFechaAlta > 5) {
            return (costoProductos + extraFormaPago) * 0.9;
        }
        return costoProductos + extraFormaPago;
    }
}

public Interface FormaPago{
	public double obtenerExtra();
}

public class Efectivo implements FormaPago{
	public double obtenerExtra(){
  	return 0;
  }
}
public class SeisCuotas implements FormaPago{
	public double obtenerExtra(){
  	return 0.2;
  }
}
public class DoceCuotas implements FormaPago{
	public double obtenerExtra(){
  	return  0.5;
  }
}

public class Cliente {
    private LocalDate fechaAlta;
    
    public LocalDate getFechaAlta() {
        return this.fechaAlta;
    }
}

public class Producto {
    private double precio;
    
    public double getPrecio() {
        return this.precio;
    }
}


## Refactoring: Move Method, Extract Method

public class Pedido {
    private Cliente cliente;
    private List<Producto> productos;
    private FormaPago formaPago;

    public Pedido(Cliente cliente, List<Producto> productos, FormaPago formaPago) {
        if (!"efectivo".equals(formaPago)
                && !"6 cuotas".equals(formaPago)
                && !"12 cuotas".equals(formaPago)) {
            throw new Error("Forma de pago incorrecta");
        }
        this.cliente = cliente;
        this.productos = productos;
        this.formaPago = formaPago;
    }

    public double getCostoTotal() {
        double costoProductos = this.productos.stream().mapToDouble(producto -> producto.getPrecio()).sum();
        
        double extraFormaPago = costoProductos * this.FormaPago.calcularFormaPago();

        int añosDesdeFechaAlta = this.cliente.AñosDesdeFechaAlta();
        
        // Aplicar descuento del 10% si el cliente tiene más de 5 años de antigüedad
        if (añosDesdeFechaAlta > 5) {                              // Extract Method, Replace Temp with Query
            return (costoProductos + extraFormaPago) * 0.9;        // Extract Method, Replace Temp with Query
        }
        return costoProductos + extraFormaPago;                   //Extract Method, Replace Temp with Query
    }
}

public Interface FormaPago{
	public double obtenerExtra();
}

public class Efectivo implements FormaPago{
	public double obtenerExtra(){
  	return 0;
  }
}
public class SeisCuotas implements FormaPago{
	public double obtenerExtra(){
  	return 0.2;
  }
}
public class DoceCuotas implements FormaPago{
	public double obtenerExtra(){
  	return  0.5;
  }
}

public class Cliente {
    private LocalDate fechaAlta;
    
    public LocalDate getFechaAlta() {
        return this.fechaAlta;
    }
    
    public int AñosDesdeFechaAlta(){
    	return Period.between(this.FechaAlta,LocalDate.now()).getYears();
    }
}

public class Producto {
    private double precio;
    
    public double getPrecio() {
        return this.precio;
    }
}

## Extract Method, Replace Temp with Query

public class Pedido {
    private Cliente cliente;
    private List<Producto> productos;
    private FormaPago formaPago;

    public Pedido(Cliente cliente, List<Producto> productos, FormaPago formaPago) {
        if (!"efectivo".equals(formaPago)
                && !"6 cuotas".equals(formaPago)
                && !"12 cuotas".equals(formaPago)) {
            throw new Error("Forma de pago incorrecta");
        }
        this.cliente = cliente;
        this.productos = productos;
        this.formaPago = formaPago;
    }

    public double getCostoTotal() {
        double costoProductos = this.productos.stream().mapToDouble(producto -> producto.getPrecio()).sum();
        
        double extraFormaPago = costoProductos * this.FormaPago.calcularFormaPago();

        int  añosDesdeFechaAlta = this.cliente.AñosDesdeFechaAlta();
        
        // Aplicar descuento del 10% si el cliente tiene más de 5 años de antigüedad
        return  calcularExtraPorAños(añosDesdeFechaAlta, costoProductos, extraFormaPago);
    }
    public double obtener calcularExtraPorAños(int añosDesdeFechaAlta, double costoProductos, double extraFormaPago){
       if (añosDesdeFechaAlta > 5) {
            return ( costoProductos + extraFormaPago) * 0.9;
        }
        return costoProductos + extraFormaPago;
    }
}

public Interface FormaPago{
	public double obtenerExtra();
}

public class Efectivo implements FormaPago{
	public double obtenerExtra(){
  	return 0;
  }
}
public class SeisCuotas implements FormaPago{
	public double obtenerExtra(){
  	return 0.2;
  }
}
public class DoceCuotas implements FormaPago{
	public double obtenerExtra(){
  	return  0.5;
  }
}

public class Cliente {
    private LocalDate fechaAlta;
    
    public LocalDate getFechaAlta() {
        return this.fechaAlta;
    }
    
    public int AñosDesdeFechaAlta(){
    	return Period.between(this.FechaAlta,LocalDate.now()).getYears();
    }
}

public class Producto {
    private double precio;
    
    public double getPrecio() {
        return this.precio;
    }
}


