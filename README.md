# DigitShowSystem

東京大学の地盤研で使用されている、三軸試験機向けの計測・制御システム **DigitShow 系** の関連リポジトリをまとめたポータルリポジトリです。

## そもそもDigitShowBasicとは
Windows98時代に 東京大学 本郷地盤研 博士卒 の 中央開発株式会社 王 林(LIN Wang)さん が作成した DigitShow を参考に、  
Windows2000時代に 東京大学 本郷地盤研 助手 本多 剛(HONDA Tsuyoshi)さんが作成したのが DigitShowBasic です。  
当初はInterfaceとCONTECのISA/PCI/PCI ExpressのAD/DAボードをターゲットとしたアプリでしたが、  
USB接続の自作Modbusボードに最低限のAD/DAを搭載することで問題を解消しています。  
時代とともにPCの小型化と拡張スロットがほぼGPU用途になり、Slot数が減少したため、  
制御装置および計測装置の置き換えが困難になってきているのがこのシステムの問題です。  

## システム概要

DigitShow 系システムは、センサー(ロードセル・変位計など)の値を AD/DA ボードで読み取り、Windows PC 上のソフトウェアで計測・記録・フィードバック制御を行うシステム群です。
現在の主力は、独自開発の **Modbus RTU ボード** と、Windows VC++/MFC 製の高機能アプリ **DigitShowModbus** の組み合わせです。

```
[センサー] ──▶ [Modbus RTU AD/DAボード] ──USB/RS-485──▶ [Windows PC]
 ロードセル      (PCB + ファームウェア)                  DigitShowModbus
 変位計 など     dsm_*_pcb / dsm_*_firmware              (計測・制御・記録)
```

* **ハードウェア(PCB)** … KiCAD で設計した Modbus RTU AD/DA ボード(AI 16ch / AO 8ch)。バージョン世代ごとに v1 Trio、v2 Quartet、v3 Yamanin、v4 Milia があります。
* **ファームウェア** … 各ボード世代に対応する Modbus RTU スレーブファームウェア。
* **ホストアプリ** … ボードと通信して計測・制御・データ保存を行う Windows アプリ(DigitShowModbus / DigitShowBasic 系)。
* **周辺ツール・ドキュメント** … ドキュメント、遠隔監視 Web アセット、データ可視化用 fork ツールなど。

## リポジトリ一覧(職務版: mkt-kuno)

### ハードウェア・ファームウェア(非公開)

