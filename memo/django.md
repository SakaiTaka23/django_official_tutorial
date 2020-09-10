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

* テンプレート関数は {% %}で囲むことによって使う
* extends, block content, for, if　などがある



#### テンプレート内でのurl指定

* 直接urlを書くことによっても指定することができるがその場合変更したい時のリスクが大きい

→urlの名前で指定する

url関数を使用して実装

```<li><a href="{% url 'polls:detail' question.id %}">{{ question.question_text }}</a></li>```

* detailはルーティング時にurlにつけた名前
* polls: はネームスペース を記述している
* question.idは渡す引数























