# Ejercicio 2.2

```java
public class Juego {
// ......
	public void incrementar(Jugador j) { // FEATURE ENVY
		j.puntuacion = j.puntuacion + 100;
	}
	public void decrementar(Jugador j) { // FEATURE ENVY
		j.puntuacion = j.puntuacion - 50;
	}

  public class Jugador {
		public String nombre;
		public String apellido;
		public int puntuacion = 0;
	}
}
```
### Code Smell: Feature Envy -> Los metodos incrementar y decrementar acceden directamente atributos de otra clase.
### Refactor: Move Method
```java
public class Juego {
// ......
	public void incrementar(Jugador j) {  //Bad method names
		j.incrementar();
	}
	public void decrementar(Jugador j) { //Bad method names
		j.decrementar();
	}

  public class Jugador {
		public String nombre;
		public String apellido;
		public int puntuacion = 0;
    
    public void incrementar() { //Bad method names
			j.puntuacion =+ 100;
		}
		public void decrementar() { //Bad method names
			j.puntuacion -= 50;
		}
	}
}
```
### Code Smell: Bad method names -> Los nombres de los metodos son poco explicativos con su propósito
### Refactor: Change method name
```java
public class Juego {
// ......
	public void incrementarPuntuacion(Jugador j) { 
		j.incrementarPuntuacion();
	}
	public void decrementarPuntuacion(Jugador j) { 
		j.decrementarPuntuacion();
	}

  public class Jugador {
		public String nombre; // Break Encapsulate
		public String apellido; // Break Encapsulate
		public int puntuacion = 0; // Break Encapsulate
    
    public void incrementarPuntuacion() { 
			j.puntuacion =+ 100;
		}
		public void decrementarPuntuacion() { 
			j.puntuacion -= 50;
		}
	}
}
```
### Code Smell: Break Encapsulate -> Los atributos de la clase Jugador son publicos y pueden modificarse desde otras clases
### Refactor: Encapsulate Field

```java
public class Juego {
// ......
	public void incrementarPuntuacion(Jugador j) { 
		j.incrementarPuntuacion();
	}
	public void decrementarPuntuacion(Jugador j) { 
		j.decrementarPuntuacion();
	}

  public class Jugador {
		private String nombre; 
		private String apellido; 
		private int puntuacion = 0; 
    
    public void incrementarPuntuacion() { 
			j.puntuacion =+ 100;
		}
		public void decrementarPuntuacion() { 
			j.puntuacion -= 50;
		}
	}
}
```
