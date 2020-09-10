# pipenv

* ライブラリインストール 
* ```pipenv install 〇〇 ```
* gitなどでクローンしてきた時
* ```  pipenv install```
* テキスト(requirements.txt)などで保存されている時 
* ``` pip install -r requirements.txt```
* ↑のテキストを作成する時
* ``` pipenv --python 3.8.5 ```

仮想環境を作成

* ```pipenv install --python 3.8 ```

シェル

* 起動
* ``` pipenv shell ```
* 終了
* ``` deactivate exit ```

削除

* 仮想環境そのものを削除

* ```pipenv --rm```
* 内容のみ削除
* ```pipenv uninstall all```

