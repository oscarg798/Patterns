La pereza es el mejor ejemplo para este patron, imaginemos que en nuestra rutina diaria despues de desayunar siempre tomamos 
alguna bebida

```kotlin
class MyMorning {

    fun takeBreakfast(){
      eat()
      drinkOrangeJuice()
    }
    
    fun eat(){
      //do something
    }
    
    fun drinkOrangeJuice(){
      //do something
    }
}
```

Pero Luego un domingo nos despertamos y queremos no tomar jugo de naranja sino que queremos una tasa de cafe; si notomamos nuestra clase
anterior siempre debe tomar solo jugo de naranja, para resolver esto y tomar diferentes debidas despues de comer
podrias usar el platron **Template Method**. Para esto debemos primero crear un clase abstracta con la implementación comun, 
y dejar un metodo abstracto para la implementación que difiere


```kotlin
abstract class MyMorning {

    fun takeBreakfast(){
      eat()
      drinkSomething()
    }
    
    private fun eat(){
      //do something
    }
    
    abstract fun drinkSomething()
}
```

Luego podriamos tener dos implementaciones bases, una para los dias de Lunes a Sabado

```kotlin
class MyRegularMorning: MyMorning() {

    override fun drinkSomething(){
      //Drink Orange Juice
    }
}
```

Y otra para los Domingos

```kotlin
class SundayMorning: MyMorning() {
    
    override fun drinkSomething(){
      //Drink some coffee
    }
}
```

Debemos notar que nuestras implementaciones concretas no necesitan sobreescribir el metodo  `takeBreakfast` ya que su clase padre lo hace y nosotros no añadimos ningun comportamiento al mismo. 

Tambien podriamos si queremos en nuestro caso añadir una implementacion por defecto al metodo `drinkSomething` para lo casos en 
que deseamos tomar jugos de naranja, y para los domingos solo no llamar el super(). Nuestro ejemplo quedaria de la siguiente manera

```kotlin
abstract class MyMorning {

    fun takeBreakfast(){
        eat()
        drinkSomething()
    }

    private fun eat(){
        //do something
    }

    open fun drinkSomething(){ 
        //Drink Orange Juice
    }
}
```

```kotlin
class MyRegularMorning: MyMorning()
```

```kotlin
class MySundayMorning: MyMorning() {


    override fun drinkSomething() {
        super.drinkSomething()
        //Drink some coffee
    }
}
```

En kotlin los metodos abstractos no pueden tener implementaciones por  lo cual lo declaramos simplemente `open`

Cuando en una clase tenemos comportamientos que son repetitivos y otros que no podemos usar el *Template method* para extraer el comportamiento
comun en un metodo, y el variante en otro, esto aporta los siguientes beneficios

1. Elimina el codigo duplicado moviendo la implementación del  codigo que no cambia a una clase padre
2. Permite a las clases hijas facilmente definir su comportamiento
3. Simplifica y comunica de manera efectiva los pasos de un algoritmo

Y tiene las siguientes desventajas

1. Complica el diseño debido a que  las clases hijas deben implementar el mismo metodo una y otra vez






