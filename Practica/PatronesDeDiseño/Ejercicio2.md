## Ejercicio 2

### Aplico patrón de diseño template method.

```java
public abstract class Empleado {
	private int sueldoBasico;
	
	
	public abstract double basico();
	
	public abstract double  adicional();
	
	public double descuento() {
		return this.sueldoBasico * 0.13 + this.adicional() * 0.05;
	}

	public int getSueldoBasico() {
		return sueldoBasico;
	}
	
	protected double sueldo() {
		return this.basico() + this.adicional() - this.descuento();
	}
}
public class Temporario extends Empleado {
	private int cantHijos;
	private int cantHoras;
	private boolean casado;

	public double basico() {
		return this.getSueldoBasico() + this.cantHoras * 300;
	}
	
	@Override
	public double  adicional(){
		return this.montoCasado() + this.cantHijos * 2000;
	}
	
	public int montoCasado() {
		if(casado) {
			return 5000;
		}
		return 0;
	}

	public int getCantHijos() {
		return this.cantHijos;
	}

	public int getCantHoras() {
		return cantHoras;
	}
	

}
public class Pasante extends Empleado {
	private int examenRendidos;
	@Override
	public double basico() {
		return this.getSueldoBasico();
	}
	
	@Override
	public double  adicional() {
		return this.examenRendidos * 2000;
	}

}
public class Planta extends Empleado{
	private int cantHijos;
	private boolean casado;
	private int aniosAntiguedad;
	
	@Override
	public double basico() {
		return this.getSueldoBasico();
	}
	
	@Override
	public double adicional() {
		return this.montoCasado() + this.cantHijos * 2000 + this.aniosAntiguedad * 2000;
	}
	
	public int montoCasado() {
		if(casado) {
			return 5000;
		}
		return 0;
	}


}
```