| リポジトリ | 対応 | 内容 | 公開範囲 |
|---|---|---|---|
| [dsm_quartet_firmware](https://github.com/mkt-kuno/dsm_quartet_firmware) | v1〜v2  | Trio / Quartet Modbus ボード 用ファームウェア | 非公開 |
| [dsm_quartet_pcb](https://github.com/mkt-kuno/dsm_quartet_pcb) | v1〜v2 | Trio / Quartet Modbus ボードの KiCAD PCB プロジェクト | 非公開 |
| [dsm_milia_firmware](https://github.com/mkt-kuno/dsm_milia_firmware) | v3〜v4 | Yamanin / Milia Modbus ボード用ファームウェア | 非公開 |
| [dsm_yamanin_pcb](https://github.com/mkt-kuno/dsm_yamanin_pcb) | v3 | Yamanin Modbus ボードの KiCAD PCB プロジェクト | 非公開 |
| [dsm_milia_pcb](https://github.com/mkt-kuno/dsm_milia_pcb) | v4 | Milia Modbus ボードの KiCAD PCB プロジェクト | 非公開 |

### ホストアプリ

| リポジトリ | 内容 | 状態 | 公開範囲・ライセンス |
|---|---|---|---|
| [DigitShowModbus](https://github.com/mkt-kuno/DigitShowModbus) | 高機能アプリ。Modbus ボード用の計測・制御アプリ(Windows VC++ MFC) | Main | **非公開**・独自ライセンス(UT-DSM License) |
| [DigitShowBasic](https://github.com/mkt-kuno/DigitShowBasic) | Modbus ボードではなく CONTEC AD/DA ボード用の計測・制御アプリ(Windows VC++ MFC) | Legacy | 公開・**GPLv3** |
| [DigitShowBasicTS](https://github.com/mkt-kuno/DigitShowBasicTS) | Modbus ボードではなく CONTEC AD/DA ボード用の計測・制御アプリ、中空ねじり用(Windows VC++ MFC) | Legacy | 非公開・**GPLv3**(予定) |
| [DigitShowBasicM](https://github.com/mkt-kuno/DigitShowBasicM) | DigitShowBasic の AD/DA 部分だけを Modbus ボードに差し替えた計測・制御アプリ(Windows VC++ MFC) | Stable | 公開・**GPLv3** |
| [DigitShowSide](https://github.com/mkt-kuno/DigitShowSide) | 試作アプリ。Modbus ボード対象 Qt PySide検証用アプリ (Windows Python Qt)| Develop |**非公開**・ライセンス未定|

### ドキュメント・周辺ツール

| リポジトリ | 内容 | 公開範囲・ライセンス |
|---|---|---|
| [DigitShowDoc](https://github.com/mkt-kuno/DigitShowDoc) | DigitShowModbus 用の Markdown 説明書ドキュメント | 公開 |
| [DigitShowWebview](https://github.com/mkt-kuno/DigitShowWebview) | DigitShowModbus 用の研究室内ネット遠隔監視用 Web アセット | 公開・**LGPLv3** |
| [ChartCtrl](https://github.com/mkt-kuno/ChartCtrl) | DigitShowModbus v4.4 以降で使用している MFC 用 Plot/Chart ライブラリ。CodeProject 由来で、元公開ページは消滅 | 公開・**CPOL** |
| [csvtk](https://github.com/mkt-kuno/csvtk) | ヒストリカルループの Plot を実現するための fork 版。DigitShowModbus v4.3 まで対応 | 公開・**MIT**(上流: shenwei356/csvtk) |
| [ADS1115_WE](https://github.com/mkt-kuno/ADS1115_WE) | Read-Modify-Write(I2C バスの占有問題) 改善版。 ADS1115 ライブラリのfork | 公開・**MIT**(上流: wollewald/ADS1115_WE) |

## コラム: 趣味版(modbus_simple シリーズ)

職務版とは別に、個人アカウント([@KikuchiMakoto](https://github.com/KikuchiMakoto))で開発している **趣味版** もあり、こちらも結構いい感じです。
材料試験向けの自作 Modbus RTU 計測・制御システムで、**アナログ入力 16ch・出力 8ch** をブラウザだけで記録・制御できます(Chrome / Edge、インストール不要)。

```
[センサー] ──▶ [計測基板] ──▶ [Arduino Nano R4] ──USB──▶ [ブラウザ]
 ロードセル      HX711 ×8        Modbus RTU スレーブ        グラフ・保存
 変位計          ADS1115 ×2                                 Python 制御
                 GP8403 ×4
```

| リポジトリ | 内容 | ライセンス |
|---|---|---|
| [modbus_simple_system](https://github.com/KikuchiMakoto/modbus_simple_system) | **トップページ**。システム全体の説明・ドキュメントはこちら(README を読めば分かります) | 各リポジトリのライセンスに従う |
| [modbus_simple_pcb](https://github.com/KikuchiMakoto/modbus_simple_pcb) | 計測基板の KiCAD データ・ガーバー | 公開 |
| [modbus_simple_firmware](https://github.com/KikuchiMakoto/modbus_simple_firmware) | Arduino Nano R4 用 AD/DA ボード firmware(HX711 / ADS1115 / GP8403 / ModbusRTUSlave) | 公開・**GPLv3** |
| [modbus_simple_logger](https://github.com/KikuchiMakoto/modbus_simple_logger) | ブラウザ上で動作する Modbus RTU ロガー(SPA / PWA)。Web Serial API で接続し、リアルタイム計測・キャリブレーション・チャート表示・TSV 保存が可能 | 公開・**MIT** |

次世代アプリの方向性については [次世代アプリの Issue](https://github.com/mkt-kuno/DigitShowSystem/issues/3) で議論しており、趣味版の TypeScript 製ロガー([modbus_simple_logger](https://github.com/KikuchiMakoto/modbus_simple_logger))を第一の柱と捉えています。

## コラム: センサーの知見

生研式三軸試験機に搭載されるセンサー(ロードセル・変位計・差圧発信器・電空レギュレータ等)について、**方式と信号インターフェースの単位で**整理した技術メモを **[SENSORS.md](./SENSORS.md)** にまとめています。型番は載せずに「原理 × 出力形式 × 受け方」で記述しているので、製造中止・代替調達に左右されない知識として使えます。保守・改造・新規構築時の一次資料です。
