# Ejercicio 2.5

```java
public class Supermercado {
	public void notificarPedido(long nroPedido, Cliente cliente) {
		String notificacion = MessageFormat.format(“Estimado cliente, se le informa
		que hemos recibido su pedido con número {0}, el cual será enviado a la dirección
		{1}”, new Object[] { nroPedido, cliente.getDireccionFormateada() });
		// lo imprimimos en pantalla, podría ser un mail, SMS, etc..
		System.out.println(notificacion);
		}
}
public class Cliente {
	public String getDireccionFormateada() {
		return
		this.direccion.getLocalidad() + “, ” + // Feature Envy
		this.direccion.getCalle() + “, ” + // Feature Envy
		this.direccion.getNumero() +“, ”+  // Feature Envy
		this.direccion.getDepartamento(); // Feature Envy
}
```  

## Code Smell: Feature Envy
## Refactoring: Extract Method

```java
public class Supermercado {
	public void notificarPedido(long nroPedido, Cliente cliente) {
		String notificacion = MessageFormat.format(“Estimado cliente, se le informa
		que hemos recibido su pedido con número {0}, el cual será enviado a la dirección
		{1}”, new Object[] { nroPedido, cliente.getDireccionFormateada() });
		// lo imprimimos en pantalla, podría ser un mail, SMS, etc..
		System.out.println(notificacion);
		}
}
  
public class Cliente {
	public String getDireccionFormateada() {
		return this.getDireccion().obtenerDireccionFormateada();
  }
}

public class Direccion {
  public String Localidad; // Break Encapsulate
  public String Calle; // Break Encapsulate
  public String Numero; // Break Encapsulate
  public String Departamento; // Break Encapsulate

	public String obtenerDireccionFormateada(){
		return
		this.direccion.getLocalidad() + “, ” +
		this.direccion.getCalle() + “, ” + 
		this.direccion.getNumero() +“, ”+ 
		this.direccion.getDepartamento(); 
  }  
    
}
```

## Code Smell: Break Encapsulate
## Refactoring: Encapsulate Field

```java  
public class Supermercado {
	public void notificarPedido(long nroPedido, Cliente cliente) { 
		String notificacion = MessageFormat.format(“Estimado cliente, se le informa
		que hemos recibido su pedido con número {0}, el cual será enviado a la dirección
		{1}”, new Object[] { nroPedido, cliente.getDireccionFormateada() });
		// lo imprimimos en pantalla, podría ser un mail, SMS, etc..
		System.out.println(notificacion);
		}
}
  
public class Cliente {
	public String getDireccionFormateada() {  
		return this.getDireccion().obtenerDireccionFormateada();
  }
}

public class Direccion {
  private String Localidad; 
  private String Calle; 
  private String Numero; 
  private String Departamento;

	public String obtenerDireccionFormateada(){
		return
		this.direccion.getLocalidad() + “, ” +
		this.direccion.getCalle() + “, ” + 
		this.direccion.getNumero() +“, ”+ 
		this.direccion.getDepartamento(); 
  }  
    
}  
```
