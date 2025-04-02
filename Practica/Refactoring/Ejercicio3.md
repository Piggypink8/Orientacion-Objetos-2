# Ejercicio 3

```java
public class Document {
	List<String> words; // Break Encapsulate
	public long characterCount() {
		long count = this.words.stream().mapToLong(w -> w.length()).sum(); // Temporary Field
		return count;
}
public long calculateAvg() { // Bad method name
	long avgLength = this.words.stream().mapToLong(w -> w.length()).sum() / this.words.size(); // Temporary Field
	return avgLength;
}
// Resto del código que no importa
}
```

## Code Smell: Break Encapsulate
## Refactoring: Encapsulate Field

```java
public class Document {
	private List<String> words; 
	public long characterCount() {
		long count = this.words.stream().mapToLong(w -> w.length()).sum(); // Temporary Field
		return count;
}
public long calculateAvg() { // Bad method name
	long avgLength = this.words.stream().mapToLong(w -> w.length()).sum() / this.words.size(); // Temporary Field, Duplicated Code
	return avgLength;
}
// Resto del código que no importa
}
```

## Code Smell: Bad Method Name
## Refactoring: Change Name Method

```java
public class Document {
	private List<String> words; 
	public long characterCount() {
		long count = this.words.stream().mapToLong(w -> w.length()).sum(); // Temporary Field
		return count;
}
public long calculateCharactersAvg() { 
	long avgLength = this.words.stream().mapToLong(w -> w.length()).sum() / this.words.size(); // Temporary Field, Duplicated Code
	return avgLength;
}
// Resto del código que no importa
}
```

## Code Smell: Temporary Field
## Refactoring: Replace temp with query

```java
public class Document {
	private List<String> words; 
	public long characterCount() {
		return this.words.stream().mapToLong(w -> w.length()).sum(); 
	}
public long calculateCharactersAvg() { 
	return this.words.stream().mapToLong(w -> w.length()).sum() / this.words.size(); // Duplicated Code
}
// Resto del código que no importa
}
```
## Code Smell: Duplicated Code
## Refactoring: En este caso debería aplicarse el Substitute Algorithm pero al modificar el comportamiento del método ya que this.words.size() puede valer 0 y 
reemplazarlo por un stream de average no cumpliría con el comportamiento natural del método.

