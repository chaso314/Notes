# IBM System z の構成要素

## プロセッサー
### プロセッサーの種類
- CP (Central Processor): 汎用
  - z/OS、z/VM、z/VSE (Virtual Storage Extended)などOSを稼働
- IFL (Integrated Facility for Linux): Linux 専用
  - z/VMとLinuxのみが稼働
- zAAP (System z Application Assist Processor): Java 専用
  - WebSphere Application ServerのJavaプログラムを稼働。CPで稼働するJavaアプリケーションの40%をオフロード(処理の肩代わり)可能
- zIIP (System z Integrated Information Processor): DB2 専用
  - CPのDB2の処理の一部をオフロード
- SAP (System Assist Processor): I/O専用
  - LPAR (Logical Partition)とI/O装置のI/O処理の開始・終了を管理
- ICF (Internal Coupling Facility): 統合機構
  - Parallel Sysplex 並列シスプレックス環境を実現するためCF(Coupling Facility)機能を提供。データ共有(資源ロック情報、ログ情報)と複数サーバー間接続(データベースやトランザクションのバッファー)の機能を提供する。
    
### プロセッサーの構成
- プロセッサー・ブック: 基板の集まり。ボード
  - Z筐体のCEC (Central Electric Complex)に格納される。モデルによりブックの数が異なる
  - CP、ICF、SAPなどの複数のさまざまなタイプのプロセッサー(MCM)とキャッシュとメモリーを1つのボードに総裁
- MCM(Multi Chip Module): 基板
  - 複数のプロセッサーと制御チップ (SC: System Controller)を搭載する基板
- プロセッサー: CPU。チップ
  - マルチコアプロセッサー。複数プロセッサーがMCMとして基板に実装される 
- プロセッサー・コア: 演算装置
  - 命令を実行する物理的最小単位。1つのプロセッサー・コアがPU (Processor Unit)となり、プロセッサーの種類のうちの1つとして機能する
- PU (Processor Unit): 処理機構。エンジン
  - 命令を実行する論理的最小単位。特定の処理に特化した演算装置の総称。例として1つのPUが1つのIFLとして機能する
- キャッシュ: プロセッサーが直接アクセスするデータを格納する領域
  - L1～L4の4段階。L1～L2はプロセッサー・コア毎に所持。L3はプロセッサー内で共用。L4はブックをまたいで共用

### プロセッサー関連機能
- キャパシティー・オン・デマンド・オファリング (CoD): 通常無効化されている資源を必要時に有効化できる機能
  - キャパシティー・バック・アップ：CBU
    - 被災時に最大90日間性能を拡張する
  - キャパシティー・フォー・プランド・イベント：CPE
    - 保守などで停止時に最大3日間性能拡張
  - オンオフ・キャパシティー・オンデマンド：On/Off CoD
    - 繁忙期などに一時的に性能拡張
  - キャパシティー・プロビジョニング (z/OS MVS Capacity Provisioning)
    - On/Off CoD登録済みの際に、自動的に一時的に性能拡張する条件をルール設定する機能
---
## メモリー・サブシステム: メモリ管理システム
- RAIM（Redundant Array of Independent Memory)技術を採用。IBMが開発したRAIDのDiskでなくMemory版の冗長化機構
- DRAM(Dynamic Random Access Memory)チップ障害、DIMM(Dual Inline Memory Module)(メモリ全体)の障害、メモリチャネル障害時にデータを復旧
---
## チャネル・サブシステム: 入出力管理システム
- ディスクやネットワークに対する入出力処理をCPUの代わりに実施
### 動的チャネル・サブシステム (Dynamic Channel Subsystem): 入出力経路仮想化
- 入出力経路選択処理をCPUの代わりに実施し、パスを複数PUで共有。転送の高速化や障害時の代替経路選択に強み
### ESCON (Enterprise System CONection): 光ファイバ入出力管理システム
- ESCON専用光ケーブルとESCON専用アダプタ(16-port ESCON)を介して周辺機器に接続。光ファイバと形状が異なり、互換性無し
### FICON (FIbre CONnection): 光ファイバ入出力管理システム
- 一般的な光ファイバケーブルを介して周辺機器に接続。FCPをIBM独自の通信プロトコルで拡張したもののため、一部周辺機器(SAN)で使用不可
### FCP (Fibre Channel Protocol): 光ファイバ入出力管理システム
- 一般的な光ファイバケーブルを介して周辺機器に接続する一般的なプロトコル。SAN(Storage Area Network)への接続やLinux、z/VM、z/VSEでの機器もサポート
### OSA (Open Systems Adapter): LANアダプタ
- TCP/IP、Ethernetに接続するためのアダプタ
---
## シスプレックス用システム間接続: サーバー間接続
- 複数台のIBM System zサーバーをCF機能で接続。InfiniBandなど少し特殊な光ケーブルを使用
---
## サポート・エレメント(Support Element: SE): ハードウェア･コンソール
- 保守用ハードウェア・コンソール。IBMの技術員が使用。冗長構成で筐体内に2つ設置
---
## ハードウェア･マネージメント･コンソール (Hardware Management Console: HMC): 遠隔管理コンソール
- ネットワーク経由で遠隔アクセスできるサーバー設定コンソール
### IBM zEnterprise Unified Resource Manager: 多資源一元管理ソフトウェア
- 異なるアーキテクチャ(z/OS、Linux、AIXなど)の資源をHMCを通して集中管理
- システムを2項目に分けて管理する
  - Manage: 運用、仮想サーバー、ハイパーバイザー、仮想ネットワーク、仮想ストレージ、電源、稼働状況
  - Automate: パフォーマンス監視、レポート、リソース最適化
