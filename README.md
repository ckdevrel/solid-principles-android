
## I — The Interface Segregation Principle (ISP):

### Bad
```kotlin
fun main() {
    val adapter = Adapter()
    adapter(object: Adapter.OnClickListener {
        override fun onItemClick(position: Int) {
            // Yes, I have received a callback, go to the next activity.
            println("Clicked position is $position")
        }
        override fun onRadioButtonClick(position: Int) {
            // This is no longer needed for this activity, but still I have been implemented for no use...
        }

    })
    adapter.execute()
}

class Adapter {

    private var onClickListener: OnClickListener? =null
   
    operator fun invoke (onClickListener: OnClickListener) {
        this.onClickListener = onClickListener
    }

    fun execute() {
        onClickListener?.onItemClick(4)
    }
    
    interface OnClickListener {
        fun onItemClick(position: Int)
        fun onRadioButtonClick(position: Int)
    }
}

```
### Good

```kotlin
fun main() {
    val adapter = Adapter()
    adapter(object: Adapter.OnItemClickListener {
        override fun onItemClick(position: Int) {
            // Yes, I have received a callback, go to the next activity.
            println("Clicked position is $position")
        }
    })
    adapter.execute()
}

class Adapter {

    private var onItemClickListener: OnItemClickListener? =null
   
    operator fun invoke (onItemClickListener: OnItemClickListener) {
        this.onItemClickListener = onItemClickListener
    }

    fun execute() {
        onItemClickListener?.onItemClick(4)
    }
    
    interface OnItemClickListener {
        fun onItemClick(position: Int)
    }
    
    interface OnRadioClickListener {
        fun onRadioButtonClick(position: Int)
    }

}
```

## D - The Dependency Inversion Principle (DIP)

### Bad
```kotlin
fun main() {
    /* As a end user, I don't care about how will I retrieve the data. 
     * Do whatever you want and simply get my data. 
     */
    val user = User()
    user.getMyData(3)
}

class User {

    fun getMyData(requestCode: Int) {
        when (requestCode) {
            1 -> {
                val firebase = Firebase()
                firebase.fetchData() 
            }
            2 -> {
                val restClient = RestClient()
                restClient.fetchData() 
            } 
            else -> print("I dont care about the user. Don't do anything. Keep quiet!")
        } 

    }
}

class Firebase {
    fun fetchData(){
        print("Syncing the data from the firebase storage")
    } 
}

class RestClient {
    fun fetchData(){ 
        print("Hitting the api and getting back the response")
    } 
}
```
### Good
```kotlin
fun main() {
    /* As a end user, I don't care about how will I retrieve the data. 
     * Do whatever you want and simply get my data. 
     */
    val user = User(dataFetcher = Firebase())
    user.getMyData()
}

class User(private val dataFetcher: DataFetcher) {

    fun getMyData() {
        dataFetcher.fetchData()
    }
}

interface DataFetcher {
    fun fetchData()
}

class Firebase: DataFetcher {
    override fun fetchData(){
       print("Syncing the data from the firebase storage")
    } 
}

class RestClient: DataFetcher {
    override fun fetchData(){ 
       print("Hitting the api and getting back the response")
    } 
}
```
