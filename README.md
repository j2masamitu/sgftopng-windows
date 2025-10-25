# sgftopng for Windows

SGF (Smart Game Format) ファイルを PNG/JPG/GIF 画像に変換する Windows 対応版ツールです。

[English README](#english-readme)

## 概要

sgftopng は、囲碁の棋譜を記録する標準フォーマットである SGF ファイルから、碁盤の図面を生成するツールです。このバージョンは、オリジナルの sgfutils パッケージを Windows 環境で動作するように移植したものです。

## 特徴

- SGF ファイルから PNG/JPG/GIF 画像を生成
- 手数範囲の指定（例: 1-50手目のみ表示）
- 座標の表示/非表示
- 最終局面の表示（手数なし）
- フォントのカスタマイズ
- 碁盤の一部のみを表示（部分図）
- 複数の棋譜・変化図への対応

## 必要な環境

- **Windows 7 以降**
- **ImageMagick 7.x** - 画像生成に必要
  - ダウンロード: https://imagemagick.org/script/download.php#windows
  - または Chocolatey: `choco install imagemagick`

## インストール

1. このアーカイブを適当なフォルダに展開
2. ImageMagick をインストール
3. ImageMagick の `magick.exe` にパスが通っていることを確認

## 使い方

### 基本的な使い方

```bash
# 最もシンプルな例（out.png が作成される）
sgftopng.exe < game.sgf

# 出力ファイル名を指定
sgftopng.exe diagram.png < game.sgf

# 特定の手数範囲を表示（51-100手目）
sgftopng.exe output.jpg 51-100 < game.sgf
```

### よく使うオプション

```bash
# 座標を表示（左右上下すべて）
sgftopng.exe -coordLRTB diagram.png < game.sgf

# 最終局面を手数なしで表示
sgftopng.exe -nonrs final.png < game.sgf

# 1手目から番号を振り直す
sgftopng.exe -from 1 diagram.png 51-100 < game.sgf

# 碁盤の一部だけ表示（左上隅）
sgftopng.exe -view 1-10,1-10 corner.png < game.sgf

# フォントを指定
sgftopng.exe -font "MS Gothic" diagram.png < game.sgf

# 変化図を表示（まず -info で変化図番号を確認）
sgftopng.exe -info < game.sgf
sgftopng.exe -var 2 variation.png < game.sgf
```

### オプション一覧

| オプション | 説明 |
|-----------|------|
| `-info` | 変化図の一覧を表示（図は作成しない） |
| `-game N` | 複数棋譜ファイルの N番目の棋譜を選択 |
| `-var N` | N番目の変化図を表示 |
| `-from M` | 石の番号を M から始める |
| `-nonrs` | 手数を表示せず、最終局面のみ表示 |
| `-coordL` | 左側に座標を表示 |
| `-coordR` | 右側に座標を表示 |
| `-coordT` | 上側に座標を表示 |
| `-coordB` | 下側に座標を表示 |
| `-coordLRTB` | 四辺すべてに座標を表示 |
| `-view R1-R2,C1-C2` | 碁盤の一部を表示（行R1-R2, 列C1-C2） |
| `-font FONT` | フォント名を指定 |
| `-size N` | 画像サイズ（ピクセル）を指定 |
| `-o FILE` | 出力ファイル名を指定（代替方法） |

### 使用例

#### 例1: 定石図を作成

```bash
sgftopng.exe -coordLT -view 1-10,1-10 joseki.png 1-15 < game.sgf
```
左上隅の1-15手目までを、座標付きで表示

#### 例2: 終局図を作成

```bash
sgftopng.exe -nonrs -coordLRTB final-position.png < game.sgf
```
手数なしで最終局面を表示、四辺に座標を表示

#### 例3: 特定の局面を抽出

```bash
sgftopng.exe problem.png 100-120 < game.sgf
```
100-120手目を表示（100手目までは番号なし）

## ビルド方法

実行ファイル（sgftopng.exe）が含まれているため、通常はビルド不要です。

ソースコードからビルドする場合は、`BUILD_WINDOWS.md` を参照してください。

### 簡易ビルド手順

```bash
# MinGW-w64 の場合
gcc -Wall -O3 -o sgftopng.exe sgftopng_windows.c

# または Makefile を使用
mingw32-make -f Makefile.windows
```

## SGF フォーマットについて

SGF (Smart Game Format) は囲碁の棋譜を記録するための標準的なテキストフォーマットです。

### 最小限の SGF 例

```
(;FF[4]GM[1]SZ[19];B[pd];W[dp];B[pp];W[dd])
```

- `FF[4]` - ファイルフォーマットバージョン4
- `GM[1]` - ゲームタイプ: 囲碁
- `SZ[19]` - 碁盤サイズ: 19路盤
- `B[pd]` - 黒石を pd（右上星）に配置
- `W[dp]` - 白石を dp（左下星）に配置

座標は `aa` = 左上隅、`ss` = 右下隅（19路盤の場合）

## トラブルシューティング

### "convert: command not found" または "magick: command not found"

ImageMagick がインストールされていないか、パスが通っていません。

1. ImageMagick をインストール
2. 環境変数 PATH に ImageMagick のインストールフォルダを追加
3. コマンドプロンプトを再起動

### 日本語のファイル名やパスが文字化けする

コマンドプロンプトの代わりに PowerShell を使用するか、パスに日本語を含まないようにしてください。

### 画像が生成されない

1. SGF ファイルが正しい形式か確認
2. ImageMagick が正しくインストールされているか確認
3. `-info` オプションで SGF ファイルが読めるか確認

## ライセンス

このプログラムは GNU General Public License v2 またはそれ以降のバージョンの下で配布されています。

オリジナルの sgfutils パッケージ: https://homepages.cwi.nl/~aeb/go/sgfutils/html/sgfutils.html

## クレジット

- オリジナル sgfutils: Andries Brouwer 氏ほか
- Windows 移植: Junji MASAMITSU

## 関連リンク

- オリジナル sgfutils: https://homepages.cwi.nl/~aeb/go/sgfutils/html/sgfutils.html
- SGF 仕様: https://www.red-bean.com/sgf/
- ImageMagick: https://imagemagick.org/

---

# English README

## Overview

sgftopng is a Windows-compatible tool that converts SGF (Smart Game Format) files into PNG/JPG/GIF images. This version is a port of the original sgfutils package for Windows environments.

## Features

- Generate PNG/JPG/GIF images from SGF files
- Specify move ranges (e.g., show only moves 1-50)
- Show/hide board coordinates
- Display final position without move numbers
- Customize fonts
- Display partial board (corners, specific regions)
- Support for multiple games and variations

## Requirements

- **Windows 7 or later**
- **ImageMagick 7.x** - Required for image generation
  - Download: https://imagemagick.org/script/download.php#windows
  - Or via Chocolatey: `choco install imagemagick`

## Installation

1. Extract this archive to a folder
2. Install ImageMagick
3. Ensure `magick.exe` is in your PATH

## Usage

### Basic Usage

```bash
# Simplest example (creates out.png)
sgftopng.exe < game.sgf

# Specify output filename
sgftopng.exe diagram.png < game.sgf

# Show specific move range (moves 51-100)
sgftopng.exe output.jpg 51-100 < game.sgf
```

### Common Options

```bash
# Show coordinates on all sides
sgftopng.exe -coordLRTB diagram.png < game.sgf

# Show final position without move numbers
sgftopng.exe -nonrs final.png < game.sgf

# Renumber from 1
sgftopng.exe -from 1 diagram.png 51-100 < game.sgf

# Show corner only
sgftopng.exe -view 1-10,1-10 corner.png < game.sgf

# Specify font
sgftopng.exe -font Arial diagram.png < game.sgf
```

### Available Options

| Option | Description |
|--------|-------------|
| `-info` | List variations (don't create diagram) |
| `-game N` | Select game N from multi-game file |
| `-var N` | Display variation N |
| `-from M` | Number stones starting from M |
| `-nonrs` | Don't number stones, show final position only |
| `-coordL` | Show coordinates on left side |
| `-coordR` | Show coordinates on right side |
| `-coordT` | Show coordinates on top side |
| `-coordB` | Show coordinates on bottom side |
| `-coordLRTB` | Show coordinates on all sides |
| `-view R1-R2,C1-C2` | Show partial board (rows R1-R2, cols C1-C2) |
| `-font FONT` | Specify font name |
| `-size N` | Specify image size in pixels |
| `-o FILE` | Specify output filename (alternative method) |

## Building from Source

An executable (sgftopng.exe) is included, so building is not normally required.

To build from source, see `BUILD_WINDOWS.md`.

### Quick Build

```bash
# With MinGW-w64
gcc -Wall -O3 -o sgftopng.exe sgftopng_windows.c

# Or using Makefile
mingw32-make -f Makefile.windows
```

## License

This program is distributed under the GNU General Public License v2 or later.

Original sgfutils package: https://homepages.cwi.nl/~aeb/go/sgfutils/html/sgfutils.html

## Credits

- Original sgfutils: Andries Brouwer et al.
- Windows port: Junji MASAMITSU

## Links

- Original sgfutils: https://homepages.cwi.nl/~aeb/go/sgfutils/html/sgfutils.html
- SGF Specification: https://www.red-bean.com/sgf/
- ImageMagick: https://imagemagick.org/
