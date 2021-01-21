# Pythonのチュートリアル

## 環境構築

### Windowsの場合

Windowsの場合、[Python 3.8.7](https://www.python.org/downloads/release/python-387/)の下のほうにある、`Windows installer (64-bit)`からインストーラーをダウンロードしてPythonのランタイムをインストールする。
現時点(2021/1/20)での最新版は3.9系だが、別途インストールするパッケージ(ライブラリ)が対応していなかったりするトラブルが発生する可能性があるので、ひとつ前の3.8系のものをインストールするのが良い。

インストールしたら環境変数`PATH`に`C:\Python38`と`C:\Python39\Scripts`があることを確認する。
確認の方法は、コマンドプロンプトなら以下のコマンドで確認できる。(`> `は入力不要で、コマンドの入力部分を示す)
PowerShellを使用している場合は`echo $env:PATH`で同等の表示ができる。

```bat
> echo %PATH%
C:\Python38\Scripts\;C:\Python38\; ... (なんかたくさん表示される)
```

このコマンドは環境変数`PATH`の内容を表示するもので、`PATH`は`;`を区切りにして複数のディレクトリを指定している。
このため、通常はPython以外のたくさんのディレクトリが`;`で区切って表示される。
この環境変数にディレクトリを追加することを、PATHを通すという。

念のため、コマンドプロンプトで以下のコマンドを打って、変更されたPATHを反映させる。

```bat
> refreshenv
```

PATHが通って入ればPythonを使う準備は完了で、以下のコマンドでインストールされたPythonのバージョンを確認することができる。

```sh
> python --version
Python 3.8.7
```

#### エディタ

Pythonスクリプトのコードを書くのに、メモ帳ではない高機能なエディタを使用すると効率が上がる。
最近は[Visual Studio Code(VSCode)](https://azure.microsoft.com/ja-jp/products/visual-studio-code/)を入れておけば大丈夫という風潮。
社内規定などでインストールして使うのに不都合があればポータブル版もある。


#### Pythonスクリプトの実行

以下の1行だけのファイルを作成して、`hello.py`という名前で保存する。

```python
print("Hello.")
```

コマンドプロンプトなどで、保存したファイルの場所に移動して以下のコマンドを打つと`Hello.`が表示されるはず。
場所の移動は`cd`で行う。

```sh
python hello.py
```

#### pipパッケージのインストール

VPNや社内ネットワークなど、プロキシ環境下の場合は事前に付属の`pip_proxy_on.bat`を参考にプロキシの設定を行う必要がある。
ユーザ名やパスワードは適宜変更が必要だが、記号類はURLエンコードする必要があることに注意すること。
例えばユーザ名に`@`が含まれる場合は当該部分を`%40`に置き換える必要がある。

Pythonのパッケージ(ライブラリ)は基本的にpipというコマンドで管理されており、このコマンドによってWeb上の各種パッケージをインストールすることができる。
このチュートリアルで使用する予定のパッケージを以下のコマンドでインストールしておく。パッケージのインストールにはデフォルトで管理者権限が必要。
環境を汚したくない場合は別途`venv`などを使って仮想環境を作っておくとよい。`venv`については[venv: Python 仮想環境管理](https://qiita.com/fiftystorm36/items/b2fd47cf32c7694adc2e)などを参照のこと(検索で一番上に出てきた記事)。

```sh
pip install --upgrade pip
pip install numpy pandas matplotlib pylint lxml
```

最初の行は、pip事態のアップグレードをしている。
二行目でパッケージをインストールしている。

* [NumPy](https://numpy.org/): 数値計算用のパッケージのデファクトスタンダード。
* [pandas](https://pandas.pydata.org/): データ解析用パッケージのデファクトスタンダード。
* [Matplotlib](https://matplotlib.org/): Pythonでのグラフ表示用のパッケージ。他にもモダンなグラフ表示パッケージはあるが、バックエンドはMatplotlibだったりする。
* [Pylint](https://www.pylint.org/): Pythonの文法チェック(リント)を行うツール。入れておくとVSCodeなどのエディタで自動的に文法チェックしてくれる。
* [lxml](https://lxml.de/): pandasでHTMLのスクレイピングをするときに必要になるやつ。直接は使わない。

## 気象庁データのスクレイピング

### 前提

* ここでやりたいこと
    + 気象庁の過去の気象データをスクレイピングして、テキスト保存
    + 気象庁の過去の気象データをスクレイピングしてグラフ表示
    + 余裕があれば[OpenWeather](https://openweathermap.org)の予報データをAPI経由で取得して、グラフ表示

スクレイピングする対象は、以下のものにするがみだらにアクセスしないように注意。

* [東京(トウキョウ) 毎正時の観測データ (当日)](https://www.jma.go.jp/jp/amedas_h/today-44132.html?areaCode=000&groupCode=30)
* [東京(トウキョウ) 毎正時の観測データ (昨日)](https://www.jma.go.jp/jp/amedas_h/yesterday-44132.html?areaCode=000&groupCode=30)

OpenWeatherについてはAPIキーの取得が必要だが、キーの発行は登録から数時間かかるので注意。


### 実装

現時点ではヒントのみを記載しておく。各自コーディングする。(なんか同じ人のブログばっかりひっかかるが知り合いではない)

* ~~[Python, pandasでwebページの表（htmlのtable）をスクレイピング](https://note.nkmk.me/python-pandas-web-html-table-scraping/)~~ ここのコードは、`html5lib`と`beautifulsoup4`というパッケージに依存している
* [Python用データ分析ツールpandasのread_html関数の使い方](https://shirabeta.net/How-To-Use-pandas.read_html.html) プレーン(?)なpandas(+lxml)だけでスクレイピングする例
* [pandasでcsvファイルの書き出し・追記（to_csv）](https://note.nkmk.me/python-pandas-to-csv/)
* [pandasのplotメソッドでグラフを作成しデータを可視化](https://note.nkmk.me/python-pandas-plot/)

OpenWeatherのデータ取得はスクレイピングではないので、`urllib.request`などを使用してAPIアクセスする必要がある。
基本的には以下の流れでデータの取得、pandasデータフレームへの変換ができる。
コードは、[weather_data.py](https://github.com/nv-h/i2c_env_sensors/blob/master/openweathermap/weather_data.py)が参考になる。
予報データ取得のためのAPIの説明は[5 day weather forecast](https://openweathermap.org/forecast5)あたりに記載されている。

```python
import pandas as pd

# 標準ライブラリのインポート(pipでのインストール不要)
import urllib.request
import json

URL = "http://api.openweathermap.org/XXXXX" # 適切なURLを記入、ここでAPIキーも必要

json_obj = None

# GETで取得した内容をjsonに変換
# jsonにするのはデバッグ時や保存時に見やすくなるため
request = urllib.request.Request(URL, method='GET')
with urllib.request.urlopen(request) as response:
    json_obj = json.loads(response.read().decode("utf-8"))

# どんな形か見たければ、print()で簡易的に確認できる。
print(json_obj)

# グラフ表示に向けてpandasデータフレーム形式に変換
pd.json_normalize(json_obj['list'])
```
