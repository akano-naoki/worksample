selfはインスタンスオブジェクト自身を指しています。クラスの中でインスタンスオブジェクトを呼び出す際やそのインスタンスオブジェクトを呼び出す際は、selfを使用します。


実際のコードで解説します。

以下の Person クラスがあります。


```
class Person
   attr_accessor :first_name, :family_name

   def initialize(first_name, family_name)
     self.first_name = first_name
     self.family_name = family_name
   end
end
```

ここで、新たな「Person」のオブジェクトを定義しました。takuさんです。

```
taku = Person.new('Takumi', 'YAMASHITA')
taku.first_name # => 'Takumi'
taku.family_name # => 'YAMASHITA'
```

さて、ここで taku さんのフルネームを返す処理を作るとどうなるでしょうか。
以下のようになります。

```
class Person
   attr_accessor :first_name, :family_name

   def initialize(first_name, family_name)
     self.first_name = first_name
     self.family_name = family_name
   end
   
   # 新しく追加
   def full_name
      self.first_name + ' ' + self.family_name
   end
end

taku.full_name # => 'Takumi YAMASHITA'
```

得たい ```full_name``` は、takuさん自身の名前(first_name)と、takuさん自身の苗字（full_name）を合わせたものになります。
ここの ```self``` は、「takuさん自身」、```self.first_name``` は、「takuさん自身の下の名前」を示しています。

```
ken = Person('Kentaro', 'HOSHINO')
ken.full_name #=> 'Kentaro HOSHINO'
```

自分自身が Ken の場合、```self``` は「kenさん自身」、```self.first_name``` は、「kenさん自身の下の名前」を示しています。

その「自分自身の」を呼び出すときに ```self``` という記述を使います。

「インスタンスオブジェクト」という名前を覚えるよりも先に、実際のコードではどうなっているか、自分のイメージの中でつかめるようにしましょう。
そのイメージがつかめた後、正確な概念と用語を覚えるようにすると学習が捗ります。
