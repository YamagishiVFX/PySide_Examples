# PySide2 Getting Started
Updated: 

Created: 2022/06/21 Tatsuya YAMAGISHI

## 関連 & 参考：
- [PySide2公式](https://doc.qt.io/qtforpython-5/index.html)
- [PySide2公式 : Qt for Python Quick start](https://doc.qt.io/qtforpython-5/quickstart.html#project-quick-start)
- [VFXのためのPySideまとめ](https://yamagishi-2bit.blogspot.com/2021/09/pyside.html)

## 概要：
1. Python3のインストール:
2. PySide2インストール:
3. 開発環境:
4. Qアプリケーションの作成:
5. Import PySide2:
6. GUIの作成:
   1. QtDesigner
7. Windowの作成:
   1. QMainWIndow
   2. QDialog
   3. QWidet
8. Widgetの継承基本系:
   1. クラスの継承
9. Widgetのカスタマイズ:
   1. Parent
10. Layout:
11. シグナルの設定:
12. DialogやMainWindowに作成したWidgetを配置:
13. Examples:


## 0. はじめに
- PySide2のコアはQt5(C++のライブラリ)
- PyQt5という似たようなライブラリもある
  - [PyQt5 vs. Qt for Python(PySide2)](https://qiita.com/ynuma/items/a005bf5538e282030460)
  - VFXではKatanaなど一部ツールがPyQtを採用
- PySide2のプログラムは `クラス` を使うため、Pythonのクラスの基本的な知識が必要。

## 1. Python3のインストール
Pythonのインストールに関しては色々解説があると思うのでここでは割愛。

### Pythonのバージョンについて
- VFX用途の場合は2022/06/21現在、`3.9の最新` がいいのではないかと思う。
- 主要ツールが3.7系なので `下位互換を意識する場合は3.7系の最新` が良いか。
- サポート終了、PySide2がPython2.7で動かないなど、**Python2系の選択は論外**。
- [VFX Reference Platform](https://vfxplatform.com/)などを参照。
  - CY2023 DRAFTに `Python3.10.x` の記載があるため、先を見越して `3.10` を選択するのもいいかも？3.10系では型に関する部分など、根本部分の仕様変更があるようだ。
  - [Python 3.10ではもう float から int への暗黙的変換を行いません。つまり、従来は float タイプの値を受け入れていた関数で、タイプエラーが発生するということです。
浮動小数点数の引数は明示的に整数に変換する必要がありま](https://wiki.blender.jp/Dev:JA/Ref/Release_Notes/3.10/Python_API)

## 2. PySide2 インストール：

### 参考
- [pipでアップデートするときのコマンド pip update](https://qiita.com/HyunwookPark/items/242a8ceea656416b6da8)
- [pipでいれたパッケージを一括アップデート](https://dragstar.hatenablog.com/entry/2016/09/02/113243)
- 関連：[[PySide2] インストール [VFX]](https://yamagishi-2bit.blogspot.com/2021/11/vfx-vfxpyside2-pyside2.html)


Pipを最新にしておく
```
pip install --upgrade pip
```

### PySide2のインストール
```
pip install PySide2
```
環境によっては`pip3`
- Python2 Python3が混在するような環境ではPython3用のpipコマンド `pip3` の場合がある。

```
pip3 install PySide2
```


**情報確認**
```
pip show PySide2
```

**PySide2を最新に**
```
pip install -U PySide2
```

**一覧**
```
pip list
```

**アップデート可能なパッケージリスト**
```
pip list -o
```


## 3. 開発環境
### VSCodeのインストール
- コードエディタとしてVSCodeをインストール
- MayaなどのVFXツールでは各ツールの `簡易コードエディタ` で代用可能なので、ここを飛ばしても開発は可能。
  - VSCodeなどの高機能エディタは `スペルミス` や `補完機能` など便利な機能が沢山あるため `VSCode` などの高機能エディタの使用を推奨。これに慣れてしまうと普通のドキュメント作成もVSCodeが手放せなくなってくる・・・。
  
- `VSCode` 以外のエディタではPythonだと `PyCharm` の名前をよく耳にする。
- Python単体では `JupyterLab`、`Google Colaboratory` なども便利だが PySideが `画面作成` のためのライブラリなのであまり相性が良くないように感じる。（主観）

    ![image](https://i.gyazo.com/a70de37f8f1609d7a447dfdbcb494af1.png)


### Python拡張をインストール
- 言語は `日本語` に設定して問題ないと思います。
- 左のツールバーのエクステンションなどからPythonの拡張をインストール

![](https://i.gyazo.com/a12b8c984f90b87ef7421532434f1109.png)


### Pythonスクリプトの実行
- プログラムを書いて `test.py` などで保存
- CTRL+F5 の `Run Without Debugging` で プログラムを評価
- 僕の環境では、F5の `Start Debugging` だと PySide2が上手く動作しない事がある。

![](https://i.gyazo.com/47e4c22b22258078972e2d57dd49afaf.jpg)

ターミナル（コマンドプロンプト）などでコマンドで実行
```
python test.py
```

### 大事な設定：TABはスペース4つを確認
画面右下を確認

![](https://i.gyazo.com/34de041a17d1adf071e27f8df55dd94e.png)

関連：[全ツール共通のスクリプトエディタの設定：インデントはスペース4つ](https://yamagishi-2bit.blogspot.com/2021/05/vfxpython.html)

### VSCode便利な操作
- [参考：VS Code の便利なショートカットキー](https://qiita.com/12345/items/64f4372fbca041e949d0)
- 記事を書いている環境が `英語キーボード` 環境であるため、日本語キーボードとショートカットが異なる場合あり。
  
| ショートカット | 説明 |
| ---- | ---- |
| Ctrl + \ | ページを水平分割 |
| Ctrl + / | コメントアウト |
| Alt + Shift + ↑↓ | 選択行（複数可）を複製 |
| Alt + Shift + ALT + ↑↓ | カーソルを複製（複数行同時編集） |
| Shift + Delete | 1行丸ごと削除 | 
| Ctrl + D | 選択している単語と同じ文字を選択。押すたびに追加選択 | 
| Alt + PgUp/PgDn | カーソル位置を変えずにスクロール | 


## 3. Qアプリケーション作成

アプリケーション作成最小コード

![](https://i.gyazo.com/9fe7fe6fe3f9385c9dffc2d03c15038a.png)
```Python
import sys

from PySide2.QtWidgets import QApplication, QWidget

# Qアプリケーション作成
app = QApplication(sys.argv)

# ウィジェットオブジェクト作成
window = QWidget()

# ウィジェットの表示
window.show()

# アプリケーションメインループ開始
app.exec_()
```

### 注1：QAplicationは一番最初に必ず作らないとならない
![image](https://i.gyazo.com/9d87fbcd0015771c9c0061dcbb0cb981.png)

- Widgetなどのオブジェクトが作れない。
```
> QWidget: Must construct a QApplication before a QWidget
```

### 注2：QtでGUIが作成されているVFXツールの場合
一般の環境ではあまりないかもしれないが、MayaなどのVFXツールはQtでアプリケーションが作られている事があり、ツール起動時に `QAplication` が作成されているらしく、`QAplication` を作ろうとするとエラーを返す。

**先ほどのコードの実行結果：**
```
# Result
# Error: RuntimeError: file <maya console> line 8: A QApplication instance already exists. 
```

Qtベースのツール上でのPySideはアプリケーションの処理を定義せずに実行する。
![image](https://i.gyazo.com/12c9f68e67d9fd7c5800d7c776f4be0d.png)

### PySide2が使えるVFXツールにおけるウィジェットの表示
```Python
import sys

from PySide2.QtWidgets import QWidget

# ウィジェットオブジェクト作成
window = QWidget()

# ウィジェットの表示
window.show()
```
- Nuke
- Houdini
- 3dsMax

などが同じコードでウィンドウを表示出来る。


## 5. Import PySide2
- [関連：import PySide](https://yamagishi-2bit.blogspot.com/2021/11/pyside2-import-pyside-vfx.html)
  - ここでは以下の方式を使用。
  - いくつかimportの方式があり、それぞれメリットデミリットがあるように思えるが、僕個人は `最適` を提案出来るほど知識を有していない。
- `PySide2.QtWidgets` に様々な種類のウィジェットが入っている。
```Python
from PySide2.QtCore import (
    QPoint, QRect, QSize, QTime, QUrl, Qt
)
from PySide2.QtGui import (
    QBrush, QColor, QKeySequence
)
from PySide2.QtWidgets import (
    QApplication,
    QMainWindow,
)
```

### 動作確認：バージョンの表示
```Python
"""
 Reference From : 
    https://doc.qt.io/qtforpython-5/quickstart.html
"""

import PySide2.QtCore

# Prints PySide2 version
print(PySide2.__version__)

# Prints the Qt version used to compile PySide2
print(PySide2.QtCore.__version__)
```
Result：表示されるバージョンはPySideのバージョン問よりはCoreとなっているQtのバージョンのようだ。
```
5.15.0
5.15.0
```

## 6.GUIの作成
ここではプログラムコード主体で進めていくが、GUIのデザインは `QtDesigner` を使うと楽。

- 関連：[QtDesignerで学ぶQtの基本概念](https://yamagishi-2bit.blogspot.com/2021/11/pyside2-qtdesignerqt-vfx.html)
- 関連：[QtDesignerの.uiファイルの読み込み](https://yamagishi-2bit.blogspot.com/2021/11/pyside2-qtdesigner-vfx.html)

- Linuxは `QtDesigner` を別途インストールする必要があるらしい。



## 7. Windowの作成
PySide2で準備されているWindow用のウィジェットは

- `QDialog`
- `QMainWindow`

の２つ。

---

### QDialog：ウィンドウとして表示
- 公式[PySide2 QDialog](https://doc.qt.io/qtforpython-5/PySide2/QtWidgets/QDialog.html)

最も簡単にWindowの作成、運用が可能。

![](https://i.gyazo.com/17d20a4102d4ace5183b77798b2be89e.png)

```Python
import sys

from PySide2.QtWidgets import (
    QApplication,
    QDialog
)

# Qアプリケーション作成
app = QApplication(sys.argv)

# ダイアログオブジェクト作成
dialog = QDialog()

# ウィンドウタイトルを変更
dialog.setWindowTitle('My Dialog')

# ウィンドウサイズの変更
dialog.resize(300, 200)

# ウィジェットの表示
dialog.show()

# アプリケーションメインループ開始
app.exec_()
```

### モーダルウィンドウ：QDialog.exec_()
QDialogは `exec_()` というメソッドを持っており、独自でアプリケーションの起動が出来る。この表示方法により `モーダルウィンドウ` として振舞うようだ。将来的に自分でダイアログとして表示させる際に知っておいた方がいい機能。

**参考：**

- [PySide2.QtWidgets.QDialog.exec_()](https://doc.qt.io/qtforpython-5/PySide2/QtWidgets/QDialog.html#PySide2.QtWidgets.PySide2.QtWidgets.QDialog.exec_)
- [モーダルウィンドウ](https://wa3.i-3-i.info/word11432.html)

Example : `show()` と `app.exec_()` を省略

![](https://i.gyazo.com/dc9fb8cb5da7204a2eb20c726b46c895.png)

```Python
import sys

from PySide2.QtWidgets import (
    QApplication,
    QDialog
)

# Qアプリケーション作成
app = QApplication(sys.argv)

# ダイアログオブジェクト作成
dialog = QDialog()

# ウィンドウタイトルを変更
dialog.setWindowTitle('My Dialog')

# ウィンドウサイズの変更
dialog.resize(300, 200)

# ダイアログでメインループ開始
dialog.exec_()
```

戻り値を受け取れる。ダイアログから値を取得する際などに大事な仕様。

```Python
# ダイアログでメインループ開始
result = dialog.exec_()
print(result)

# Result:
# 0
```

### QDailogからの派生ダイアログ
QDialogから派生する様々な種類のダイアログがあるようだ。

**引用：** [PySide2 QDialog](https://doc.qt.io/qtforpython-5/PySide2/QtWidgets/QDialog.html)
![](https://i.gyazo.com/492c8f0bac409df65e9fbf8967bdede7.jpg)


### QMainWindow
先ほどのQWidgetと表示は全く同じだが、QMainWindowはメニューバーやステータスバーを使えるようだ。

![](https://i.gyazo.com/0aae8bd03f9657f2734c4e8fb4d067e2.png)

```Python
import sys

from PySide2.QtWidgets import (
    QApplication,
    QMainWindow
)

# Qアプリケーション作成
app = QApplication(sys.argv)

# ウィジェットオブジェクト作成
window = QMainWindow()

# ウィンドウタイトルを変更
window.setWindowTitle('Main Window')

# ウィンドウサイズの変更
window.resize(300, 200)

# ウィジェットの表示
window.show()

# アプリケーションメインループ開始
app.exec_()
```

### QMainWinodwにメニューやステータスバーを追加
- [関連：QMainWindow](https://yamagishi-2bit.blogspot.com/2021/11/pyside2-qmainwindow-vfx.html)


QtDesignerでQMainWinodwを選択するとデフォルトでメニューやステータスバーが追加されているが、コードでフルスクラッチする場合は少し定義が多くなる。

![](https://i.gyazo.com/1da326108a048cd96121a7922a3bf43c.png)

Example : 説明用にメニューやステータスバーを実装
```Python
import sys

from PySide2.QtGui import (
    QKeySequence
)

from PySide2.QtWidgets import (
    QApplication, QAction, QMainWindow,
    QMenu, QMenuBar, QStatusBar,
)

# Qアプリケーション作成
app = QApplication(sys.argv)

# ウィンドウオブジェクト作成
window = QMainWindow()


#--------------------------#
# メニューバーの作成
#--------------------------#
"""
 * QMainWindow.menuBar()でもメニューバーを作成出来る。

menubar = window.menuBar()
"""
menubar = QMenuBar()
window.setMenuBar(menubar)

# ファイルメニューの作成
menu_file = QMenu('File')
menubar.addAction(menu_file.menuAction())

# # ファイルメニュー内にアクションを追加
action = QAction('Exit')
action.setShortcut(QKeySequence('Ctrl+Q'))
action.triggered.connect(window.close)
menu_file.addAction(action)


#--------------------------#
# ステータスバーの作成
#--------------------------#
"""
 * QMainWindow.statuBar()でもステータスバーを作成出来る。

statusbar = window.statusBar()
"""
statusbar = QStatusBar(window)
window.setStatusBar(statusbar)
statusbar.showMessage('Status Bar') 
# statusbar.showMessage('Status Bar', 5000) # timeout: int=0 (ms)

# ウィンドウタイトルを変更
window.setWindowTitle('Main Window')

# ウィンドウサイズの変更
window.resize(300, 200)

# ウィンドウの表示
window.show()

# アプリケーションメインループ開始
app.exec_()
```

----


### QWidget
- 全てのWidgetのベース。これをベースに様々なWidgetに派生している。
- 後述する **parentの指定によって挙動が変わる。**
  - parentを指定しないと `Window` として起動
  - parentを指定すると `パーツ(Widget)` として配置
- **PySideでは `parent` を意識する事はとても大事**

引用：PySide2 QWidget
- `QMainWindow` や `QDialog` も `QWidget`　の派生クラスである事が分かる。

![](https://i.gyazo.com/5933fec47f762bf1ed072e628cf320b9.png)

Example:

![image](https://i.gyazo.com/c75f1fe1b0a38f4f5444fd3236542086.png)

```Python
import sys

from PySide2.QtWidgets import (
    QApplication,
    QWidget
)

# Qアプリケーション作成
app = QApplication(sys.argv)

# ウィジェットオブジェクト作成
view = QWidget()

# ウィンドウタイトルを変更
view.setWindowTitle('Main Window')

# ウィンドウサイズの変更
view.resize(300, 200)

# ウィジェットの表示
view.show()

# アプリケーションメインループ開始
app.exec_()
```

**Example2 : QPushButtonをWindowとして表示**

![image](https://i.gyazo.com/a2c06c60df8eff1ebe16ce53e64b7aaa.png)


```Python
import sys

from PySide2.QtWidgets import (
    QApplication,
    QPushButton
)

# Qアプリケーション作成
app = QApplication(sys.argv)

# ウィジェットオブジェクト作成
view = QPushButton('Push')

# ウィンドウタイトルを変更
view.setWindowTitle('Main Window')

# ウィンドウサイズの変更
view.resize(300, 200)

# ウィジェットの表示
view.show()

# アプリケーションメインループ開始
app.exec_()
```

## Widgetのカスタマイズ基本系
- `QDialog`、`QMainWinodw` を始め様々なWidgetがあるが、`QWidget` から派生しているため、基本的な挙動はWidgetでも同じ。

### 基本系
- 特定のWidgetのクラスを継承し拡張するやり方が基本

Example: QWidgetのカスタマイズ

![image](https://i.gyazo.com/416864341facd0c3b376ae18aa2be73f.png)

```Python
import sys

from PySide2.QtWidgets import (
    QApplication,
    QWidget
)

# QWidgetクラスを継承してカスタムクラスを作成
class MyWidget(QWidget):
    # __init__関数。parent=Noneはテンプレート
    def __init__(self, parent=None):

        # parentを継承元に渡すようにする。
        super().__init__(parent)

# Qアプリケーション作成
app = QApplication(sys.argv)

# ウィジェットオブジェクト作成
view = MyWidget()

# ウィンドウタイトルを変更
view.setWindowTitle('MyWidget')

# ウィンドウサイズの変更
view.resize(300, 200)

# ウィジェットの表示
view.show()

# アプリケーションメインループ開始
app.exec_()
```

### Widgetの設定をクラス内に移動
- 処理する場所をクラス内に移しただけ。結果は同じ。どこが最適か？は実装によって異なってくると思う。
- Widgetに使える関数は「[公式リファレンス:QWidget](https://doc.qt.io/qtforpython-5/PySide2/QtWidgets/QWidget.html)」などを参照
- 派生したクラスでさらに関数が拡張され、使える関数は滅茶苦茶沢山ある。

![image](https://i.gyazo.com/416864341facd0c3b376ae18aa2be73f.png)

```Python
import sys

from PySide2.QtWidgets import (
    QApplication,
    QWidget
)

# QWidgetクラスを継承してカスタムクラスを作成
class MyWidget(QWidget):
    def __init__(self, parent=None):
        super().__init__(parent)
        
        # ウィンドウタイトルの設定
        self.setWindowTitle('MyWidget')
        
        # ウィンドウサイズの変更
        self.resize(300, 200)


app = QApplication(sys.argv)
view = MyWidget()
view.show()
app.exec_()
```

### 基本的にはどのクラスを継承しても構文は同じ
Example : QDialogにした場合

![image](https://i.gyazo.com/416864341facd0c3b376ae18aa2be73f.png)

```Python
import sys

from PySide2.QtWidgets import (
    QApplication,
    QDialog
)

# QDialogクラスを継承してカスタムクラスを作成
class MyWidget(QDialog):
    def __init__(self, parent=None):
        super().__init__(parent)
        
        # ウィンドウタイトルの設定
        self.setWindowTitle('MyWidget')
        
        # ウィンドウサイズの変更
        self.resize(300, 200)


app = QApplication(sys.argv)
view = MyWidget()
view.show()
app.exec_()
```

基底クラスを `QDialog` に変更しただけ

![](https://i.gyazo.com/f616bf0c73fe38bdee852b91d05337d2.png)


### QPushButtonの場合

![](https://i.gyazo.com/a2160eacc08b7d8e6184756e94c2d363.png)

```Python
import sys

from PySide2.QtWidgets import (
    QApplication,
    QPushButton
)

# QPushButtonクラスを継承してカスタムクラスを作成
class MyWidget(QPushButton):
    def __init__(self, parent=None):
        super().__init__(parent)
        
        # ウィンドウタイトルの設定
        self.setWindowTitle('MyWidget')
        
        # ウィンドウサイズの変更
        self.resize(300, 200)


app = QApplication(sys.argv)
# QPushButtonの第一引数はボタンラベル名
view = MyWidget('Push')
view.show()
app.exec_()
```

## Widgetのカスタマイズ
- 基本的にはどのクラスでも問題ない。
- `QWidget` は `QDialog` や `QMainWindow` のように、**用途が明確では無い**ので必要に応じて拡張が出来る。

### QWidgetを拡張してボタンを配置してみる。
ボタンオブジェクトを配置したが、何も表示されない。

![image](https://i.gyazo.com/2799dfec90067f4eaaaa9246ede405bd.png)

```Python
import sys

from PySide2.QtWidgets import (
    QApplication, QPushButton, QWidget,
)

# QWidgetクラスを継承してカスタムクラスを作成
class MyWidget(QWidget):
    def __init__(self, parent=None):
        super().__init__(parent)

        self.button = QPushButton('Push')
        
        self.setWindowTitle('MyWidget')
        self.resize(300, 200)


app = QApplication(sys.argv)
view = MyWidget()
view.show()
app.exec_()****
```

### widgetはparentをしていしないとWindow。parentを指定するとパーツ。
`QPushButton.show()` をするとButtonも別Windowとして表示される。`QPushButton` など Widgetは `QWidget` の派生クラスなので、基本的に **`QWidget`の特徴を継承** しているようだ。

![image](https://i.gyazo.com/2d74dacf2e7ca185eeb54bbaa903e27c.png)


### parentを指定してみる
[PySide2 : QPushButtonのリファレンス](https://doc.qt.io/qtforpython-5/PySide2/QtWidgets/QPushButton.html) を見てみると最後の引数がparentのようだ。
![image](https://i.gyazo.com/9afdd20d353519ab543dc59d9acc438e.png)

`parent` としてベースのQWidgetを指定してみる。今度はWidget内にパーツとして配置された。
![image](https://i.gyazo.com/89fc50a452127d7c963d206cd7179110.png)


### Widgetの配置：setGeometry(x, y, width, height)
Widgetのレイアウトは `QWidget.setGeometry(x: int, y:int, w: int, h: int)` で出来る。

![image](https://i.gyazo.com/f83efdfa4716789da7c1f5f17a516077.png)

QtDesignerなら、レイアウトを簡単に出来るが・・・
![image](https://i.gyazo.com/5a6bb2436206eb0c867ecf07dc1a25a1.png)


### QLayout
数値でレイアウトしていくのは大変なので、PySideでのWidgetのレイアウトは基本的に `QLayout` を使って行っていく。

QLayoutは

- 水平レイアウト：QHBoxLayout
- 垂直レイアウト：QVBoxLayout
- グリッドレイアウト：QGridLayout

などがある。

### Widgetのメインレイアウトを設定：setLayout
QtDesignerだと図の部分

![](https://i.gyazo.com/64ea713fd81b1a829056ea52327fcd3f.png)


### Example

![image](https://i.gyazo.com/2e89fe2e0f0e42ee1208215b5f44d42b.png)

`QWidget.layout()` というWidgetのメインレイアウト取得用の関数があるため、変数名に注意。

```Python
import sys

from PySide2.QtWidgets import (
    QApplication, QPushButton, QWidget, QVBoxLayout
)

# QDialogクラスを継承してカスタムクラスを作成
class MyWidget(QWidget):
    def __init__(self, parent=None):
        super().__init__(parent)
        """
        setLayoutでparentの定義もされるので引数のselfを省略しても問題ない。
        self.main_layout = QVBoxLayout()
        """

        # 縦レイアウトを作成
        self.main_layout = QVBoxLayout()
        # Widgetにレイアウトをセット
        self.setLayout(self.main_layout)

        
        # ボタンを作成
        self.button_1 = QPushButton('Button1', self)
        # レイアウトにボタンを追加
        self.main_layout.addWidget(self.button_1)


        # ボタン2を作成
        self.button_2 = QPushButton('Button2', self)
        self.main_layout.addWidget(self.button_2)

        self.setWindowTitle('MyWidget')
        self.resize(300, 200)

app = QApplication(sys.argv)
view = MyWidget()
view.show()
app.exec_()
```

### QHBoxLayoutの場合
![image](https://i.gyazo.com/ab1df78b7604329b374b9940330f8f1e.png)


### ボタンのサイズの変更
![image](https://i.gyazo.com/86a18e4d62e7a4b28a26597dc3a3e977.png)


`QLayout` を使った場合、Widgetの配置やサイズは自動で調整されるため、大きさを変更したい場合は

- QtWidget.setMinimumSize(w: int, h: int)
  - QtWidget.setMinimumWidth(w: int)
  - QtWidget.setMinimumheight(h: int)
- QtWidget.setMaximumSize(w: int, h: int)
  - QtWidget.setMaximumWidth(w: int)
  - QtWidget.setMaximumHeight(h: int)
- QtWidget.setFixSize(w: int, h: int)
  - QtWidget.setFixWidth(w: int)
  - QtWidget.setFixHeight(h: int)

など使うと楽。

![](https://i.gyazo.com/ec312cd5b2af45c69ec8189d8030da6e.png)


```Python
import sys

from PySide2.QtWidgets import (
    QApplication, QPushButton, QWidget, QVBoxLayout
)

# QDialogクラスを継承してカスタムクラスを作成
class MyWidget(QWidget):
    def __init__(self, parent=None):
        super().__init__(parent)
        """
        setLayoutでparentの定義もされるので引数のselfを省略しても問題ない。
        self.main_layout = QVBoxLayout()
        """
        # 縦レイアウトを作成
        self.main_layout = QVBoxLayout(self)
        # Widgetにレイアウトをセット
        self.setLayout(self.main_layout)


        # ボタンを作成
        self.button_1 = QPushButton('Button1', self)
        # 固定サイズを設定
        self.button_1.setFixedSize(200, 50)
        # レイアウトにボタンを追加
        self.main_layout.addWidget(self.button_1)


        # ボタン2を作成
        self.button_2 = QPushButton('Button2', self)
        # 最大幅設定
        self.button_2.setMaximumWidth(100)
        self.main_layout.addWidget(self.button_2)

        self.setWindowTitle('MyWidget')
        self.resize(300, 200)

app = QApplication(sys.argv)
view = MyWidget()
view.show()
app.exec_()
```

### Layoutを組み合わせる
Layoutは `addLayout( <QLayout> )` でレイアウトも追加出来る。これを使う事でレイアウトの自由度が高くなる。

![](https://i.gyazo.com/48f6a097a56e1d0877f11640d7da24ad.png)

Example:
```Python
import sys

from PySide2.QtCore import Qt

from PySide2.QtWidgets import (
    QApplication, QHBoxLayout, 
    QLabel, QLineEdit, QPushButton,
    QVBoxLayout, QWidget
)

# QDialogクラスを継承してカスタムクラスを作成
class MyWidget(QWidget):
    def __init__(self, parent=None):
        super().__init__(parent)

        # メインレイアウトを作成
        self.main_layout = QVBoxLayout()
        # Widgetにレイアウトをセット
        self.setLayout(self.main_layout)


        # ボタンを作成
        self.button = QPushButton('Button1', self)
        # レイアウトにボタンを追加
        self.main_layout.addWidget(self.button)


        # レイアウト1を作成
        self.layout_1 = QHBoxLayout()

        # ラベルを作成しレイアウト１に登録
        self.label = QLabel('Name:')
        self.layout_1.addWidget(self.label)

        # ラインエディットを作成しレイアウト１に登録
        self.lineedit = QLineEdit(self)
        self.lineedit.setMinimumWidth(100)
        self.layout_1.addWidget(self.lineedit)

        # ボタンsetを作成しレイアウト１に登録
        self.button_set = QPushButton('Set', self)
        self.layout_1.addWidget(self.button_set)

        # レイアウト１をメインレイアウトにセット
        self.main_layout.addLayout(self.layout_1)

        self.setWindowTitle('MyWidget')
        self.resize(300, 200)

app = QApplication(sys.argv)
view = MyWidget()
view.show()
app.exec_()
```


### setLayoutの省略
QLayoutの引数parentにWidgetクラスを指定すれば、setLayout()の処理を省略出来たりする。
- 明示的にした方が分かりやすい気もするが。

![image](https://i.gyazo.com/47ae662277a07fdfe3fb2106259b571f.png)


### 位置合わせ
レイアウトの`setAlignment` を使う。

- `QLayout.setAlignment( <Qt.AlignmentFlag> )`

![image](https://i.gyazo.com/602af1f14406d266fbe2f40de1784d1f.png)


参考：[QtCore.Qt.AlignmentFlag](https://doc.qt.io/qtforpython-5/PySide2/QtCore/Qt.html#PySide2.QtCore.PySide2.QtCore.Qt.AlignmentFlag)

- `QtCore.Qt.AlignCenter`
- `QtCore.Qt.AlignTop`
- `QtCore.Qt.AlignLeft`
- `QtCore.Qt.AlignRight`

など。

## 11. シグナルの設定
PySideでUIを操作した際の処理を定義するためには `signal` を使うのが基本のようだ。

### テスト用スクリプト
![image](https://i.gyazo.com/dc774050a6a1be7a7f5993b3e149819e.png)

```Python
import sys

from PySide2.QtWidgets import (
    QApplication, QPushButton, QLineEdit, QWidget, QVBoxLayout
)

# QDialogクラスを継承してカスタムクラスを作成
class MyWidget(QWidget):
    def __init__(self, parent=None):
        super().__init__(parent)
        
        # メインレイアウト
        self.main_layout = QVBoxLayout()
        # Widgetにレイアウトをセット
        self.setLayout(self.main_layout)


        # ラベルを作成
        self.line_edit = QLineEdit(self)
        # レイアウトにボタンを追加
        self.main_layout.addWidget(self.line_edit)


        # ボタンを作成
        self.button = QPushButton('Push', self)
        self.main_layout.addWidget(self.button)

        self.setWindowTitle('MyWidget')
        
app = QApplication(sys.argv)
view = MyWidget()
view.resize(300, 200)
view.show()
app.exec_()
```

### QLineEditのシグナルを調べる
リファレンスを見てQLineEditの持つシグナルを調べる

参考：[PySide2 QLineEdit Signals](https://doc.qt.io/qtforpython-5/PySide2/QtWidgets/QLineEdit.html#signals)

![](https://i.gyazo.com/096f47ccbf6a79905480a14da6aed36a.png)

`textChanged`、`textEdited` など文字情報が変わった際に処理されるシグナルを使ってみようと思う。が、意味が同じに感じる。リファレンスを見る。説明が何もない。

![](https://i.gyazo.com/d2a9a07cd194b82b5b35543682943b3c.png)

こういう時は本家のQtのリファレンスを見に行く。

参考: [Qt5 LineEdit](https://doc.qt.io/qt-6/qlineedit.html#textChanged)

![](https://i.gyazo.com/28cc8956a5ee3ca1505fcf14a61ff599.png)

- **textChanged**
  - `文字` が変更した際に呼び出される。`setText()` などプログラム的に変更がされても実行される。
- **textEdit**
  - ユーザーが編集した際のみに呼び出される。

とても大きな違いがあるようだ。ここでは `textEdit` を使ってみる。

### シグナルの接続と接続先の関数を準備する。

文字を入力する度にコンソールに関数の実行結果が表示される。

![image](https://i.gyazo.com/776f246594093ff14da3796e4efda5d0.png)

Example:
```Python
import sys

from PySide2.QtWidgets import (
    QApplication, QPushButton, QLineEdit, QWidget, QVBoxLayout
)

# QDialogクラスを継承してカスタムクラスを作成
class MyWidget(QWidget):
    def __init__(self, parent=None):
        super().__init__(parent)
        
        # メインレイアウト
        self.main_layout = QVBoxLayout()
        # Widgetにレイアウトをセット
        self.setLayout(self.main_layout)


        # ラベルを作成
        self.line_edit = QLineEdit(self)
        self.line_edit.textEdited.connect(self.line_edit_edited)
        # レイアウトにボタンを追加
        self.main_layout.addWidget(self.line_edit)


        # ボタンを作成
        self.button = QPushButton('Push', self)
        self.main_layout.addWidget(self.button)

        self.setWindowTitle('MyWidget')

    def line_edit_edited(self):
        print('Test')
        
app = QApplication(sys.argv)
view = MyWidget()
view.resize(300, 200)
view.show()
app.exec_()
```

### シグナルのconnect先は関数を指定する
PySideの用語でsignalの接続先は `Slot（スロット）` と呼ばれるようだ。

![image](https://i.gyazo.com/48ec67c13608cd0bd6f873a7c0c3e651.png)


### 関数の実行結果ではなく、関数そのものを引数にする。
最初少し困惑したが、Pythonの仕様で関数そのものを代入出来る。
- `関数名()` で関数の実行結果
- `関数名` で関数そのもの

Example:
```Python
test = print
test('Test')

# Result:
# Test
```

### シグナルが引数を持つ場合がある
この場合、スロット側で何かしらの引数を受け取れる。詳しくはリファレンス参照。

![image](https://i.gyazo.com/6a041ff728f88c2329f4de85b42c89f3.png)

面倒なので、とりあえず関数を作って中身を見てみた。入力した文字列を受け取れるようだ。

![](https://i.gyazo.com/28124dc9aa533c60caaa71a06d998711.png)


```Python
import sys

from PySide2.QtWidgets import (
    QApplication, QPushButton, QLineEdit, QWidget, QVBoxLayout
)

# QDialogクラスを継承してカスタムクラスを作成
class MyWidget(QWidget):
    def __init__(self, parent=None):
        super().__init__(parent)
        
        # メインレイアウト
        self.main_layout = QVBoxLayout()
        # Widgetにレイアウトをセット
        self.setLayout(self.main_layout)


        # ラベルを作成
        self.line_edit = QLineEdit(self)
        # シグナルをスロットに接続
        self.line_edit.textEdited.connect(self.line_edit_edited)
        # レイアウトにボタンを追加
        self.main_layout.addWidget(self.line_edit)


        # ボタンを作成
        self.button = QPushButton('Push', self)
        self.main_layout.addWidget(self.button)

        self.setWindowTitle('MyWidget')

    # シグナルの接続先関数
    def line_edit_edited(self, arg):
        """ line_edit.textEdited

         * ここにLineEditが編集された際の処理を定義
        
        """
        
        print(type(arg))
        print(arg)

        
        
app = QApplication(sys.argv)
view = MyWidget()
view.resize(300, 200)
view.show()
app.exec_()
```

### Tips:Lambdaで定義も出来る
無記名関数をスロットとして定義出来る。知っておくと何かの時に役立つテクニック。
```Python
import sys

from PySide2.QtWidgets import (
    QApplication, QPushButton, QLineEdit, QWidget, QVBoxLayout
)

# QDialogクラスを継承してカスタムクラスを作成
class MyWidget(QWidget):
    def __init__(self, parent=None):
        super().__init__(parent)
        
        # メインレイアウト
        self.main_layout = QVBoxLayout()
        # Widgetにレイアウトをセット
        self.setLayout(self.main_layout)


        # ラベルを作成
        self.line_edit = QLineEdit(self)
        self.line_edit.textEdited.connect(lambda: print('Test'))
        # レイアウトにボタンを追加
        self.main_layout.addWidget(self.line_edit)


        # ボタンを作成
        self.button = QPushButton('Push', self)
        self.main_layout.addWidget(self.button)

        self.setWindowTitle('MyWidget')
        
app = QApplication(sys.argv)
view = MyWidget()
view.resize(300, 200)
view.show()
app.exec_()
```

### ボタンが押されたらLineEidtをクリアするようにしてみた
```Python
import sys

from PySide2.QtWidgets import (
    QApplication, QPushButton, QLineEdit, QWidget, QVBoxLayout
)

# QDialogクラスを継承してカスタムクラスを作成
class MyWidget(QWidget):
    def __init__(self, parent=None):
        super().__init__(parent)
        
        # メインレイアウト
        self.main_layout = QVBoxLayout()
        # Widgetにレイアウトをセット
        self.setLayout(self.main_layout)


        # ラベルを作成
        self.line_edit = QLineEdit(self)
        # シグナルをスロットに接続
        self.line_edit.textEdited.connect(self.line_edit_edited)
        # レイアウトにボタンを追加
        self.main_layout.addWidget(self.line_edit)


        # ボタンを作成
        """
        QPushButtonもLineEditと同じように
            * QPushButton.clicked
            * QPushButton.pressed
        というボタンを押した系のシグナルがある。
        クリックと右クリックのみの違いだったようだ
        """
        self.button = QPushButton('Push', self)
        self.button.clicked.connect(self.button_clicked)
        self.main_layout.addWidget(self.button)

        self.setWindowTitle('MyWidget')

    # シグナルの接続先関数
    def line_edit_edited(self, arg):
        """ line_edit.textEdited

         * ここにLineEditが編集された際の処理を記述
        
        """

        print(type(arg))
        print(arg)

    # ボタンを押した時のスロット
    def button_clicked(self):
        """ button.clicked

         * ここにボタンが押された際の処理を記述
        
        """
        self.line_edit.setText('')

        
        
app = QApplication(sys.argv)
view = MyWidget()
view.resize(300, 200)
view.show()
app.exec_()
```

## 作成したWidgetをDialogやMainWindowに配置
- PySideの仕様上 `QWidget`、`QDalog`,`QMainWindow` は同じようにも使える。
- `QWidgetクラス` は汎用性が高い。
- `QDialog` や `QMainWindow` は用途が明確。
- **用途を限定** したくない場合はGUIのデザインは `QWidget` を基本にしておくと便利。

### ダイアログとして表示
![iamge](https://i.gyazo.com/38ef3aeaa7aa4d43eb683db33e64ea5c.png)

- MyWidget部分は先ほどのコードを使うので、ここではQDialog部分のコードのみ掲載。

```Python
import sys

from PySide2.QtWidgets import (
    QApplication, QDialog,
    QPushButton, QLineEdit, QWidget, QVBoxLayout
)

# QDialogクラスを継承してカスタムクラスを作成
class MyWidget(QWidget):
    ・・・・
    <省略>
    ・・・・

class MyDialog(QDialog):
    def __init__(self, parent=None):
        super().__init__(parent)

        self.main_layout = QVBoxLayout(self)
        self.setLayout(self.main_layout)

        self.widget = MyWidget(self)
        self.main_layout.addWidget(self.widget)

        self.setWindowTitle(self.__class__.__name__)
        self.resize(300, 200)

app = QApplication(sys.argv)
dialog = MyDialog()
result = dialog.exec_()

"""
ダイアログだが普通のWindowとして表示したい場合

app = QApplication(sys.argv)
dialog = MyDialog()
dialog.show()
app.exec_()
"""
```

### ダイアログの使い方など
- 今回のコードではQDialogである意味は全くないが、「ダイアログとして使いたい」「ウィンドウで入力した情報を取得したい」等の場合に、意味が出てくる。
- 「OK」などのボタンを実装したり。

参考：

- 参考：DF TALK [祝PySide2デビュー！ ～ただひたすらウィジェットを紹介するページ～](https://dftalk.jp/?p=20768)
- 関連：[QDialog](https://yamagishi-2bit.blogspot.com/2021/11/updated20211126-yamagishi.html)

----

### メインウィンドウとして表示
QMainWindowのメインとなるエリアは `CentralWidget` と呼ばれる。何かエヴァっぽいすね。

![image](https://i.gyazo.com/df9290b8388021fefe257c502ea5ca38.png)


## Examples：
![image](https://i.gyazo.com/38ef3aeaa7aa4d43eb683db33e64ea5c.png)

```Python
import sys

from PySide2.QtWidgets import (
    QApplication, QMainWindow,
    QPushButton, QLineEdit, QWidget, QVBoxLayout
)

# QDialogクラスを継承してカスタムクラスを作成
class MyWidget(QWidget):
    ・・・・
    <省略>
    ・・・・    

class MyWindow(QMainWindow):
    def __init__(self, parent=None):
        super().__init__(parent)

        
        self.widget = MyWidget(self)
        self.setCentralWidget(self.widget)

        self.setWindowTitle(self.__class__.__name__)
        self.resize(300, 200)


app = QApplication(sys.argv)
window = MyWindow()
window.show()
app.exec_()
```

### 有効/無効
`QWidget.setDisabled(<Bool>)`

![image](https://i.gyazo.com/2191dea64f2f0fd59912bebfd1be32b2.png)

```Python
self.text_edit = QTextEdit(self)
self.text_edit.setPlainText('Test')
self.text_edit.setDisabled(True)
```

### 非表示
![image](https://gyazo.com/08451df39ced122de611a34bdf4f0b70.png)
```Python
self.lineedit.setHidden(True)
```

### フォントの変更
- `QWidget.setFont(GtGui.QFont)`

![image](https://i.gyazo.com/e3dec6fcee8a42bd02ce2b30d5c0523e.png)

```Python
from PySide2.QtGui import QFont

font = QFont('Arial Black')
# font.setFamily('Arial Black')
font.setPointSize(15)
font.setItalic(True)


self.label = QLabel('Test')
self.label.setFont(font)
```

### フォント一覧：QFontComboBox
![image](https://i.gyazo.com/0b4521b8a8c1bff56a1efb9be9211fe6.png)

```Python
self.font_combobox =  QFontComboBox(self)
self.main_layout.addWidget(self.font_combobox)

# QFont取得
font = self.font_combobox.currentFont()
```

## Tips
- Python2用のPySide2の公式ビルドは存在しないと思うのでPython2にPySide2はインストール出来ないと思う。
- 一部VFXツールではツールのPython2用のPySide2ビルドが内包されていて、`Python2+PySide2` という特殊な環境が存在している。
- VSCodeで`jpynb`
    - なんと、JupyterNoteやJupyterLabのファイルはVSCode対応していて、普通に実行や編集が出来る。
    ![](https://i.gyazo.com/bf46de0ea8c982e7642c9c771e0a4358.png)