# AWSのソリューション

## section1 BigData:
- BigDataとは
  - 従来のアプリキーションでは扱うことが難しい
  - データセットから価値を引き出すための解決方法を指す
  - 課題
    - 分析
    - 検索
    - 転送
    - 更新
    - キャプチャ
    - 共有
    - 可視化
    - 情報プライバシー
    - データ管理
    - ストレージ
    - クエリ実行
  - BigDataと通常のデータの違い
    - これらの要素によってデータから価値を引き出すための新たな方法が必要となるか
    - 今得られている価値以上のものを求めているか
    - 異なるもの
      - ボリューム
      - 速度(データの生成、取得等の頻度)
      - 多様性
  - 核心となる考え方
    - 従来の環境では手に余るデータセット
    - 負荷を分散
    - 大規模なデータセットを分散→コンピューティングの高速化
  - Apache Hadoop
    - データベースではない
    - 伸縮自在なデータストレージとバッチ処理のフレームワークである
    - 外部データを取り込み、処理し、集約する
    - 結果はエクスポート可能
    - HDFS:Hadoop Distributed File System(分散処理システムのApache Hadoopが利用する分散ファイルシステム。OSのファイルシステムを代替するものではなく、その上に独自のファイル管理システムを構築するもの。)
    - ラックアウェアネス(同じラックに1つ、異なるラックに1つの計3つの同じデータを作成する)
    - ユーザが書き込むのは一回のみ、残りは内部で動作する
    - Data Nodeには各ブロックのマッピングが保存される
    - MapReduce
      - 大規模なデータセットの並列処理
      - データが構造化されているかは問わない
      - このフェーズでは処理結果が整理され、集約された結果を出力する
      - 2つのフェーズ
        - マップ
        - リデュース
      - その他
        - パーティション(マッパーからキー、バリューペアを受け取るリデューサーが決定される)
        - シャッフル(データがマッパーからリデューサーに転送される)
        - ソート(キーでソートする)
    - Hive
      - データウェアハウスインフラストラクチャ
      - HQL(SQLをオブジェクト指向風に少しラップしただけの言語)を利用可能
      - 非構造化データのクエリ
      - アドホッククエリ(一般的に定期的に行われるクエリに対し、アドホッククエリとは恒常的ではなく、その時に欲しい情報の組み合わせをデータベースから抽出したり操作したりといった処理を行うための問い合わせ)が簡単に実行できる
    - Pig
      - データ処理向けのプログラミング環境
      - Pig Latin(シンプルな専用言語)
        - クエリ
        - データ操作
        - SQLとMapReduceを結合
  - AWSビッグデータのエコシステム
    - 収集
      - リアルタイム:Kinetic firehose
      - データのインポート:snowball
    - 保存
      - オブジェクトストレージ:S3
      - リアルうタイム:Knetic streams
      - RDBMS:amazon rds
      - NoSQL:dynamodb
    - 処理と分析
      - Hadoopエコシステム:emr
      - リアルタイム:lamda, kinesis analytics
      - データウェアハウジング:redshift
      - 機械学習:machine learing
    - 可視化
      - ビジネスインテリジェンスデータ可視化:quicksight
    - IoT:IoT
    - 伸縮自在な検索分析:Elastic Search
  - ビッグデータツール
    - 各フェーズで様々なツールとの互換性がある
  - AWS Marketplace
    - 2000種類以上の出品
    - 1クリックデプロイ
    - 従量課金制の料金体系

## section2 クラウド移行:
日数 = 合計バイト数/(Mbps * 125 * 1000 * ネットワーク使用率 * 60秒 * 60分 * 24時間)
- 移行手段例
  - 以下の手段を単体、または組み合わせて使用することができる
  - 1回の大規模なバッチ
  - 定期的なデバイスストリーム
  - 断続的な更新
  - ハイブリッドデータストレージ(AWSクラウドとオンプレミスのデータストア)
- リスクを最小限に抑えながらデータを短時間で異動する
  - 接続速度10Mbps未満、データ量500GB未満→アンマネージド型移行ツール
  - 接続速度10Mbps超、データ量500GB超→アンマネージド型移行ツール
