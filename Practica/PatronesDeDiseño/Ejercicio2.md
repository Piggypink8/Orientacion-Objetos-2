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
	
	public void setSueldoBasico(int sueldo) {
		this.sueldoBasico = sueldo;
	}

}

public class Temporario extends Empleado {
	private int cantHijos;
	private int cantHoras;
	private boolean casado;

	
	
	public Temporario(int cantHijos, int cantHoras, boolean casado) {
		this.cantHijos = cantHijos;
		this.cantHoras = cantHoras;
		this.casado = casado;
		this.setSueldoBasico(20000);
	}

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
	
	public Pasante(int examenRendidos) {
		this.examenRendidos = examenRendidos;
		this.setSueldoBasico(20000);
	}

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
	
	public Planta(int cantHijos, boolean casado, int aniosAntiguedad) {
		this.cantHijos = cantHijos;
		this.casado = casado;
		this.aniosAntiguedad = aniosAntiguedad;
		this.setSueldoBasico(50000);
	}

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

### TEST

```java
public class EmpleadoTest {
	
	Empleado pasante, temporario, planta;
	
	@BeforeEach
	void setUp() throws Exception {
		pasante = new Pasante(1);
		temporario = new Temporario(1,1,true);
		planta = new Planta(1,true,1);
	}
	
    @Test
    public void testSueldo() {
    	
        assertEquals(19300, pasante.sueldo());
        assertEquals(24350, temporario.sueldo());
        assertEquals(52050, planta.sueldo());
    }
}
```
