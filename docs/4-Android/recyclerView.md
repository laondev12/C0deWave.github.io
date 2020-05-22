---
layout: default
title: RecyclerView
parent: Android
nav_order: 1
---

# RecyclerView를 만드는 방법

리싸이클러 뷰를 만들기 위해서는 어댑터를 만들어야 합니다.

## 리사이클러뷰는 무엇인가?

리스트 뷰의 발전한 버전으로써 화면에 맞게 리스트를 생성하고 추가로 위아래에 여백으로 뷰를 좀더 만들어 줍니다.

그 후에 이렇게 만든 뷰 홀더 안에 데이터를 담아서 보냄으로써 뷰를 재활용하기 때문에 리사이클러 뷰 라고 합니다.

## 어댑터란

어댑터는 레이아웃을 만들어서 관리하고 뷰를 만들어서 이를 연결해서 반환하는 역할을 합니다.

---

## 메인 액티비티 부분에 리사이클러뷰 추가하기

![](https://github.com/C0deWave/C0deWave.github.io/blob/master/image/200522/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7%202020-05-22%20%EC%98%A4%EC%A0%84%2010.58.19.png?raw=true)

---

## 아이템 레이아웃 만들기

![](https://github.com/C0deWave/C0deWave.github.io/blob/master/image/200522/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7%202020-05-22%20%EC%98%A4%ED%9B%84%203.39.53.png?raw=true)

---

## 어댑터 만들기

```Kotlin
//어댑터 클래스는 RecyclerView.Adapter<아이템 클래스>() 를 상속 받아야 합니다.
class TextAdapter(var data : List<String>) :   
    RecyclerView.Adapter<TextItem>() 
    {
        // 반환형이 ItemClass 인 onCreateViewHolder 가 필요합니다.
        //처음에 화면에 들어가는 개수 + 여분으로 레이아웃을 만들어주고 
        //더이상 작동하지 않습니다.
        override fun onCreateViewHolder(
            parent: ViewGroup, 
            viewType: Int) : TextItem 
            {
                var view = LayoutInflater.from(parent.context)
                            .inflate(R.layout.item,parent,false)
                return TextItem(view)
            }

    //데이터의 수를 받아서 아이템이 몇가나 있는지 알려주는 함수입니다.
    //데이터의 사이즈를 반환하면 됩니다.
    override fun getItemCount(): Int {
        return data.size
    }

    //데이터를 뷰홀더에 바인딩해주는 작업을 하는 부분입니다.
    //우리가 위아래로 스크롤하면 작동합니다.
    override fun onBindViewHolder(holder: TextItem, position: Int) {
        holder.binddata(data[position])
    }
}

//아이템 클래스로써 RecyclerView.ViewHolder(itemView)를 상속 받아야 합니다.
//아이템을 바인딩 해주는 함수가 들어있습니다.
class TextItem(itemView: View) : RecyclerView.ViewHolder(itemView){
    fun binddata(string : String){
        var textView = itemView.textView
        textView.text = string
    }
}
```

---

## 메인 액티비티 부분

```Kotlin
class MainActivity : AppCompatActivity() {

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

        var listArray:MutableList<String> = mutableListOf("a","b","c","d","e","d","asd","wqeq","dsa")

        recyclerView.adapter = TextAdapter(listArray)
        recyclerView.layoutManager = LinearLayoutManager(this)
    }
}
```

---

## 결과 화면

![](https://github.com/C0deWave/C0deWave.github.io/blob/master/image/200522/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7%202020-05-22%20%EC%98%A4%ED%9B%84%203.38.29.png?raw=true)

잘 작동하는 것을 볼 수 있습니다.