- 移行に使用するツール
  - アンマネージド型
    - amazon s3 コマンドラインインターフェース
      - 一回のオペレーションで最大5GBのイブジェクトをアップロード
      - それ以上のオブジェクトはマルチパートアップロード
        - アップロードを開始する
        - オブジェクトの各パートをアップロードする
        - マルチパートアップロードが完了する
    - amazon glacier コマンドラインインターフェース
      - 大規模なデータセットを移行する
      - 100GBを超える場合はマルチパートアップロード
  - マネージド型
    - AWS direct connect
      - AWSとオンプレミスロケーションの間に専用ネットワーク接続を確立する
      - 1GBまたは10GBのネット
      - データ転送サービスではない
      - 高帯域幅のバックボーンが提供される
      - バンドルで提供されるもの
        - 高度なハイブリッドアーキテクチャ
        - オンプレミスネットワーク
        - セキュリティ
        - ストレージ
        - コンピューティングテクノロジー
    - aws snowball
      - 対処できる問題
        - 高額なネットワーク転送コスト
        - 長い転送時間
        - セキュリティ上の懸念事項
      - 最適な用途
        - 大量のデータを転送する
        - ネットワークのアップグレードが不可能
        - 大量のデータの滞留が生じている
        - 遠隔地に位置している
        - 高速インターネット接続がない
        - インターネット経由のデータ転送に1時間以上かかる
      - キャパシティ
        - 50TBまたは80TB
        - 必要な場合は複数のSnowballデバイスを使用する
      - インターネットを経由しない
    - amazon s3 transfer acceleration
      - 利用できる帯域幅を最大化
      - 世界各地から中央の拠点に転送される定期的なジョブに最適
      - 距離がスループットに与える影響を最小限にできる

    - レガシーデータストアを時間をかけて段階的に移行できる場合
    - または、新しいデータを多くの非クラウドソースから集約する場合
      - 既存のインストール方法を活用または補完することができる
      - 自分のアプリケーションにAmazon kineses data firehoseストリームを組み込むコードを作成することができる
      - aws storage gateway
        - データは圧縮され、安全に転送される
        - ストレージ設定でボリュームをローカルに保管、またはキャッシュを選択できる
        - 仮想テープライブラリモード
      - aws kinesis firehose
        - ストリーミングデータをAWSにロードする
        - Amazon S3やAmazon redshiftに自動的にロードできる
        - →現在使用しているビジネスインテリジェンスツールやダッシュボードを使って、ほぼリアルタイムに分析できる。
        - 自動的にスケールされるため、継続的な管理は不要
        - ロード前にデータのバッチ処理、圧縮処理、暗号化が行われる
        - →送信先でのストレージ量を最小化し、セキュリティを強化できる。
      - テクノロジーのパートナーシップ
        - サードパーティから選択
        - バックアップカタログが一貫性のあるものになる
        - Amazon S3 コネクタ
          - バックアップソフトウェアでクラウドをターゲットとして直接指定することができる
          - 既存のオンプレミスのプロセスに影響を与えることや、プロセスの再設計することなしに、クラウドにバックアップを作成できる
- AWS migration service
  - aws server migration service
    - オンプレミスのワークロードをAWSに移行する
      - 稼働中サーバのボリュームの増分レプリケーションの自動化、スケジュール設定、追跡を行う
      - AWSマネジメントコンソールで数回のクリックで使用を簡単に開始できる
      - カスタマイズされたレプリケーションを作成して管理する
      - 移行をよりスピーディーに実行する
      - サーバーのダウンタイムを大幅に削減できる
  - aws application discovery service
    - オンプレミスデータセンターで実行されているアプリケース音を特定する
    - アプリケーションとそのパフォーマンスのリストを作成する
    - 情報は暗号化され、CSVやXMLファイルとしてエクスポートできる
    - 主な特徴
      - アプリケーションの検出
      - アプリケーション依存関係のマッピング
      - アプリケーションパフォーマンスの測定
        - 移行後のパフォーマンスの基準確立を支援する

## section3 モバイルアプリケーション:
- AWSのモバイルアプリケーション
  - 一般的なモバイルプラットフォームとの互換性がある
    - iOS, Android/Fire OS, Xamarin, Unity用のライブラリ、コードサンプル、ドキュメントが用意されている
  - 任意のコーディング言語を使用可能
  - AWS mobile sdkによる迅速な構築と統合
  - 一般的なバックエンドプラットフォーム向けSDK
    - Java, Ruby, PHP, Node.js, .Netなど
- AWSでのモバイル開発
  - モバイルアプリケーションの要件
    - Amazon Cognito
      - ユーザーアイデンティティの管理と認証
      - ユーザーデータの同期
    - Amazon Mobile Analytics
      - 非同期通信
      - アクティブデバイス分析
      - ユーザー行動分析
      - エンゲージメント分析
    - Amazon SNS モバイルプッシュ
      - プッシュ通知
      - イベントトリガー
    - AWS Lamda
      - プラットフォームに依存しないモバイルバックエンド
      - データの検証と変換
    - モバイル用に最適化されたコネクタ(Kinesis, S3, DynamoDB, SQS)
      - ファイルとメディアのストレージ
      - 共有データベースストレージ
      - データ収集
  - 開発者が直面する問題
    - プラットフォーム間の細分化が必要
    - デバイス間での同期の維持が困難
    - 画一的で面倒な作業を行わざるを得ない
