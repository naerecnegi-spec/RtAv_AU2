# RtAv_AU2# RtAv_AU2 パッケージ

REAPERのプロジェクトファイル（.rpp）を読み込んで、AviUtl2側で素材の配置・反転・移動・回転などを同期させるスクリプト集 **RtAv_AU2** の配布パッケージです。

RtAv_AU2 は、AviUtl（初代）向けの **RtAv2（by 7539）** を AviUtl2（64bit）向けに移植したものです。従来のDLLを廃止し、純粋なLua実装に置き換えています。

## 同梱内容

```
RtAv_AU2_package/
├─ README.md                ← このファイル
├─ RtAv_AU2/
│  ├─ RtAv_AU2.lua             RPP解析モジュール本体
│  ├─ RtAv_AU2Function.lua     共通関数ライブラリ
│  ├─ @RtAv_AU2.anm2            基本（rpp読み込み）
│  ├─ @RtAv_AU2(読込系).anm2    オブジェクト書き換え系
│  ├─ @RtAv_AU2(描画系).anm2    描画系（両端クリッピング等）
│  └─ @RtAv_AU2(エフェクト).anm2 反転・座標等のエフェクト系
└─ aviutl2_sdk_mirror/       AviUtl2 SDKのミラー（開発・改変時の参照用）
```

## 動作環境

- AviUtl2（64bit）
- LuaJIT の FFI が使える環境（日本語パス対応のファイル読み込みに使用）

## 導入方法

`RtAv_AU2/` フォルダ内の6ファイルを、AviUtl2のスクリプト配置フォルダへコピーしてください。
.lua の2ファイル（`RtAv_AU2.lua` / `RtAv_AU2Function.lua`）は .anm2 から `require` で読み込まれるため、必ず同じ場所に置いてください。

## 基本的な使い方

1. REAPERで素材を配置し、プロジェクトを .rpp 形式で保存する
2. AviUtl2で「rpp読み込み」（@RtAv_AU2.anm2）に、その .rpp ファイルを指定する
3. 必要に応じて、読込系・描画系・エフェクト系のスクリプトをオブジェクトに適用する

詳しい使い方・パラメータの意味は各 .anm2 ファイル冒頭のコメントと、以下のドキュメントを参照してください。

- ドキュメント: https://scrapbox.io/nanikani-shugo/RtAv_AU2

## 実装メモ（RtAv2からの主な変更点）

- **DLL廃止・純Lua化**: `RtAv_AU2.lua` が .rpp の解析（TEMPO / TRACK / ITEM の POSITION・LENGTH・FILE・PLAYRATE）を担う
- **日本語パス対応**: LuaJIT FFI 経由で Win32 のワイド文字ファイルAPI（`_wfopen`）を使い、UTF-8 / CP932 のパスでもファイルを開けるようにした
- **周期パターン**: アイテムごとの音程・長さの周期指定は `RtAv_AU2Function.lua` 側で処理する（RtAv2の仕様を継承）
- **デバッグログ**: 読み込み時に `C:\ProgramData\aviutl2\Log\rtav2_debug.log` へ解析結果（テンポ・トラック数・アイテム数）を出力する

## バージョン

- RtAv_AU2.lua: V1.2.1
- 2026-11-01: .anm2 ファイル内のコメント文字化けを修正（コードには変更なし）

## クレジット

- 原作: RtAv2 by 7539（2019/03/25 v1-1.00 配布、2021/08/10 v2-1.00 完成）
- 移植: RtAv_AU2（AviUtl2対応版）

## aviutl2_sdk_mirror について

`aviutl2_sdk_mirror/` は、AviUtl2 SDK の有志ミラー（メンテナー: @sevenc-nanashi）の同梱コピーです。RtAv_AU2 の開発・改変時にSDKのヘッダやサンプルを参照するためのもので、利用規約は同梱の `license.txt`（AviUtl ExEdit2 Plugin SDK License / MIT License）に従います。

- 出典: https://github.com/aviutl2/aviutl2_sdk_mirror
- 取得時点のコミット: b5a4349febe81341c5ee25750538830bc39d0b9c（2026-10-31）
