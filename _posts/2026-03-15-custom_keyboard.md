---
title: 自作キーボードを作製した
data: 2026-03-15
categories: Electroics
---
## Introduction
キーボードを作りたくなった。<br>

私の仕事を抽象化すれば、キーボードを叩く毎日なのだが、ノートPCのパンタグラフ式キーボードは打鍵音が高くてうるさい。<br>

自作キーボードには以前から興味をもっていた。ただ、回路設計が面倒くさくいから、やる気が起きなかった。<br>だが、転職して組み込み半導体に関する仕事をするようになったので、仕事の勉強の一環として、これを機に自作することにした。<br><br>

---
## 1. 構想と勉強
### 1.1. キー配置
理想のキー配置を考えるが、型にはまった思考しかできないので、結局ありきたりなISO60%になった。守破離の守ってことで。<br>

60%レイアウトは、特殊キー（矢印、page upなど）とファンクションキーが無い分、コンパクトな構成だ。ただ、矢印キーはコーディングで頻繁に使うので、スペースキーの幅を減らして、キースペースを確保した。<br>
<img src="/img/0315_01.png" width="100%" height="auto"><br>

キー配置は[keyboard-layout-editor.com](https://www.keyboard-layout-editor.com/)（kle）でデザインを作成した。後の基板設計でこのデータを使うので、ページ右上の[Download]からJSONファイルをダウンロードしておく。<br><br>

### 1.2. 仕様検討と部品選定
基板設計に向けたハードウェア仕様と、ファームウェア自作に向けたソフトウェアの仕様を決める。大層な話ではないが、参考までに。<br>

#### 1.2.1. HW
60%レイアウトの有線キーボード。サイズはThinkPadの横幅に揃えたい。<br>

電子部品はJLCPCBのPCBAサービスで購入できる部品の中から選定した。
* USB: **U262-161N-4BVC11**
    * USB-Cのレセプタクル
    * この世の全てのケーブルをtype-Cで統一したい
* LDO: **LM3940IMPX-3.3/NOPB**
    * USB電源からMCU電源を生成する低損失のLDO
    * 出力電流は1Aもいらいないと思うけど、ヒートシンク無しだから余裕をもって
* ESD: **ESD122DMXR**
    * USBのデータラインのESD対策
* Schottky Diode: **SS34**
    * USB電源（VBUS）への逆流防止用
* MCU: **STM32F072VBT6**
    * 外付け水晶振動子（48MHz）無しでUSB機能を使える
    * ピン数が多いパッケージ（LQFP-100）なのでパターン設計が楽（&larr;めっちゃ大事！）
    * 自作キーボード用ファームウェア QMKに対応（参考: [QMK](https://docs.qmk.fm/compatible_microcontrollers)）
* Diode: **1N1418W**
    * キーマトリクス用の整流ダイオード
<br>

#### 1.2.2. SW
ファームウェアは[QMK](https://docs.qmk.fm/)を使って楽したい。<br>キー配置のjsonを作成してビルドすれば、FWが簡単に作れるらしい。最高やん！<br>

あと、せっかくの自作キーボードだから、マクロ機能を追加したい。特定のキーを押したら、会社のシステムへのログインが一発で完了するとか、ゲームで一連の連続動作を実行するとか。<br>QMKは自作コードのマクロ追加も簡単にできる。（参考: [QMK](https://docs.qmk.fm/feature_macros)）
<br><br>

### 1.3. USB機器の勉強
* [Introduction to USB with STM32](https://wiki.st.com/stm32mcu/wiki/Introduction_to_USB_with_STM32#What_is_the_Universal_Serial_Bus_-28USB-29-3F)<br>USBの概要を学べる良いページ。第1章を読めば、USB開発のキーワードを把握できるのでオススメ。
    * **ディスクリプタ**<br>USB機器の機器情報や仕様に関するデータ構造体。ホスト（PC）はディスクリプタをもとに、接続されたUSB機器と通信を行う。
    * **エンドポイント**<br>送受信するデータ用のバッファメモリ。ホストはUSB機器との通信をポーリング周期間隔で行うので、送りたいデータはエンドポイントへ一時的にデータを格納しておく。
* [How to implement a USB device custom HID class on STM32](https://community.st.com/t5/stm32-mcus/how-to-implement-a-usb-device-custom-hid-class-on-stm32-part-1/ta-p/49423)<br>STM32でのUSB開発チュートリアル
* [USB Type-C コネクタに必要な終端抵抗](https://community.infineon.com/t5/%E3%83%8A%E3%83%AC%E3%83%83%E3%82%B8%E3%83%99%E3%83%BC%E3%82%B9%E3%82%A2%E3%83%BC%E3%83%86%E3%82%A3%E3%82%AF%E3%83%AB-KBA/USB-Type-C-%E3%82%B3%E3%83%8D%E3%82%AF%E3%82%BF%E3%81%AB%E5%BF%85%E8%A6%81%E3%81%AA%E7%B5%82%E7%AB%AF%E6%8A%B5%E6%8A%97-Community-Translated-JA/ta-p/249400?utm_source=copilot.com#.)<br>
* [D+ピンにプルアップは必要ですか？](https://www.macnica.co.jp/business/semiconductor/support/faqs/silicon_labs/109673/)<br>USBのデータ線のプルアップ抵抗と転送速度の関係について
<br><br>

## 2. ハードウェア作製
### 2.1. 部品入手
[遊舎工房](https://shop.yushakobo.jp/)でキースイッチとキーキャップを調達する。<br>
"[LeleLab Supsup SuperX White Keycap](https://shop.yushakobo.jp/products/10706?variant=51141650972903)"というスケルトンのキーキャップが気に入ったので、これに合うキースイッチを選んだ。<br>
キースイッチとキーキャップ、スタビライザーを購入して、合計13,694円。<br><br>

### 2.2. PCB作製
KiCADで回路図とPCBを設計して、[JLCPCB](https://jlcpcb.com/)で基板を作製してもらった。<br>

KiCADを触るのは8年ぶりなので、使い方の勉強から始める。調べながらやっていくよりも体系的にまとまった資料を読んだ方が早いので、適当な入門書を借りてHands-Onした。<br><br>

#### 2.2.1. 回路設計
回路図を設計する。<br>

キーボードの回路はシンプルなので、自作キーボード界隈の先人の回路図を読めば、必要な機能を把握できる。<br>私の作成した回路図も[github](https://github.com/h4rutank/CrazySpring_KB/blob/main/schematic.pdf)にあげているので、参考になれば幸いです。<br>

回路の主な特徴は
* **USB-Cレセプタクル**<br>USBの仕様で、22Ωのダンピング抵抗やCCラインのプルダウン抵抗、<u>データ線のプルアップ抵抗</u>が必要。（参考: [STM community](https://community.st.com/t5/stm32-mcus-products/stm32h7-and-usb-c-schematics/td-p/714872)）
* **電源回路**<br>USBのVBUS（5V）&rarr; ショットキーバリアダイオード（SBD）&rarr; LDO &rarr; MCU電源（3.3V）
* **ESD**<br>静電気対策でUSBのデータ線にはESD保護を付けている。USBでなくても、外部I/Fをもつ信号ラインにはESDは必須。
* **キーマトリクス回路**<br>[ikejiのblog](https://blog.ikejima.org/make/keyboard/2019/12/14/keyboard-circuit.html)を参考にしてください
<br>

#### 2.2.2. PCB設計
次に、PCBを設計する。<br>

* **キースイッチのlibrary**<br>[foostan/kbd](https://github.com/foostan/kbd)をインポートする。<br>回路設計の嫌いな作業ランキング1位「部品ライブラリの作成」を回避できます。
* **kicad-kbplacer**<br>[kicad-kbplacer](https://github.com/adamws/kicad-kbplacer?tab=readme-ov-file#how-to-use-from-kicad)というプラグインをインストールする。<br>kleで作成したキー配置のJSONファイルから、PCB上でキースイッチを自動配置してくれるツール。<br>回路設計の嫌いな作業ランキング同率1位「PCBの部品配置」を回避できます。
* **デザインルール**<br>PCBを発注するならば、基板メーカーの[設計ガイドライン](https://jlcpcb.com/capabilities/pcb-capabilities)に準拠する必要がある（参考: [KiCad](https://docs.kicad.org/9.0/en/getting_started_in_kicad/getting_started_in_kicad.html#board_setup_and_stackup)）
<br>

回路設計の嫌いな作業ランキング同率1位「パターン設計」は回避できません。数が多いから面倒くさい。<br>
<img src="/img/0315_02.png" width="90%" height="auto">
<br><br>

#### 2.2.3. PCB発注
KiCADの設計データをもとに、JLCPCBへ発注する。<br>

JLCPCBはPCBを作るだけでなくて、そのPCBに電子部品をはんだ付けしてくれる**PCBAサービス**をやっている。<br>自分でチップコンデンサやICを購入して、はんだ付けするコストを考えると、JLCPCBに任せる方が安上がりなので、今回はPCBAサービスを利用する。<br>

PCBAサービスの利用方法は、[JH4VAJさんのブログ](https://www.jh4vaj.com/archives/33375)を参考にしてください。<br>KiCADでガーバデータとBOMを作成したら、JLCPCBの注文画面でアップロードするだけ。<br><br>

参考までに、PCBの作成が\$20.6、PCBアセンブリサービスが\$101.34で、送料とクーポンを合わせて\$126.14（=20,725円）だった。（財布が）厳しいって。<br><br>

注文して2日で出荷。仕事が早すぎる。<br>
<img src="/img/0315_03.png" width="90%" height="auto">
<br>
届いたPCBにキースイッチと電解コンデンサをはんだ付けして、ハードウェアの回路部分は完成。<br><br>

### 2.3. ケース作製
Fusion360で設計して、[JLC3DP](https://jlc3dp.com/)で3Dプリントしてもらった。<br>

画像のようなキーボードの土台となる部分を印刷してもらう。金欠だから、上側のカバーは無い。<br>
<img src="/img/0315_04.png" width="50%" height="auto"><br>
印刷方式はSLAで材料は9600 Resin、値段は\$24.75。送料と割引クーポンを合わせて\$35.28（=5,737円）だった。<br>

注文時期がLunar New Yearとかぶってしまったので、2週間ほどかかったが、平常時であれば1週間で出荷するらしい。便利な世の中になったな～。<br>
<img src="/img/0315_08.png" width="80%" height="auto">
<br>

そして、完成したキーボードがこちら。<br>
<img src="/img/0315_05.jpeg" width="100%" height="auto"><br>
<img src="/img/0315_06.jpeg" width="100%" height="auto">
<br><br>

## 3. ソフトウェア作成
QMKを使いたい。

### 3.1. QMK
* 開発環境: Ubuntu 22.04
* Debugger Probe: ST-LINK v2
    * MCUへのFW書き込み・デバッグ用
<br><br>

### 3.2. 自作ファームウェア

* 開発環境: Windows 11
* IDE: STM32cubeIDE v1.18.1
    * 最新バージョンではGUIのコンフィグツールSW4STM32がIDEと別々になって、使いにくいので昔のバージョン
* Debugger Probe: ST-LINK v2

👷‍♂️👷‍♂️👷‍♂️ 気が向いたら書きます 👷‍♂️👷‍♂️👷‍♂️<br><br>

## 4. つまづき
> いろいろ多分、思うようにはならへん。
><div style="text-align: right;">&mdash; 岡林風穂『キャットファイト』</div>


Ubuntu PCに挿して使おうとすると、なぜかキー入力をできない。何回も挿しなおしたら、たまに入力できるけど。<br>
ターミナルで`lsusb`コマンドを使って、PCに接続されているUSB機器一覧を表示すると、確かに表示されている。キーボードの電源LEDも光ってるから、USBコネクタの接触不良ではなさそう。なぜだろう。<br>試しに、別のWindows PCへ接続すると、キーボードが認識されない。デバイスドライバに表示されてないじゃん。<br>

FWの問題かもしれないと考えて、QMKから自作ファームウェアに変更した。しかし、状況は変わらず。<br><br>

ようやく「ハード側に問題があるかもしれない」と気づき、USB周辺の回路図を見直す。<br>

CCやMCUとの接続は問題がなさそうなので、クロックに問題があるかもしれない。<br>「crystal-lessの力を過信したな」と言われてる気がした。（百錬三郎）<br>

ただ、PCBにテストピンを付けていないので、オシロスコープでクロック波形を観察するのが面倒だった。この検証は後回しにしよう。<br>

回路の問題の99%は設計者による技術資料の確認不足が原因（ソース不明）なので、STM32F072のデータシートとリファレンス・マニュアルのUSBに関する内容を読み直す。<br><br>

データシートの3.21 Universal serial bus (USB)を読むと、
> The STM32F072x8/xB embeds a full-speed USB device peripheral compliant with the USB specification version 2.0. The internal USB PHY supports USB FS signaling, embedded DP pull-up and also battery charging detection according to Battery Charging Specification Revision 1.2. The USB interface implements a full-speed (12 Mbit/s) function interface with added support for USB 2.0 Link Power Management.

と書いてある。<br>データラインのプルアップ抵抗は内蔵されていて、転送速度はFSに設定されてるんかい！<br>

キーボードはLSが一般的なので、USB_DMへプルアップ抵抗を接続していた。（下図の赤枠）<br>しかし、そのせいで、データラインが両方プルアップされてしまい、ホスト側で転送速度が分からなくなっていたらしい。<br>

<img src="/img/0315_07.png" width="100%" height="auto">
<br>
プルアップ抵抗R5を基板から外して、無事解決しました。<br>
ちなみに、Ubuntuでたまに動く理由は、OSのUSBスタックが転送速度が異常でも寛容だから。
<br><br>

---
## Summary
「スコスコ」とした打鍵音が良い。<br>IKEA効果も相まって、気に入っている。

...総額40,156円。あれ？HHKBを購入できる金額では？<br>これが、大量生産の妙か。