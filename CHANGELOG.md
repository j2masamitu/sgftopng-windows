# Changelog

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
