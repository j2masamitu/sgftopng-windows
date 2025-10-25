# Windows版 sgftopng ビルド手順

## 準備したファイル

- `sgftopng_windows.c` - Windows対応版のソースコード
  - `unistd.h` を Windows の `io.h` に置き換え
  - `index()` 関数を標準の `strchr()` に置き換え
  - `__attribute__((noreturn))` を MSVC/GCC 両対応のマクロに変更

- `Makefile.windows` - Windows用のMakefile

## ビルド方法

### 方法1: MinGW-w64を使用（推奨）

1. MinGW-w64をインストール:
   - [公式サイト](https://www.mingw-w64.org/)からダウンロード
   - または、管理者権限でChocolateyを使用: `choco install mingw -y`

2. ビルド:
   ```bash
   gcc -Wall -O3 -o sgftopng.exe sgftopng_windows.c
   ```

   または、Makefileを使用:
   ```bash
   mingw32-make -f Makefile.windows
   ```

### 方法2: Visual Studio Build Toolsを使用

1. Visual Studio Build Toolsをインストール（既にVisual Studioがインストールされている場合は不要）

2. "Developer Command Prompt for VS"を開く

3. ビルド:
   ```cmd
   cl /W3 /O2 /Fe:sgftopng.exe sgftopng_windows.c
   ```

### 方法3: TCC (Tiny C Compiler)を使用（最軽量）

1. [TCC](https://bellard.org/tcc/)をダウンロード

2. ビルド:
   ```bash
   tcc -o sgftopng.exe sgftopng_windows.c
   ```

## 実行方法

ビルド後、ImageMagickの`convert`コマンドがPATHに含まれていることを確認してください。

```bash
# 基本的な使い方
sgftopng.exe < game.sgf

# 出力ファイル名を指定
sgftopng.exe diagram.png < game.sgf

# 特定の手数範囲を表示
sgftopng.exe output.jpg 51-100 < game.sgf
```

## 必要な実行時依存関係

- **ImageMagick**: `convert`コマンドが必要
  - インストール: https://imagemagick.org/script/download.php#windows
  - または: `choco install imagemagick -y`

## トラブルシューティング

### "convert: command not found"エラー

ImageMagickがインストールされていないか、PATHに含まれていません。

1. ImageMagickをインストール
2. 環境変数PATHにImageMagickのインストールディレクトリを追加

### コンパイルエラー

- MinGWの場合: `-D_WIN32`フラグを追加してください
- MSVCの場合: `/D_WIN32`フラグを追加してください

## オリジナルとの違い

Windows対応のために以下の変更を行いました:

1. `#include <unistd.h>` → `#include <io.h>` (Windows環境)
2. `unlink()` → `_unlink()` (Windows環境)
3. `index()` → `strchr()` (標準関数)
4. `__attribute__((noreturn))` → プラットフォーム別マクロ

これらの変更により、Linux/Unix版とWindows版の両方で動作するようになっています。
