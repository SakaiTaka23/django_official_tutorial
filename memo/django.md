# Django



*　実行
*　```python manage.py rumserver```

``

## 設定ファイル **settings.py **

* db設定
* 言語・タームゾーン
* アプリ設定
* 静的ファイル設定

### 日本語設定

* languagecode = 'ja'
* Time_zone = 'Asia/Tokyo'



## migrate

``` python manage.py makemigrations ``` 

```python manage.py migrate```



## アプリ作成

```python manage.py startapp 〇〇```



## 管理者機能

* 管理者の作り方

* ```python manage.py createsuperuser ```

  →メールアドレスは空でも良い

モデルのadmin.pyにモデルをインポートし登録することで自作したモデルも管理できる

```from .models import 〇〇 ```

```admin.site.register(〇〇)```



## shell

起動

```python manage.py shell```



## ルーティング

* 各アプリ内にurls.pyを作成してルーティングを行う
* その後本体アプリのurls.pyに登録する(urls.pyのIncludeing another URLconf参照)

urlのパスはurls.pyのurlpatternsに記述

中にはpath関数を使用してルートを定義する

* path(ルート、関数、{名前})
* urlに変数がある場合は<>で囲んで型も指定、view関数に直接引数として渡すことができる

例： path('\<int:question_id\>/vote/', views.vote, name='vote'),

View:  def vote(request, question_id): ←question_idを直接引数とする

​			〜処理〜

* ルーティングのurlpatternsの上に　app_names = '〇〇'　を追加することによりネームスペースを変更することができる。このように指定することによって他のアプリで同じ名前のテンプレートがあっても競合することがない



## モデル

* 作成したアプリ名のフォルダ内のmodels.py



### モデルでのdbの型設定

詳細

https://docs.djangoproject.com/en/3.1/ref/models/fields/#field-types

* クラスにはmodels.Modelを継承させる

テンプレート

カラム 名 = models.フィールド(引数)

* モデルをマイグレートするときはsettings.pyのInstalledapps内に名前を入れておくこと
* configを参照する？
* クラス.apps.クラス＋config (アプリのapp.pyに書かれている)



## ビュー

各アプリのviews.pyを指す

ビューの役割

* リクエストページに関してHttpResponseオブジェクトを返す
* 404のような例外を返す

→それ以外はユーザー次第



### 404の返し方

* オブジェクトを探して見つからなかった場合404を返すことができる

1. ```from django.http import 404```
2. ```get_object_or_404(クラス,条件)```

このとき見つからなかった場合は404を返す

* get_list_or_404も存在する



### reverse関数

urlsに設定された名前を渡すと実際のurlを返す。一つ目の引数にurlの名前、2つ目以降には任意でその引数、キーワード引数などを受け取る 

* 引数を渡すときはキーワードを指定すること
* ```        return HttpResponseRedirect(reverse('polls:results', args=(question.id,)))```
* 一つ目の引数でurlの名前を指定、二つ目の引数でargsに引数を入れている



### ジェネリックビュー

* よく使用されるビューをまとめたもの

***実装方法*** 

1. url:ルートを書く

   ```    path('', views.IndexView.as_view(), name='index'),```

   views.実行する関数.as_view()

   * urls.pyのルーティングで引数が必要なものは引数をpkにする

2. view:

   ```python views.py
   from django.views import generic
   
   class IndexView(generic.ListView):
       template_name = 'polls/index.html'
       context_object_name = 'latest_question_list'
   ```

   * generic.ListView(ここは使いたいジェネリックビューによって変わる)を継承し、テンプレート名、テンプレートに渡す引数を指定
   * テンプレート名はデフォルトでは　アプリ名/モデル名_index.htmlとなるが変更したい場合はtemplate_name変数をオーバーライドする

   https://docs.djangoproject.com/en/3.1/topics/class-based-views/





## テンプレート

テンプレートは各アプリに templates/〇〇 を作成して書く

* settings.pyのinstalled apps内にアプリを入れている場合デフォルトでtemplates/〇〇内を探す
* 同じ名前のテンプレートが複数あった場合に先に見つけた方を優先して表示するため基本的にテンプレートはアプリ内に書く
* 変数参照時なかった場合であってもエラーは出ず、空白となる
* 変数は{{}}で囲むことによって参照できる　キーの名前を指定することによって表示させることができる

### ビューからテンプレートの表示

returnの戻り値

```return render (request,テンプレート,context)```

* requestはrender関数の引数なのでデフォルトで入れる
* contextは辞書型で入れる　テンプレートへ渡す引数を表している（任意）

### テンプレート関数

https://docs.djangoproject.com/ja/3.1/topics/templates/

* タグ・フィルタといったものもある

https://docs.djangoproject.com/en/3.1/ref/templates/builtins/

* テンプレート関数は {% %}で囲むことによって使う
* extends, block content, for, if　などがある
* forループ内で {{ forloop.counter }}を入れるとそのループの回数を取得できる



#### テンプレート内でのurl指定

* 直接urlを書くことによっても指定することができるがその場合変更したい時のリスクが大きい

→urlの名前で指定する

url関数を使用して実装

```<li><a href="{% url 'polls:detail' question.id %}">{{ question.question_text }}</a></li>```

* detailはルーティング時にurlにつけた名前
* polls: はネームスペース を記述している
* question.idは渡す引数



## formの作成



* postメソッドを使うformを作成したときは csrfタグをつける

```{% csrf_token %}```

*  フォームから送られてきた情報を取得するには request.POST['取得したい要素']

  →これらの値は常にstringとなっている

  →その要素がない場合はKeyErrorが発生する

* Djangoに限らずPOSTされたデータの処理に成功した場合はリダイレクトするべき

### リダイレクト処理の方法

```from django.http import HttpResponseRedirect```

```        return HttpResponseRedirect(reverse('polls:results', args=(question.id,)))```

* polls:resultsがリダイレクト先
* argsが引数



## テスト

* 各アプリのtests.pyに記述
* クラスはTestCaseを継承して書く　（名前の例：テストするクラス+Modles+Tests)
* self.assertIs(テストする関数 , 帰ってくるべき戻り値) 、self.assertEqual などでテストを試す
* 関数はtestから始まる名前であること

**実行**

```python
#〇〇にはクラス名を書く
python manage.py test 〇〇
```

* テストはモデルの関数のみならずテンプレートで表示されている結果のテストも行うことができる
* self.client.get(ルート) でそのルートに入り、リスポンスステータス、特定の文字列が含まれているか、特定のcontextの内容がどうなっているかのテストを行うことができる
* 必要であればクラスの外に擬似データを作成するような関数も作成する
* ソフトフェアが達成するべき要件に関しては全てテストを書くべき

**テストが満たすべき規則**

テストが多くなることも考えられるのでテストはいくつかの規則を守る

* テストクラスはそれぞれのモデル、ビューで分けるべき
* それぞれが満たすべき条件に関してテストメソッドを分ける
* メソッドの名前は何をテストしているかを表す















