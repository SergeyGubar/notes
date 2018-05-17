#### Recycler view

Optimization:

```kotlin
recycler.setHasFixedSize(true) // do it when the recycler view item do not change sizes of its children at runtime to optimize performance
```

Don't call notifyDataSetChanged() for UI update, use DiffUtil: [article](https://medium.com/@iammert/using-diffutil-in-android-recyclerview-bdca8e4fbb00)

Setting listener and getting correct position:

```kotlin
 override fun onCreateViewHolder(parent: ViewGroup, viewType: Int): LightViewHolder {
        val inflater = LayoutInflater.from(parent.context)
        val viewHolder =  LightViewHolder(inflater.inflate(R.layout.layout_example, parent, false))
        viewHolder.itemView.setOnClickListener({ v -> // Always set listener here
            someCallback(data[viewHolder.adapterPosition]) // To ensure that we get correct position use adapterPosition
        })
 }
```



