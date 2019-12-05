## Welcome to GitHub Pages

You can use the [editor on GitHub](https://github.com/TakeoffAndroid/solid-principles-android/edit/master/README.md) to maintain and preview the content for your website in Markdown files.

Whenever you commit to this repository, GitHub Pages will run [Jekyll](https://jekyllrb.com/) to rebuild the pages in your site, from the content in your Markdown files.

### Interface Segregation

```
fun main() {
    val adapter = Adapter()
    adapter(object: Adapter.OnClickListener {
        override fun onItemClick(position: Int) {
            // Yes! I have received a callback, open up the next activity.
            println("Clicked position is $position")
        }

        override fun onRadioButtonClick(position: Int) {
            // This is no longer needed for this activity, but still I have been implemented for no use...
        }

    })
    adapter.execute()
}

class Adapter /*:RecyclerView.Adapter<RecyclerView.ViewHolder>()*/ {

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
