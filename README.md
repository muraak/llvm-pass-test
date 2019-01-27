

# 独自定義パスをライブラリ化して組み込むサンプル

実験として、2の冪乗定数の乗算をシフト演算に置き換えるパスを作成した。

# ライブラリのビルド

各フォルダ階層の`CMakeLists.txt`を記述してcmake->makeしてライブラリをビルドする。
Macの場合、出力は`*.so`となる。

ソースのあるPassフォルダのCMakeListsには以下の変数にインストール済みのllvmに対応するパスをセットする。(Windowsの場合は不要？)

```cmake
set(LLVM_DIR /usr/local/Cellar/llvm/7.0.1/share/cmake/modules)
set(LLVM_INCLUDE_DIRS /usr/local/Cellar/llvm/7.0.1/include)
```

makeまでの流れ。

```
***$ mkdir build
***$ mkdir cd build
***$ cmake ..
***$ make
```

# テスト

以下の流れでテスト。（コマンドは次章を参照。）

- `test`フォルダの`test.c`をllvmビットコードにコンパイル(bc -> ll)
- ライブラリを含むPassで最適化(bc -> bc (through pass))
- ビットコードを実行（変換が不正な場合は例外が発生する）
- IRを確認したい場合は逆変換(bc->ll)

# llvm関連コマンド

注意：clangはライブラリ生成時に指定したllvmのbin内のものを使う！

## c -> bc

```
***$ /usr/local/Cellar/llvm/7.0.1/bin/clang -c -emit-llvm test.c
```

## bc -> bc (through pass)

```
***$ /usr/local/Cellar/llvm/7.0.1/bin/opt -load Pass/libTestPass.so -muraak -o out.bc < test.bc > /dev/null
```

## execute bc

```
***$ /usr/local/Cellar/llvm/7.0.1/bin/lli out.bc
```

## bc -> ll

```
***$ /usr/local/Cellar/llvm/7.0.1/bin/llvm-dis test.bc
```

# 参考
## Web
- [Writing an LLVM Pass](https://llvm.org/docs/WritingAnLLVMPass.html#registering-dynamically-loaded-passes)

- [大学院生のためのLLVM](https://postd.cc/llvm-for-grad-students/)

- [LLVM bitcode基礎知識](https://qiita.com/gamako/items/f37dbb05de9d3832ce6b)

## コード
- ```llvm/lib/Transforms/InstCombine/InstCombineAddSub.cpp```

- https://github.com/sampsyo/llvm-pass-skeleton/blob/master/skeleton/Skeleton.cpp


Happy llvm!<br/>
Muraak.