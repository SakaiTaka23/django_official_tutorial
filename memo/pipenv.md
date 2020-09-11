# pipenv

* ライブラリインストール 
* ```pipenv install 〇〇 ```
* gitなどでクローンしてきた時
* ```  pipenv install```
* Pipfile内のものをインストールする時
* ```pipenv install ```
* テキスト(requirements.txt)などで保存されている時 
* ``` pip install -r requirements.txt```
* ↑のテキストを作成する時
* ``` pipenv run pip freeze > requirements.txt ```

仮想環境を作成

* ```pipenv install --python 3.8 ```

シェル

* 起動
* ``` pipenv shell ```
* 終了
* ``` (deactivate)``` ←いらない？
*  ``` exit ```

削除

* 仮想環境そのものを削除

* ```pipenv --rm```
* 内容のみ削除(仮想環境は残る)
* ```pipenv uninstall all```

アンインストール

* ライブラリをアンインストール
* ```pipenv uninstall ライブラリ名```

アップデート

* ライブラリを最新のものにアップデート
* ```pipenv update```

