## lambda（無名関数） について


### 復習：block について

前回、関数の引数に処理を渡す、block について説明しました。

```
def use_7_logic(&block)
  yield(7)
end

use_7_logic { |x| x * 2} => 14
use_7_logic { |x| x * x } => 49
```

また、Rails では、view の rendering について、block の機能を実は使っている、ということを説明しました。


### block にできなくて、lambda にできること

block 自体はそれだけで存在することができません。ですが、
lambda という記述を用いると、block そのものを、Procというクラスのオブジェクトにすることが出来ます（厳密には異なります。おおよその理解）。

オブジェクト化することにより、任意のタイミングで、処理が呼び出せるようになります。


具体例を挙げます。

```
def squaring(x)
  x * x
end
squaring(3) # => 9
```

上記の内容の処理を、下記の記述でも同じことが実現できます。


```
squaring = lambda { |x| x * x  }
squaring.call(3) # => 9
squaring.class # => Proc
```

また、下記の記述も可能です。

```
squaring = -> (x) { x * x  }
squaring.call(3) # => 9
squaring.class # => Proc
```


### Rails ではどう使われているか

先の例では、どのように役に立つかが分からないかと思われます。
ですが、この形にすることによって、「処理内容がオブジェクト化しているため、それ自体を引数として他のメソッドに渡すこと」ができるようになります。

ActiveRecord の association や scope に関して、これらの記述が使われています。


```
class Blog < ApplicationRecord
	belongs_to :user
	has_many :entries, -> { order('created_at DESC') }
	has_many :publish_entries, -> { where.not(draft: true) }, class_name: 'Entry'
	scope :publish, -> { where(publish: true) }
end

```

このときに、「並び順や対象レコードを指定する方法」として、このようなやり方を覚えてください、とお伝えしました。
実は、下記の行の意味は、「has_many というメソッドの、第一引数に symbol を渡し、第二引数に Proc のオブジェクトを渡す。第二引数がある場合、その条件で、該当のレコードを取得する」という意味なのです。

```has_many :entries, -> { order('created_at DESC') }```

この方法は勿論、association や scope を作成するとき以外でも使用可能です。より効率的に、コードを書くことができるようになります。


### 参考文献

より厳密に知りたい場合、下記ページを参照してください。
https://docs.ruby-lang.org/ja/latest/class/Proc.html
