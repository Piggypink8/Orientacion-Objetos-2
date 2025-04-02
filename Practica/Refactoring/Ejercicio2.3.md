# Ejercicio2.3

```java
public List<Post> ultimosPosts(Usuario user, int cantidad) { // Long Method
	List<Post> postsOtrosUsuarios = new ArrayList<Post>();
	for (Post post : this.posts) {
		if (!post.getUsuario().equals(user)) {
			postsOtrosUsuarios.add(post);
		}
	}
	// ordena los posts por fecha
	for (int i = 0; i < postsOtrosUsuarios.size(); i++) { // Comments
		int masNuevo = i;
		for(int j= i +1; j < postsOtrosUsuarios.size(); j++) {
			if (postsOtrosUsuarios.get(j).getFecha().isAfter(
				postsOtrosUsuarios.get(masNuevo).getFecha())) {
				masNuevo = j;
			}
		}
		Post unPost = postsOtrosUsuarios.set(i,postsOtrosUsuarios.get(masNuevo));
		postsOtrosUsuarios.set(masNuevo, unPost);
	}  // Comments
	List<Post> ultimosPosts = new ArrayList<Post>();
	int index = 0;
	Iterator<Post> postIterator = postsOtrosUsuarios.iterator();
	while (postIterator.hasNext() && index < cantidad) {
		ultimosPosts.add(postIterator.next());
	}
	return ultimosPosts;		
}				// Long Method
```

## Code Smell: Long Method, Comments
## Refactoring: Extract Method 

```java
public List<Post> ultimosPosts(Usuario user, int cantidad) {
  List<Post> postsOtrosUsuarios = obtenerPostsExcluyendoUsuario(user);
  postsOtrosUsuarios = ordenarPostsMasRecientesPrimero(postsOtrosUsuarios);
  return obtenerUltimosNPosts(postOtrosUsuarios, cantidad);		
}	

public List<Post> obtenerPostsExcluyendoUsuario(Usuario user){ 	// Reivent wheel
	List<Post> postsOtrosUsuarios = new ArrayList<Post>();
	for (Post post : this.posts) {
		if (!post.getUsuario().equals(user)) {
			postsOtrosUsuarios.add(post);
		}
	}
  return postsOtrosUsuarios
}

public List<Post> ordenarPostsMasRecientesPrimero (List<Post> posts){ 	// Reivent wheel
  	for (int i = 0; i < posts.size(); i++) {
		int masNuevo = i;
		for(int j= i +1; j < postspostsposts.size(); j++) {
			if (posts.get(j).getFecha().isAfter(
				posts.get(masNuevo).getFecha())) {
				masNuevo = j;
			}
		}
		Post unPost = posts.set(i,posts.get(masNuevo));
		posts.set(masNuevo, unPost);
	}
  return posts;
}

public List<Post> obtenerUltimosNPosts(List<Post> posts, int cantidad){ 	// Reivent wheel
  List<Post> ultimosPosts = new ArrayList<Post>();
	int index = 0;
	Iterator<Post> postIterator = postsOtrosUsuarios.iterator();
	while (postIterator.hasNext() && index < cantidad) {
		ultimosPosts.add(postIterator.next());
	}
  return ultimosPosts;
}
```

## Code Smell: Reivent wheel
## Refactoring: Replace loop with pipeline

```java
public List<Post> ultimosPosts(Usuario user, int cantidad) { 
	List<Post> posts = new ArrayList<Post>(); // Long Method
  posts = this.obtenerPostsExcluyendoUsuario(user); // Long Method
	postsOtrosUsuariosRecientes = ordenarPostsMasRecientesPrimero(posts); // Long Method
	return this.obtenerUltimosNPosts(postOtrosUsuariosRecientes, cantidad);		
}	

public List<Post> obtenerPostsExcluyendoUsuario(Usuario user){ 
  return this.posts.stream().filter(post -> !post.getUsuario().equals(user)).Collect(Collectors.ToList());    
}

public List<Post> ordenarPostsMasRecientesPrimero (List<Post> posts){
  return posts.stream().sorted((postf1,postf2)-> postf1.getFecha().compareTo(postf1.getFecha())).Collect(Collector.ToList());
}

public List<Post> obtenerUltimosNPosts(List<Post> posts, int cantidad){ 	
  return posts.stream().limit(cantidad).collect(Collectors.toList());
}
```

## Code Smell: Long Method
## Refactoring: Replace Temp with Query

```java
public List<Post> ultimosPosts(Usuario user, int cantidad) { // Bad Name Method
  List<Post> posts = this.obtenerPostsExcluyendoUsuario(user);
  ordenarPostsMasRecientesPrimero(posts)
  return this.obtenerUltimosNPosts(posts, cantidad);		
}	

public List<Post> obtenerPostsExcluyendoUsuario( Usuario user){ 
  return this.posts.stream().filter(post -> !post.getUsuario().equals(user)).Collect(Collectors.ToList());    
}

public List<Post> ordenarPostsMasRecientesPrimero (List<Post> posts){
  return posts.stream().sorted((postf1,postf2)-> postf1.getFecha().compareTo(postf1.getFecha())).Collect(Collector.ToList());
}

public List<Post> obtenerUltimosNPosts(List<Post> posts, int cantidad){ 	
  return posts.stream().limit(cantidad).collect(Collectors.toList());
}
```

## Code Smell: Bad Name Method
## Refactoring: Rename Method

```java
public List<Post> ultimosNPostsExluyendoUsuario(Usuario user, int cantidad) { 
  List<Post> posts = this.obtenerPostsExcluyendoUsuario(user);
  ordenarPostsMasRecientesPrimero(posts)
  return this.obtenerUltimosNPosts(posts, cantidad);	
}	

public List<Post> obtenerPostsExcluyendoUsuario( Usuario user){ 
  return this.posts.stream().filter(post -> !post.getUsuario().equals(user)).Collect(Collectors.ToList());     // Feature Envy
}

public List<Post> ordenarPostsMasRecientesPrimero (List<Post> posts){
  return posts.stream().sorted((postf1,postf2)-> postf1.getFecha().compareTo(postf1.getFecha())).Collect(Collector.ToList());
}

public List<Post> obtenerUltimosNPosts(List<Post> posts, int cantidad){ 	
  return posts.stream().limit(cantidad).collect(Collectors.toList());
}
```
## Code Smell: Feature Envy
## Refactoring: Extract method


```java
public List<Post> ultimosNPostsExluyendoUsuario(Usuario user, int cantidad) { 
  List<Post> posts = this.obtenerPostsExcluyendoUsuario(user);
  return this.obtenerUltimosNPosts(ordenarPostsMasRecientesPrimero(posts), cantidad);		
}	

public List<Post> obtenerPostsExcluyendoUsuario( Usuario user){ 
  return this.posts.stream().filter(post -> post.notUser(user)).Collect(Collectors.ToList());    
}

public List<Post> ordenarPostsMasRecientesPrimero (List<Post> posts){
  return posts.stream().sorted((postf1,postf2)-> postf1.getFecha().compareTo(postf1.getFecha())).Collect(Collector.ToList());
}

public List<Post> obtenerUltimosNPosts(List<Post> posts, int cantidad){ 	
  return posts.stream().limit(cantidad).collect(Collectors.toList());
}
```


## Supuse un metodo que evalua que no sea el mismo usuario, no lo implementé pero creo que sería bastante obvio, en caso de hacerlo en un parcial, preguntaría si tengo que hacer la implementación o no.
