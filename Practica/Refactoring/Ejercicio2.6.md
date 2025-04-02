# Ejercicio2.6

```java
public class Usuario {
	String tipoSubscripcion;
// ...
	public void setTipoSubscripcion(String unTipo) {
		this.tipoSubscripcion = unTipo;
	}
  
	public double calcularCostoPelicula(Pelicula pelicula) { // Switch Statement
		double costo = 0;
		if (tipoSubscripcion=="Basico") {
			costo = 
		}
		else if (tipoSubscripcion== "Familia") {
			costo = 
		}
		else if (tipoSubscripcion=="Plus") {
			costo = 
		}
		else if (tipoSubscripcion=="Premium") {
			costo = 
		}
	return costo;
	}
}

public class Pelicula {
	LocalDate fechaEstreno;
// ...
	public double getCosto() {
		return this.costo;
	}
	public double calcularCargoExtraPorEstreno(){
		// Si la Película se estrenó 30 días antes de la fecha actual, retorna un cargo de 0$, caso contrario, retorna un cargo extra de 300$
		return (ChronoUnit.DAYS.between(this.fechaEstreno, LocalDate.now()) ) > 30 ? 0 : 300;
	}
}
```

## Code Smell: Switch Statement
## Refactoring: Replace Conditional with Polymorphism

```java
public class Usuario {
	private Interface tipoSubscripcion;
// ...
	public void setTipoSubscripcion(TipoSuscripcion unTipo) {
		this.tipoSubscripcion = unTipo;
	}
  
	public double calcularCostoPelicula(Pelicula pelicula) { 
		return this.tipoSubscripcion.calcularCostoPelicula(pelicula);
  }
}

public interface TipoSuscripcion {
	public double calcularCostoPelicula(Pelicula pelicula);
}

public class Basico implements TipoSuscripcion {
	public double calcularCostoPelicula(Pelicula pelicula){
		return pelicula.getCosto() + pelicula.calcularCargoExtraPorEstreno();
  }
}

public class Familia implements TipoSuscripcion {
	public double calcularCostoPelicula(Pelicula pelicula){
		return (pelicula.getCosto() + pelicula.calcularCargoExtraPorEstreno()) * 0.90; // Magic Numbers
  }
}
public class Plus implements TipoSuscripcion {
	public double calcularCostoPelicula(Pelicula pelicula){
		return pelicula.getCosto();
  }
}
public class Premium implements TipoSuscripcion {
	public double calcularCostoPelicula(Pelicula pelicula){ // Magic Numbers
		return pelicula.getCosto() * 0.75;
  }
}

public class Pelicula {
	LocalDate fechaEstreno;
// ...
	public double getCosto() {
		return this.costo;
	}
	public double calcularCargoExtraPorEstreno(){
		// Si la Película se estrenó 30 días antes de la fecha actual, retorna un cargo de 0$, caso contrario, retorna un cargo extra de 300$
		return (ChronoUnit.DAYS.between(this.fechaEstreno, LocalDate.now()) ) > 30 ? 0 : 300;
	}
}
```

## Code Smell: Magic Numbers
## Refactoring: Extract Method

```java
public class Usuario {
	private Interface tipoSubscripcion;
// ...
	public void setTipoSubscripcion(TipoSuscripcion unTipo) {
		this.tipoSubscripcion = unTipo;
	}
  
	public double calcularCostoPelicula(Pelicula pelicula) { 
		return this.tipoSubscripcion.calcularCostoPelicula(pelicula);
  }
}

public interface TipoSuscripcion {
	public double calcularCostoPelicula(Pelicula pelicula);
}

public class Basico implements TipoSuscripcion {
	public double calcularCostoPelicula(Pelicula pelicula){
		return pelicula.getCosto() + pelicula.calcularCargoExtraPorEstreno();
  }
}

public class Familia implements TipoSuscripcion {
	public double calcularCostoPelicula(Pelicula pelicula){
		return (pelicula.getCosto() + pelicula.calcularCargoExtraPorEstreno()) * this.getMontoPelicula(); 
  }
  public double getMontoPelicula(){
  	return 0.90;
  }
}

public class Plus implements TipoSuscripcion {
	public double calcularCostoPelicula(Pelicula pelicula){
		return pelicula.getCosto();
  }
}

public class Premium implements TipoSuscripcion {
	public double calcularCostoPelicula(Pelicula pelicula){
		return pelicula.getCosto() * this.getMontoPelicula();
  }
  public double getMontoPelicula(){
  	return 0.75;
  }
}

public class Pelicula {
	LocalDate fechaEstreno;
// ...
	public double getCosto() {
		return this.costo;
	}
	public double calcularCargoExtraPorEstreno(){
		// Si la Película se estrenó 30 días antes de la fecha actual, retorna un cargo de 0$, caso contrario, retorna un cargo extra de 300$ // Comments
		return (ChronoUnit.DAYS.between(this.fechaEstreno, LocalDate.now()) ) > 30 ? 0 : 300;
	}
}
```

## Code Smell: Comments
## Refactoring: Extract Method

```java
public class Usuario {
	private Interface tipoSubscripcion;
// ...
	public void setTipoSubscripcion(TipoSuscripcion unTipo) {
		this.tipoSubscripcion = unTipo;
	}
  
	public double calcularCostoPelicula(Pelicula pelicula) { 
		return this.tipoSubscripcion.calcularCostoPelicula(pelicula);
  }
}

public interface TipoSuscripcion {
	public double calcularCostoPelicula(Pelicula pelicula);
}

public class Basico implements TipoSuscripcion {
	public double calcularCostoPelicula(Pelicula pelicula){
		return pelicula.getCosto() + pelicula.calcularCargoExtraPorEstreno();
  }
}

public class Familia implements TipoSuscripcion {
	public double calcularCostoPelicula(Pelicula pelicula){
		return (pelicula.getCosto() + pelicula.calcularCargoExtraPorEstreno()) * this.getMontoPelicula(); 
  }
  public double getMontoPelicula(){
  	return 0.90;
  }
}

public class Plus implements TipoSuscripcion {
	public double calcularCostoPelicula(Pelicula pelicula){
		return pelicula.getCosto();
  }
}

public class Premium implements TipoSuscripcion {
	public double calcularCostoPelicula(Pelicula pelicula){
		return pelicula.getCosto() * this.getMontoPelicula();
  }
  public double getMontoPelicula(){
  	return 0.75;
  }
}

public class Pelicula {
	LocalDate fechaEstreno;
// ...
	public double getCosto() {
		return this.costo;
	}
	public double calcularCargoExtraPorEstreno(){
		if(this.obtenerDiasHastaFechaActual() > 30){
    	return 300;
    }
    return 0;
	}
  
  public int obtenerDiasHastaFechaActual(){
  	return ChronoUnit.DAYS.between(this.fechaEstreno, LocalDate.now())
  }
}
```
