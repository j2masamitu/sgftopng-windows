# Changelog

## Version 1.0.1 (2025-10-26)

### 修正 / Fixed

- **[重要] ImageMagick 7.x での色コード解析エラーを修正**
  - Windows環境で "no decode delegate for this image format `'#f2b06d''" エラーが発生する問題を修正
  - ImageMagickコマンド生成時に色コード（背景色 `#f2b06d`）を適切に引用符で囲むように変更
  - 修正箇所: `sgftopng_windows.c` 1205-1215行、1238-1242行
  - 影響範囲: Windows + ImageMagick 7.x の環境のみ

- **[Critical] Fixed ImageMagick 7.x color code parsing error**
  - Fixed issue where "no decode delegate for this image format `'#f2b06d''" error occurred on Windows
  - Added proper quoting for color codes (background color `#f2b06d`) in ImageMagick command generation
  - Modified code: `sgftopng_windows.c` lines 1205-1215, 1238-1242
  - Affected: Windows + ImageMagick 7.x environments only

### 変更 / Changed

- **Windows版の ImageMagick コマンド対応**
  - Windows版は ImageMagick 7.x 標準の `magick` コマンドを使用
  - Unix/Linux版は後方互換性のため `convert` コマンドを継続使用
  - プラットフォーム別コマンド生成に `#ifdef _WIN32` を使用

- **ImageMagick command compatibility for Windows**
  - Windows now uses ImageMagick 7.x standard `magick` command
  - Unix/Linux continues using `convert` for backward compatibility
  - Platform-specific command generation using `#ifdef _WIN32`

### 技術詳細 / Technical Details

**修正前のエラー / Error before fix**:
```
magick: no decode delegate for this image format `'#f2b06d''
draw command failed
```

**根本原因 / Root cause**: Windows の `cmd.exe` が引用符なしの `#` 文字を誤って解釈

**コード変更 / Code changes**:

修正前 / Before:
```c
p += sprintf(p, "convert -size %dx%d xc:%s", bdwidth, bdheight, bgcolor);
```

修正後 / After:
```c
#ifdef _WIN32
    p += sprintf(p, "magick -size %dx%d xc:\"%s\"", bdwidth, bdheight, bgcolor);
#else
    p += sprintf(p, "convert -size %dx%d xc:\"%s\"", bdwidth, bdheight, bgcolor);
#endif
```

**後方互換性 / Backward Compatibility**:
- ✅ Unix/Linux: 変更なし / No changes
- ✅ Windows ImageMagick 6.x: 互換性あり / Compatible
- ✅ Windows ImageMagick 7.x: 修正により動作 / Fixed and working
- ✅ macOS: 影響なし / No impact

**テスト環境 / Tested Environment**:
- Windows 10 + ImageMagick 7.1.2-7 Q16-HDRI x64
- コンパイラ / Compilers: TCC (Tiny C Compiler), MinGW GCC
- テストケース / Test cases: 基本変換、複数手の棋譜、座標表示 / Basic conversion, multi-move games, coordinate display

---

## Version 1.0 (2024-10-25)

### Initial Windows Release

- Windows対応版の初回リリース
- オリジナルsgfutils-0.25のsgftopngをWindows環境に移植

### 変更点

- **Windows互換性の追加**
  - `unistd.h` を Windows の `io.h` に対応
  - `unlink()` を `_unlink()` にマッピング
  - `index()` を標準C関数 `strchr()` に置き換え
  - MSVC/GCC両対応の `NORETURN` マクロを実装
  - Windows用 `strndup()` 実装を追加

- **ビルド環境**
  - MinGW-w64対応
  - Visual Studio対応
  - TCC (Tiny C Compiler)対応
  - Windows用Makefileを追加

- **ドキュメント**
  - 日英併記のREADME作成
  - Windows向けビルド手順書（BUILD_WINDOWS.md）を作成
  - 使用例とトラブルシューティングを追加

### 動作確認環境

- Windows 10/11
- ImageMagick 7.x
- MinGW-w64 gcc 8.1.0以降

### 既知の制限事項

- ImageMagickのインストールが必須
- 日本語のファイルパスで問題が発生する場合あり（PowerShell使用を推奨）

---

## Version 1.0 (2024-10-25)

### Initial Windows Release

- First release of Windows-compatible version
- Port of sgftopng from original sgfutils-0.25 to Windows environment

### Changes

- **Windows Compatibility**
  - Support for `io.h` instead of `unistd.h` on Windows
  - Map `unlink()` to `_unlink()` for Windows
  - Replace `index()` with standard C function `strchr()`
  - Implement `NORETURN` macro for MSVC/GCC compatibility
  - Add `strndup()` implementation for Windows

- **Build Environment**
  - MinGW-w64 support
  - Visual Studio support
  - TCC (Tiny C Compiler) support
  - Add Windows-specific Makefile

- **Documentation**
  - Create bilingual README (Japanese/English)
  - Add BUILD_WINDOWS.md with detailed build instructions
  - Include usage examples and troubleshooting guide

### Tested Environment

- Windows 10/11
- ImageMagick 7.x
- MinGW-w64 gcc 8.1.0 or later

### Known Limitations

- ImageMagick installation required
- Japanese file paths may cause issues (use PowerShell recommended)
