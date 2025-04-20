
## Ejercicio 3

![Ejercicio3-Patrones drawio](https://github.com/user-attachments/assets/16d906ae-e0f3-4415-968d-5935efaab4a2)


```java
import java.util.ArrayList;
import java.util.List;

public class MediaPlayer {
	List<Media> media;
	
	public MediaPlayer() {
		this.media = new ArrayList<>();
	}
	
	public void play() {
		this.media.forEach(media -> media.play());
	}
}
```
