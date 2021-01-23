# VHDLのシミュレータをインストール

無償のVHDLシミュレータは以下のようなものがある。

* [GHDL](http://ghdl.free.fr/) : よく知られたオープンソースのVHDLシミュレータ。
* [NVC](https://github.com/nickg/nvc) : 比較的新しいオープンソースのVHDLシミュレータ。GHDLより高速な気がする。
* [Modelsim - Intel FPGA Starter Edition](https://www.intel.co.jp/content/www/jp/ja/software/programmable/quartus-prime/model-sim.html) : 業界ではよく使われているHDLシミュレータで、Verilogにも対応する。無償版なので性能はオミットされておりかなり遅い。

## MSYS2

GHDLやNVCをインストールしたりビルドしたりするのに、[MSYS2](https://www.msys2.org/)が必要になる。
MSYS2は、Windows上でUnixライクな開発環境を作るのによく使われている。
もし、GHDLだけを使用するのであればMSYS2のインストールは不要。
似たようなものにCygwinがあるが、若干思想が違う。

基本的には、[MSYS2](https://www.msys2.org/)の指示通りにインストールすれば良い。
インストールしたら、コマンドプロンプトなどでもMSYS2の各種コマンドが使用できるように、PATHを通しておく。

```bat
setx /M PATH "%PATH%;C:\msys64\usr\bin;C:\msys64\mingw64\bin"
refreshenv
```


## GHDL

[MSYS2](https://www.msys2.org/)がインストールされていれば以下コマンドでインストールできる。ただし、依存関係が多いのでダウンロードに時間がかかる。
インストール後、MSYS2環境のPATH設定をしてあれば、コマンドプロンプトなどでも`ghdl`と打てば実行可能になっているはず。

```sh
pacman -S mingw-w64-x86_64-ghdl-llvm
```

MSYS2がインストールされていない場合は、[Githubのリリースページ](https://github.com/ghdl/ghdl/releases)にバイナリが用意されているのでそれを使う。
Windows 64bitの場合は`ghdl-0.37-mingw64-llvm.zip`を選んでおけば良い。
ダウンロードしたら、中の`bin`フォルダにPATHを通しておく。


## NVC

MSYS2環境で[Githubの指示](https://github.com/nickg/nvc#windows)通りにビルド、インストールする必要がある。
ただし、bash環境下である必要があるので、少し注意が必要。

まずは、必要なパッケージをインストールする。これはコマンドプロンプトなどの環境でも可能。

```bat
pacman -S base-devel mingw-w64-x86_64-{llvm,ncurses,libffi,check,pkg-config} git
```

次に、コマンドプロンプトなどで`bash`と打ってbash環境に入る。以下のように表示されればbash環境に入れている。

```
C:\Users\User>C:\tools\msys64\usr\bin\bash.exe

User@WinDev2101Eval  /c/Users/User
$
```

この状態で、ビルド作業を行う。必要があれば`cd`などで事前に作業用フォルダなどに移動しておくこと。
以下のコマンドでGithubがからコードを取ってきてビルド、インストールすることができる。

```sh
git clone https://github.com/nickg/nvc
cd nvc
./autogen.sh
mkdir build && cd build
../configure
make install
```


## Modelsim - Intel FPGA Starter Edition

アカウントを登録して、ダウンロードしてインストールするだけ。
デフォルトでPATHも通してくれる模様。
