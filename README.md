# Combo-2-Optimizaci-n-de-Recursos
# Singleton + peso mosca
# Uso: Manejar instancias únicas y compartir objetos de manera eficiente.
# Nombre: Montiel Garcia Armando #20210602
# Optimización de recursos:
Es un aspecto crucial en el desarrollo de software, especialmente cuando se trata de manejar instancias únicas y compartir objetos de manera eficiente. 
Dos patrones de diseño que pueden ser útiles para lograr esto son el Singleton y el Flyweight.
# Singleton: 
Este patrón garantiza que una clase tenga una única instancia y proporciona un punto de acceso global a esa instancia. 
Es útil cuando solo se necesita una instancia de una clase en todo el sistema. Esto puede ayudar a optimizar el uso de recursos al evitar la creación de múltiples instancias de una clase que consume muchos recursos.
# Ejemplo combinado de Singleton y Flyweight en Java:
Supongamos que tenemos una clase Texture que representa una textura en un juego,
y queremos garantizar que solo exista una instancia de cada textura en todo el juego para optimizar el uso de memoria. 
Además, algunas texturas son compartidas por múltiples objetos en el juego, por lo que también queremos utilizar el patrón Flyweight para reutilizar esas texturas compartidas.
# CODIGO:
import java.util.HashMap;
import java.util.Map;

// Singleton
public class TextureManager {
    private static TextureManager instance;
    private Map<String, Texture> textures;

    private TextureManager() {
        textures = new HashMap<>();
    }

    public static synchronized TextureManager getInstance() {
        if (instance == null) {
            instance = new TextureManager();
        }
        return instance;
    }

    // Flyweight
    public Texture getTexture(String filename) {
        if (!textures.containsKey(filename)) {
            // Si la textura no existe, la creamos y la almacenamos en el mapa.
            Texture texture = new Texture(filename);
            textures.put(filename, texture);
        }
        return textures.get(filename);
    }
}

// Flyweight
public class Texture {
    private String filename;

    public Texture(String filename) {
        this.filename = filename;
        // Aquí podría haber lógica para cargar la textura desde el archivo.
    }

    public void render() {
        // Lógica para renderizar la textura.
        System.out.println("Rendering texture: " + filename);
    }
}

// Ejemplo de uso
public class Main {
    public static void main(String[] args) {
        TextureManager textureManager = TextureManager.getInstance();

        // Obtenemos una instancia de la textura 'grass.png'
        Texture grassTexture = textureManager.getTexture("grass.png");
        grassTexture.render();

        // Obtenemos otra instancia de la misma textura
        Texture sameGrassTexture = textureManager.getTexture("grass.png");
        sameGrassTexture.render(); // No se vuelve a cargar la textura, se reutiliza la misma instancia

        // Obtenemos una instancia de otra textura
        Texture stoneTexture = textureManager.getTexture("stone.png");
        stoneTexture.render();
    }
}

La clase TextureManager actúa como un Singleton que gestiona las texturas en el juego. Utiliza el patrón Flyweight para reutilizar las instancias de texturas compartidas. 
Cuando se solicita una textura mediante el método getTexture, el TextureManager comprueba si ya existe una instancia de esa textura en el mapa. 
Si no existe, crea una nueva instancia y la almacena en el mapa; de lo contrario, simplemente devuelve la instancia existente. 
Esto garantiza que solo haya una instancia de cada textura y que las texturas compartidas sean reutilizadas en todo el juego, optimizando así el uso de memoria.
