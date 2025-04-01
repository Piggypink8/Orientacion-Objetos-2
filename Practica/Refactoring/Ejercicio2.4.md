# Ejercicio2.4

```java
public class Producto {
	private String nombre;
	private double precio;
	public double getPrecio() {
		return this.precio;
	}
}
public class ItemCarrito {
	private Producto producto;
	private int cantidad;
	public Producto getProducto() {
		return this.producto;
	}
	public int getCantidad() {
		return this.cantidad;
	}
}

public class Carrito {
	private List<ItemCarrito> items;
	
  public double total() {
		return this.items.stream().mapToDouble(item ->item.getProducto().getPrecio() * item.getCantidad()).sum(); // Feature Envy
	}
}
```
## Code Smell: Feature Envy
## Refactoring: Extract Method

```java
public class Producto {
	private String nombre;
	private double precio;
	public double getPrecio() {
		return this.precio;
	}
}
public class ItemCarrito {
	private Producto producto;
	private int cantidad;
  
	public Producto getProducto() {
		return this.producto;
	}
	public int getCantidad() {
		return this.cantidad;
	}
  
        public double obtenerTotalPrecio(){
	        return this.getProducto().getPrecio() * this.getCantidad()  // Feature Envy
        }
  
}

public class Carrito {
	private List<ItemCarrito> items;
	
  public double total() {
		return this.items.stream().mapToDouble(item ->item.obtenerTotalPrecio).sum(); 
	}
}
```

## Code Smell: Feature Envy
## Refactoring: Extract Method

```java
public class Producto {
	private String nombre;
	private double precio;
  
	public double getPrecio() {
		return this.precio;
	}
}
public class ItemCarrito {
	private Producto producto;
	private int cantidad;
  
	public Producto getProducto() {
		return this.producto;
	}
	public int getCantidad() {
		return this.cantidad;
	}

        public double obtenerTotalPrecio(){
	        return this.obtenerPrecioProducto() * this.getCantidad()  
        }
  
        public double obtenerPrecioProducto(){
  	      return this.getProducto().getPrecio();
        }
  
}

public class Carrito {
	private List<ItemCarrito> items;
	
  public double total() {
		return this.items.stream().mapToDouble(item ->item.obtenerTotalPrecio).sum(); 
	}
}
```

