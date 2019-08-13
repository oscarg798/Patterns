*Imaginemos que queremos crear un juego en el cual un jugador pueda disparar diferentes armas dada una posicion en un plano de 2 dimensiones y una dirección*

si solo existe un tipo de proyectil seria facil, creamos una clase asbtracta 

```kotlin
abstract class Projectile(private val x: Int,
                          private val y: Int,
                          private val direction: Direction)
```
Y un metodo disparar en nuestra clase Heroe

```kotlin
class OurHero {
    private var direction = Direction.LEFT
    private var x: Int = 42
    private var y: Int = 173

    fun shoot(): Projectile {
        return object : Projectile(x, y, direction) {
            // Draw and animate projectile here
        }
    }
}
```
Esta implementación funciona a la perfección pero debemos notar que, nuestro heroe ahora esta acoplado a solo un tipo de proyectil, por lo cual si  queremos diferentes tipos de armas, esta implementación se queda corta, es hay donde podemos usar el patron **strategy**

```kotlin
class Peashooter : Weapon {
    override fun shoot(x: Int,
                       y: Int,
                       direction: Direction) = 
                        object : Projectile(x, y, direction) {
        // Fly straight
    }
}

class Pomegranate : Weapon {
    override fun shoot(x: Int,
                       y: Int,
                       direction: Direction)  = 
                        object : Projectile(x, y, direction) {
        // Explode when you hit first enemy
    }
}

class Banana : Weapon {
    override fun shoot(x: Int,
                       y: Int,
                       direction: Direction)  = 
                        object : Projectile(x, y, direction) {
        // Return when you hit screen border
    }
}
```

Luego en nuestra clase heroe podemos declarar una variable de tipo de la interfaz, y asignamos una implementación por defecto

```kotlin
private var currentWeapon : Weapon = Peashooter()
```

Luego el metodo disparar 

```kotlin
fun shoot(): Projectile = currentWeapon.shoot(x, y, direction)
```

Ahora si queremos cambiar el arma solo tenemos que crear un metodo que reciba otra, y la asignamos

```kotlin
fun equip(weapon: Weapon) {
    currentWeapon = weapon
}
```

Debemos notar que esto es simple **polimorfismo**, ya que lo unico que hacemos es crear la interfaz y crear un set de implementaciónes, dejando que quien necesite una implementación concreta use la abstracción y 
no la implementación. 

Cuando estamos refactorizando podemos cambiar muchos bloques de `if` por un patron **strategy**, donde cada 
caso del `if` sera una implementación concreata.

Esto aportara los siguientes beneficio

1. Hace el algoritmo mas claro, disminuyendo o removiendo la logica de los condicionales
2. Simplifica una clase moviendo las variaciones de un metodo a una herarquia
3. Deja que cambiemos un algoritmo por otro en tiempo de ejecución

Y trae las siguientes desventajas

1. Complica el diseño ya que de alguna manera oculta las implementaciones
2. Complica la forma en la cual el algoritmo recibe la data de la clase donde se usa


         
