# Camel for Spring Boot 4.10 トレーニング

[**第1章：イントロダクション	4**](#第1章：イントロダクション)

[● 1.1 本トレーニングの目的と構成	4](#1.1-本トレーニングの目的と構成)

[● 1.2 対象とする技術スタックとバージョン	4](#1.2-対象とする技術スタックとバージョン)

[● Apache Camel 4.10	4](#apache-camel-4.10)

[● Spring Boot 3.x	4](#spring-boot-3.x)

[● XML IO DSL	4](#xml-io-dsl)

[● 1.3 前提知識と学習ゴール	5](#1.3-前提知識と学習ゴール)

[**第2章：KarafとSpring Bootの違いと移行観点	6**](#第2章：karafとspring-bootの違いと移行観点)

[● 2.1 Karaf（OSGi）とSpring Bootの構造的違い	6](#2.1-karaf（osgi）とspring-bootの構造的違い)

[○ バンドル管理 vs fat JAR	6](#バンドル管理-vs-fat-jar)

[○ Blueprint DSL vs XML IO DSL	6](#blueprint-dsl-vs-xml-io-dsl)

[○ JMX, コンソール, ログ管理の違い	7](#jmx,-コンソール,-ログ管理の違い)

[● 2.2 Camelコンテキストの起動・停止管理の違い	7](#2.2-camelコンテキストの起動・停止管理の違い)

[● 2.3 移行時の注意点	8](#2.3-移行時の注意点)

[○ OSGi依存コードの排除	8](#osgi依存コードの排除)

[○ ServiceMix・Features定義の削除	8](#servicemix・features定義の削除)

[○ デプロイ方法の変更（Karaf deploy → Spring Boot JAR実行）	8](#デプロイ方法の変更（karaf-deploy-→-spring-boot-jar実行）)

[**第3章：Camel 4.10 のアーキテクチャ変更点	9**](#第3章：camel-4.10-のアーキテクチャ変更点)

[● 3.1 Camel 3 → 4 の主な変更点	9](#3.1-camel-3-→-4-の主な変更点)

[● 3.2 削除されたコンポーネントと代替策	9](#3.2-削除されたコンポーネントと代替策)

[● 3.3 XML IO DSLにおける構文上の変更点（Camel 4）	10](#3.3-xml-io-dslにおける構文上の変更点（camel-4）)

[✅ \<bean\> の利用制限と代替	11](#✅-\<bean\>-の利用制限と代替)

[✅ \<route\> や \<from\> の uri 属性における厳密な構文チェック	11](#✅-\<route\>-や-\<from\>-の-uri-属性における厳密な構文チェック)

[✅ 対策	11](#✅-対策)

[● 3.4 ストリームキャッシュとスプール設定	12](#3.4-ストリームキャッシュとスプール設定)

[● 3.5 CamelContextの構成と拡張の変化	12](#3.5-camelcontextの構成と拡張の変化)

[Camel 4 における設計変更ポイント	12](#camel-4-における設計変更ポイント)

[XML IO DSL での CamelContext 構成例	13](#xml-io-dsl-での-camelcontext-構成例)

[ポイントまとめ（XML IO DSL の観点）	14](#ポイントまとめ（xml-io-dsl-の観点）)

[**第4章：プロジェクトセットアップと構成管理	15**](#第4章：プロジェクトセットアップと構成管理)

[● 4.1 プロジェクト構成（Maven）	15](#4.1-プロジェクト構成（maven）)

[● 4.2 Spring BootによるCamelContextの起動方法	16](#heading=h.95xpctglbkp8)

[● 4.3 XML IO DSLの読み込みとルート登録	17](#4.2-xml-io-dslの読み込みとルート登録)

[✅ XML IO DSLのルート定義ファイルの配置例	17](#✅-xml-io-dslのルート定義ファイルの配置例)

[🔍 注意点（Blueprint DSL との違い）	17](#🔍-注意点（blueprint-dsl-との違い）)

[● 4.4 application.propertiesによる構成管理とKarafとの比較	18](#4.3-application.propertiesによる構成管理とkarafとの比較)

[代表的な構成ファイル	18](#代表的な構成ファイル)

[設定例	18](#設定例)

[特徴比較	19](#特徴比較)

[**第5章：ルート開発（XML DSL）	20**](#第5章：ルート開発（xml-dsl）)

[● 5.1 基本構文と構造	20](#5.1-基本構文と構造)

[最小構成の例	20](#最小構成の例)

[読み込み方法（Spring Boot環境）	20](#読み込み方法（spring-boot環境）)

[● 5.2 Karaf Blueprint DSLとの相違点	21](#5.2-karaf-blueprint-dslとの相違点)

[● 5.3 EIPの使い方と変更点（InOnlyの廃止と属性化）	22](#5.3-eipの使い方と変更点（inonlyの廃止と属性化）)

[代表的なEIPの例	22](#代表的なeipの例)

[変更点：\<inOnly\> / \<inOut\> 要素の削除	22](#変更点：\<inonly\>-/-\<inout\>-要素の削除)

[✅ 旧（Blueprint DSLやSpring XML DSL）:	22](#✅-旧（blueprint-dslやspring-xml-dsl）:)

[移行アドバイス	23](#移行アドバイス)

[● 5.4 errorHandler／onExceptionの定義方法	23](#5.4-errorhandler／onexceptionの定義方法)

[✅ グローバルエラーハンドラの定義（XML IO DSL）	23](#✅-グローバルエラーハンドラの定義（xml-io-dsl）)

[✅ onExceptionの定義例	23](#✅-onexceptionの定義例)

[● 5.5 REST DSLと\<jetty\>等の代替に関する実装注意点	25](#5.5-rest-dslと\<jetty\>等の代替に関する実装注意点)

[REST DSLの使用例（XML IO DSL）	25](#rest-dslの使用例（xml-io-dsl）)

[注意点	25](#注意点)

[**第6章：テストとトラブルシューティング	26**](#第6章：テストとトラブルシューティング)

[● 6.1 camel-test-spring \+ JUnit5 の活用	26](#6.1-camel-test-spring-+-junit5-の活用)

[依存関係（pom.xml）	26](#依存関係（pom.xml）)

[テスト例	26](#テスト例)

[● 6.2 Spring Bootログ構成（access, gc, consoleログなど）	27](#6.2-spring-bootログ構成（access,-gc,-consoleログなど）)

[標準構成ファイル：logback-spring.xml	27](#標準構成ファイル：logback-spring.xml)

[ログ種別と対応表	27](#ログ種別と対応表)

[● 6.3 Actuatorによるヘルスチェック	28](#6.3-actuatorによるヘルスチェック)

[依存追加（pom.xml）	28](#依存追加（pom.xml）)

[設定（application.yaml）	28](#設定（application.yaml）)

[出力例	29](#出力例)

[● 6.4 BacklogTracer／Tracerの使い方と変更点	29](#6.4-backlogtracer／tracerの使い方と変更点)

[BacklogTracer：ルートの「最後のメッセージ」を記録	29](#backlogtracer：ルートの「最後のメッセージ」を記録)

[Tracer：全メッセージの逐次トレース（パフォーマンスへの影響大）	30](#tracer：全メッセージの逐次トレース（パフォーマンスへの影響大）)

[注意点（Camel 4.10での変更）	30](#注意点（camel-4.10での変更）)

[Spring Bootでの有効化例（application.properties）	30](#spring-bootでの有効化例（application.properties）)

[**第7章：運用・監視・ビルド・デプロイ	31**](#第7章：運用・監視・ビルド・デプロイ)

[● 7.1 Fat JARの作成と実行方法	31](#7.1-fat-jarの作成と実行方法)

[Fat JAR作成手順（Maven）	31](#fat-jar作成手順（maven）)

[● 7.2 Mavenビルドと依存関係管理	32](#7.2-mavenビルドと依存関係管理)

[依存の定義例（HTTP, CXF, XML DSL）	32](#依存の定義例（http,-cxf,-xml-dsl）)

[● 7.3 外部プロパティ管理（ConfigMap非使用前提）	33](#7.3-外部プロパティ管理（configmap非使用前提）)

[プロファイルごとのプロパティファイル	33](#プロファイルごとのプロパティファイル)

[実行時にプロファイル指定	33](#実行時にプロファイル指定)

[コマンドラインからの上書きも可能	33](#コマンドラインからの上書きも可能)

[● 7.4 JMX監視とHawtIO（Spring Bootでの使い方）	34](#7.4-jmx監視とhawtio（spring-bootでの使い方）)

[JMX有効化	34](#jmx有効化)

[HawtIO導入（埋め込み）	34](#hawtio導入（埋め込み）)

[**第8章：移行演習	36**](#第8章：移行演習)

[● 8.1 Blueprintルート → XML IO DSLへの変換演習	36](#8.1-blueprintルート-→-xml-io-dslへの変換演習)

[Blueprint DSL での例（before）	36](#blueprint-dsl-での例（before）)

[XML IO DSL での例（after）	36](#xml-io-dsl-での例（after）)

[変換ポイント	37](#変換ポイント)

[● 8.2 cxf: コンポーネントを含むSOAPルートの移行	37](#8.2-cxf:-コンポーネントを含むsoapルートの移行)

[Blueprint DSL の例（before）	37](#blueprint-dsl-の例（before）)

[XML IO DSL の例（after）	38](#xml-io-dsl-の例（after）)

[🔍 補足と注意点	38](#🔍-補足と注意点)

[● 8.3 ルーティングとException処理の構造再現演習	39](#8.3-ルーティングとexception処理の構造再現演習)

[Blueprint DSL の例（before）	39](#blueprint-dsl-の例（before）-1)

[XML IO DSL の例（after）	39](#xml-io-dsl-の例（after）-1)

[● 🔍 移行ポイント（XML IO DSL向け）	40](#🔍-移行ポイント（xml-io-dsl向け）)

[**第9章：まとめと今後の運用設計	42**](#第9章：まとめと今後の運用設計)

[● 9.1 Camel for Spring Bootの導入効果	42](#9.1-camel-for-spring-bootの導入効果)

[● 9.2 段階的移行戦略（モジュール単位移行、ゼロダウンタイム等）	43](#9.2-段階的移行戦略（モジュール単位移行、ゼロダウンタイム等）)

[推奨される段階的移行アプローチ	43](#推奨される段階的移行アプローチ)

[● 9.3 継続的改善に向けた運用設計のポイント	43](#9.3-継続的改善に向けた運用設計のポイント)

[1\. 可視化と監視の強化	43](#1.-可視化と監視の強化)

[2\. プロパティと機密情報の管理	43](#2.-プロパティと機密情報の管理)

[3\. CI/CDの整備	44](#3.-ci/cdの整備)

[4\. アップグレード対応と技術継承	44](#4.-アップグレード対応と技術継承)

# 

# 第1章：イントロダクション {#第1章：イントロダクション}

* ## 1.1 本トレーニングの目的と構成 {#1.1-本トレーニングの目的と構成}

本トレーニングの目的は、**Apache Camel 4.10** と **Spring Boot 3.x** をベースにした **XML IO DSL** によるアプリケーション構築方法を習得し、従来の **Fuse/Karaf（Blueprint DSL）環境からの移行**に必要な知識と実践スキルを身に付けることです。

また、KarafからSpring Bootベースへの移行時に遭遇する構成・起動・運用の違いを理解し、Camel 4.x の新機能や変更点に対応できるようになることを目的としています。

本トレーニングでは以下の構成で学習を進めます：

* **第1章**：イントロダクション（目的・前提・技術スタック）  
* **第2章**：KarafとSpring Bootの比較と移行観点  
* **第3章**：Camel 4.10の技術的な変更点  
* **第4章**：プロジェクトセットアップと構成管理  
* **第5章**：XML IO DSLによるルート開発  
* **第6章**：テスト・トラブルシューティング  
* **第7章**：運用・監視・デプロイ  
* **第8章**：演習：Blueprint → XML IO DSLへの移行実践  
* **第9章**：まとめと今後の運用設計

各章では、実践的な構成例やコードスニペットを交え、移行時に特に注意すべき点を重点的に解説します。

* ## 1.2 対象とする技術スタックとバージョン {#1.2-対象とする技術スタックとバージョン}

このトレーニングでは以下の技術スタックを採用します：

* ### Apache Camel 4.10 {#apache-camel-4.10}

  * 最新のルーティングエンジン、リファクタリングされたモジュール体系  
  * Blueprint DSL非対応、Spring DSLの明示的な採用が必要

* ### Spring Boot 3.x {#spring-boot-3.x}

  * Jakarta EE 10対応（`javax.*` → `jakarta.*`）  
  * 自動構成、Spring Actuator、プロファイルなどの運用支援機能

* ### **XML IO DSL** {#xml-io-dsl}

  * Camel ルートの記述には `http://camel.apache.org/schema/io` 名前空間を使用  
  * Spring ApplicationContext には依存せず、Camel 単体で動作可能な構造（Spring Boot との親和性も高い）  
  * `<beans>` や `<camel>` タグの中に `<route>`, `<from>`, `<to>`, `<onException>` などの DSL 要素を直接記述可能  
  * Blueprint DSL よりも軽量で、Camel JBang や Kaoto などのツールと親和性が高い

* ## 1.3 前提知識と学習ゴール {#1.3-前提知識と学習ゴール}

**■ 前提知識**

このトレーニングの内容を効果的に理解するために、以下の知識を有していることが望まれます：

* Java / Maven ベースの業務アプリケーション開発経験  
* Apache Camelの基本構文とEIPの理解（split、choice、multicastなど）  
* Karaf（OSGiベース）およびBlueprint DSLによるCamelルートの開発経験  
* Spring Framework（DI、ApplicationContext）の基礎知識

■ 学習ゴール

トレーニングの完了時点で、以下の状態に到達していることを目指します：

* Spring Boot上でCamelルートをXML IO DSLで定義・動作させられる  
* Blueprint DSLで書かれたルートをXML IO DSLへ移行できる  
* Spring Bootにおける構成・起動・依存解決の方式を理解している  
* Camelルートの例外処理、プロパティ定義、外部連携を適切に実装・検証できる


---

# 

# 第2章：KarafとSpring Bootの違いと移行観点 {#第2章：karafとspring-bootの違いと移行観点}

Apache Camelのルートは、Karaf上でもSpring Boot上でも動作しますが、それぞれのプラットフォームはアーキテクチャ的に大きく異なります。本章では、Karaf（OSGiコンテナ）とSpring Bootの構造的な違いを理解した上で、Camel 4.10＋XML IO DSLへの移行時に考慮すべき要点を解説します。

* ## 2.1 Karaf（OSGi）とSpring Bootの構造的違い  {#2.1-karaf（osgi）とspring-bootの構造的違い}

  * ### バンドル管理 vs fat JAR {#バンドル管理-vs-fat-jar}

  **Karaf（OSGi）** は、機能ごとに分割されたバンドル（JAR）を動的に読み込み・起動・停止する**モジュール指向**のプラットフォームです。各バンドルは`MANIFEST.MF`内でインポート/エクスポートされるパッケージを明示的に宣言し、サービスレジストリを介して連携します。

  **Spring Boot** は、すべての依存JARとアプリケーションコードを1つの**Fat JAR**にビルドし、`java -jar`で実行する**単一プロセス指向**のランタイムです。OSGiのようなバンドル分離やクラスローダ制御は存在せず、クラスパスベースの依存管理が前提です。Fat JARは、すべての依存関係を内包するため、コンテナイメージのビルドを簡素化し、どこでも同じように動作する不変のデプロイ単位を提供します


  * ### Blueprint DSL vs XML IO DSL {#blueprint-dsl-vs-xml-io-dsl}

  **Blueprint DSL（Karaf）** は、`http://www.osgi.org/xmlns/blueprint/v1.0.0` 名前空間に基づき、CamelルートとJava BeanをOSGi環境向けに宣言的に記述します。OSGiサービス参照やIDベースの依存関係管理が特徴です。

  **XML IO DSL（Camel 4以降）** は、`http://camel.apache.org/schema/io` 名前空間を使用し、Camelのルートや設定をSpringやOSGiに依存せず、軽量かつスタンドアロンで定義できます。Camel JBangやKaotoなどツールとの親和性も高く、モダンなDSLです。

  両者は `<route>`, `<from>`, `<to>`, `<onException>` などの構文に共通性がありますが、**Blueprint DSLはOSGi依存型**、**XML IO DSLは非依存かつ軽量構成**という点で異なります。XML IO DSLではBlueprint特有の `<reference>` 要素やサービス参照は使用できません。


  

  * ### JMX, コンソール, ログ管理の違い {#jmx,-コンソール,-ログ管理の違い}

  **Karaf** は、`karaf shell` によるOSGiコンソール、`log:tail`等のリアルタイムログ確認、`jconsole`によるJMX接続が統合されています。

  **Spring Boot** は、`Actuator`エンドポイント（例：`/actuator/health`, `/actuator/logfile`）や`logback-spring.xml`によるログ制御を用い、JMX接続は`spring.jmx.enabled=true`で明示的に有効化されます。

  移行後は、Karafコンソールに相当する操作はHawtIOやSpring Boot Admin、Actuator経由の管理が基本となります。


* ## 2.2 Camelコンテキストの起動・停止管理の違い {#2.2-camelコンテキストの起動・停止管理の違い}

**Karaf** では、Camelコンテキストは各バンドル内の`blueprint.xml`定義をもとに、自動的にOSGiフレームワークから起動・停止されます。

**Spring Boot** では、CamelContextは`@SpringBootApplication`により起動される`SpringApplication.run()`の中でライフサイクルを管理します。

* `camel-spring-boot` が Spring Boot 起動時に `CamelContext` を自動的に生成します。  
* `application.yaml` に `camel.main.routesIncludePattern=classpath:routes/*.xml` などと設定することで、`XML IO DSL` 形式で記述されたルート（例: `aggregate-routes.xml`）が CamelContext に直接ロードされます。  
* Spring ApplicationContext に依存せず、Camel が独自に XML ファイルを解釈してルートを登録します。

📌 **特徴**：Spring に依存しない構造でありながら、Spring Boot の自動構成や依存注入の利便性を享受できます。

* 停止は`Ctrl+C`や`ApplicationContext.close()`により行われ、CamelContextも連動して停止されます。

**起動制御のカスタマイズは、Karaf ではバンドルのアクティベーション状態に依存して自動的に CamelContext が起動します。一方、Spring Boot では \`MainConfiguration\` や \`CamelContextConfiguration\` を使用して、CamelContext の初期化タイミングやルートの登録順などを \*\*明示的に制御\*\*する必要があります。**  

**XML IO DSL を使用する場合も同様に、\`camel.main.\*\` プロパティや \`CamelContextConfiguration\` による制御が推奨されます。**

* ## 2.3 移行時の注意点  {#2.3-移行時の注意点}

  * ### OSGi依存コードの排除 {#osgi依存コードの排除}

    * Blueprint DSLでは、`<reference interface="..."/>`や、OSGi Declarative Services（DS）に基づく`org.osgi.service`系のサービス参照が使われることがあります。

    * Spring Bootでは、これらの**OSGiサービス連携は利用できないため、明示的なDIまたはSpring管理Bean**への書き換えが必要です。

  * ### ServiceMix・Features定義の削除 {#servicemix・features定義の削除}

  * Karafでは、`features.xml`によってCamelコンポーネントや依存ライブラリを動的に解決・インストールしていました。

  * Spring Bootでは、`pom.xml`の`<dependencies>`内で依存ライブラリを明示的に指定する必要があります。

* 例：`camel-cxf`, `camel-jackson`, `camel-kafka` など

  * ### デプロイ方法の変更（Karaf deploy → Spring Boot JAR実行） {#デプロイ方法の変更（karaf-deploy-→-spring-boot-jar実行）}

    * **Karaf**：`deploy/`ディレクトリにバンドルを配置

    * **Spring Boot**：`mvn package`でfat JARを作成し、`java -jar app.jar`で起動

    * 実行方法、ログ取得、プロファイル切替などの運用方法も大きく異なるため、運用部門への引き継ぎ資料も必要となります。

    
      この章では、KarafとSpring Bootの基本的な思想と構造の違いを理解し、設計・構築・運用時に必要な移行対応の全体像を把握しました。次章では、Camel 4.10における技術的な変更点と、Blueprint DSLからの移行に影響する仕様を確認していきます。

---

# 

# 第3章：Camel 4.10 のアーキテクチャ変更点 {#第3章：camel-4.10-のアーキテクチャ変更点}

Apache Camel 4系は、Camel 3系から大幅な内部構造の見直しとAPIの整理が行われたバージョンです。特にBlueprint DSLの削除、コンポーネント構成の分割、CamelContextの抽象化など、移行プロジェクトに影響を及ぼす変更点が多数存在します。本章では、Camel 4.10への移行時に把握すべき主なアーキテクチャ変更点について解説します。

* ## 3.1 Camel 3 → 4 の主な変更点 {#3.1-camel-3-→-4-の主な変更点}

**Blueprint DSLの削除**  
 Camel 4では `http://camel.apache.org/schema/blueprint` 名前空間のDSLが完全に削除されました。Blueprint XMLで構築されたKaraf環境のルートは、**XML IO DSLまたはJava DSLへの移行が必須**です。

**APIの整理とスリム化**  
 内部APIが整理され、非推奨だった多くのメソッド・インターフェースが削除されました。特に`Exchange.setException(Throwable)`の扱いや、ルート内での型変換の自動化に関する挙動がCamel 3とは異なります。

**依存ライブラリの最小化**  
 Camel 4はコンポーネント単位での依存により、不要なライブラリを含めずにすむ設計に変更されました。これにより、Mavenで使用する依存は**明示的に定義する必要があります**。

**Jakarta EE への移行対応**  
 Camel 4では、Spring Boot 3に合わせて`javax.*`パッケージの使用が廃止され、すべて `jakarta.*` へ移行しています。独自コードやライブラリもこれに対応する必要があります。

* ## 3.2 削除されたコンポーネントと代替策 {#3.2-削除されたコンポーネントと代替策}

Camel 4では、以下のようなコンポーネントが削除されています：

| 削除されたコンポーネント | 代替手段 |
| ----- | ----- |
| `camel-jetty` | `camel-undertow` または REST DSL \+ embedded server |
| `camel-saxon` | JavaベースのXML処理または `camel-xslt` |
| `camel-servlet` | Spring Boot組込みサーバ（Tomcat等） |
| `camel-netty` | `camel-netty4` または `camel-grpc` 等 |
| `camel-twitter` | REST API を使用して独自実装へ |

Blueprint DSL は Camel 4.x で削除され、ルート定義は Spring Boot 向けの camel-spring-xml または camel-xml-io DSL に移行する必要があります。

✅ **対策**：`Blueprint DSL に依存したルートや構成（OSGiの参照、ID指定、PropertyPlaceholder など）は、`  

`- Spring Boot 上では camel-spring-xml 形式に、または`  

`- Spring 非依存構成として camel-xml-io DSL に`  

`リファクタリングする必要があります。`

``さらに、camel-xml-io を使う場合は Blueprint/Spring の `bean` 参照が使えないため、`<load-class>`, `<script>`, `<transform>` など XML IO 専用要素による再設計が必要です。``

* ## 3.3 XML IO DSLにおける構文上の変更点（Camel 4） {#3.3-xml-io-dslにおける構文上の変更点（camel-4）}

Camel 4 における XML IO DSL では、Blueprint や Spring XML DSL とは異なる構文上の特徴・制約があります。以下に主な違いを示します：

* `<description>` 要素の記法変更  
   Camel 3 までは `<description>テキスト</description>` の形式でしたが、  
   **Camel 4 の XML IO DSL では `<description text="テキスト"/>` の属性形式**に統一されています。

例（旧）:

```xml
<description>This route validates data</description>
```

例（XML IO DSL）:

```xml
<description text="This route validates data"/>
```

---

#### **✅ `<bean>` の利用制限と代替** {#✅-<bean>-の利用制限と代替}

Blueprint DSL で使用されていた `<bean id="x" class="..."/>` や `id-ref` による JavaBean 参照は、  
 **XML IO DSL では使用できません（Spring非依存構成のため）**。

代替方法としては：

* Java DSL 側で Bean を定義し、必要な処理は Java クラス経由で委譲する

* `process`, `transform`, `script` 等を XML IO DSL 上で使用する

  ---

  #### **✅ `<route>` や `<from>` の `uri` 属性における厳密な構文チェック** {#✅-<route>-や-<from>-の-uri-属性における厳密な構文チェック}

Camel 4 の XML IO DSL では `uri="..."` の属性記法において：

* **空白や不要なクォートの使用が許されず**

* **スペースを含む URI や文字列の扱いに厳格**

となっており、以前の Blueprint DSL や Spring XML DSL よりも構文ミスを検知しやすくなっています。

---

### **✅ 対策** {#✅-対策}

Blueprint DSL から XML IO DSL に移行する場合は：

* `<description>` 記法の変更（要素 → 属性）に対応する  
* `<bean>` など Spring/Blueprint 固有要素を避けるか、Javaコード側に委譲  
* XML Schema (XSD) によるバリデーションを活用し、構文の正当性を確保する


* ## 3.4 ストリームキャッシュとスプール設定 {#3.4-ストリームキャッシュとスプール設定}

Camel 4では、**大きなメッセージのメモリ圧迫を防ぐためのスプール（spooling）設定**が強化されています。

* デフォルトでは、Camelはメッセージボディの一部をストリームとして処理します（例：HTTP、ファイルなど）。

* ストリームキャッシュが有効な場合、一定サイズ以上のメッセージはメモリに保持されず、一時ファイルにスプールされます。

設定例（Spring Bootの`application.yaml`）：

```
camel:
  stream-caching:
    enabled: true
    spool-enabled: true
    spool-threshold: 4096
    spool-directory: /tmp/spool
```

* ## 3.5 CamelContextの構成と拡張の変化 {#3.5-camelcontextの構成と拡張の変化}

### **Camel 4 における設計変更ポイント** {#camel-4-における設計変更ポイント}

* Camel 4 では、`ExtendedCamelContext` は廃止され、代わりに `CamelContext` インターフェース自体が拡張されました。

* これにより、従来のように `ExtendedCamelContext` にキャストする必要はなく、**標準の `CamelContext` から直接各種設定が可能**となりました。

例（旧方式）：

```java
ExtendedCamelContext ecc = (ExtendedCamelContext) camelContext;
ecc.setXXX(...);
```

例（新方式）：

```java
camelContext.setXXX(...);  // CamelContextインターフェースが直接提供
```

### **XML IO DSL での CamelContext 構成例** {#xml-io-dsl-での-camelcontext-構成例}

XML IO DSL 環境では、Spring Boot のような Bean 定義による構成拡張は使用しません。代わりに、`application.yaml` や `camel-main` 設定ファイルで CamelContext の振る舞いを制御します。

✅ streamCaching の構成例（application.yaml）：

```
camel:
  stream-caching:
    enabled: true
    spool-enabled: true
    spool-threshold: 4096
    spool-directory: /tmp/spool
```

✅ XML IO DSL 側（routes ファイル）:

```xml
<routes xmlns="http://camel.apache.org/schema/io"
        streamCache="true">
  <route id="example-route">
    <from uri="timer://foo?period=5000"/>
    <log message="Streaming Route executed."/>
  </route>
</routes>
```

### **ポイントまとめ（XML IO DSL の観点）** {#ポイントまとめ（xml-io-dsl-の観点）}

* `CamelContext` の拡張は、Java API か `application.yaml` によって制御される。  
* Spring Boot のような `@Bean` や `CamelContextConfiguration` は使用しない。  
* XML IO DSL 単体では CamelContext の構成に関わる詳細な設定（stream caching、型変換の優先順位など）は行わず、**Camel Main 側の設定で補完**する。

本章では、Camel 4.10における技術的な変更点を整理しました。次章では、Spring BootプロジェクトにおけるCamel構成と、XML IO DSLのルート定義方法を実践的に解説します。

# 

# 第4章：プロジェクトセットアップと構成管理 {#第4章：プロジェクトセットアップと構成管理}

本章では、Camel 4.10 と Spring Boot 3.x をベースとしたアプリケーションのプロジェクト構成、CamelContext の起動方法、XML IO DSL の読み込み方法、構成ファイルの扱いについて解説します。Karaf（OSGi）とは異なり、Spring Bootではプロジェクト起動・構成・ルート登録のアプローチが統一されています。

* ## 4.1 プロジェクト構成（Maven） {#4.1-プロジェクト構成（maven）}

Camel for Spring Boot プロジェクトは、通常の Spring Boot アプリケーションと同様に Maven プロジェクトとして構成されます。以下は代表的な `pom.xml` の例です。

```xml
<project>
  <modelVersion>4.0.0</modelVersion>
  <groupId>com.example</groupId>
  <artifactId>camel-springboot-app</artifactId>
  <version>1.0.0</version>
  <packaging>jar</packaging>

  <properties>
    <java.version>21</java.version>
    <camel.version>4.10.3.redhat-00019</camel.version>
    <spring-boot.version>3.4.5</spring-boot.version>
  </properties>

  <dependencies>
    <!-- Spring Boot Starter -->
    <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter</artifactId>
    </dependency>

    <!-- Camel Spring Boot Starter -->
    <dependency>
      <groupId>org.apache.camel.springboot</groupId>
      <artifactId>camel-spring-boot-starter</artifactId>
    </dependency>

<!-- XML IO DSL サポート -->
<dependency>
  <groupId>org.apache.camel</groupId>
  <artifactId>camel-xml-io-dsl</artifactId>
</dependency>



    <!-- 使用するコンポーネント例（例：HTTP） -->
    <dependency>
      <groupId>org.apache.camel.springboot</groupId>
      <artifactId>camel-http-starter</artifactId>
    </dependency>
  </dependencies>

  <build>
    <plugins>
      <!-- Spring Boot Plugin -->
      <plugin>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-maven-plugin</artifactId>
      </plugin>
    </plugins>
  </build>
</project>
```

* ## 4.2 XML IO DSLの読み込みとルート登録 {#4.2-xml-io-dslの読み込みとルート登録}

Karafの Blueprint DSL では `blueprint.xml` に定義することで自動的にルートが読み込まれていましたが、**XML IO DSL** を Spring Boot 環境で使用する場合、**`application.yaml` または `application.properties` によるルートXMLの明示的な指定**が必要です。

#### **✅ XML IO DSLのルート定義ファイルの配置例** {#✅-xml-io-dslのルート定義ファイルの配置例}

以下のような XML ファイルを `src/main/resources/routes/my-routes.xml` に配置します：

```xml
<routes xmlns="http://camel.apache.org/schema/io">
  <route id="example-route">
    <from uri="timer:foo?period=5s"/>
    <setBody>
      <constant>Hello Camel 4.10 (IO DSL)</constant>
    </setBody>
    <log message="Received: ${body}"/>
  </route>
</routes>
```

`✅ application.yaml によるルートの有効化`

```
camel:
  xml-routes:
    paths: classpath:routes/my-routes.xml
```

#### **🔍 注意点（Blueprint DSL との違い）** {#🔍-注意点（blueprint-dsl-との違い）}

| 比較項目 | Blueprint DSL（Karaf） | XML IO DSL（Spring Boot） |
| ----- | ----- | ----- |
| ルートの読み込み | 自動（blueprint.xml） | 明示的にプロパティで指定 |
| 名前空間 | `http://www.osgi.org/xmlns/blueprint/v1.0.0` | `http://camel.apache.org/schema/io` |
| Bean参照 | OSGiサービスも利用可能 | Java DSL や DI 経由での連携が必要 |

---

このように、**XML IO DSLでは application.yaml/properties による明示的指定が必須**であり、Blueprint DSLのような自動読み込みとは異なる点に注意してください。

* ## 4.3 `application.properties`による構成管理とKarafとの比較 {#4.3-application.propertiesによる構成管理とkarafとの比較}

Karaf環境では、構成管理は以下のような形で分散されていました：

* OSGi Config Admin（`etc/*.cfg` ファイル）  
* Blueprint XML による `<property-placeholder>`  
* フラグメントバンドルでの動的設定提供

一方、Spring Bootでは、以下に統一されます：

#### **代表的な構成ファイル** {#代表的な構成ファイル}

* `src/main/resources/application.properties`  
   または  
* `application.yaml`

#### **設定例** {#設定例}

```
camel:
  xml-routes:
    paths: classpath:routes/my-routes.xml
  springboot:
    streamCachingEnabled: true
    name: camel-springboot-app

server:
  port: 8080
```

#### **特徴比較** {#特徴比較}

| 観点 | Karaf | Spring Boot |
| ----- | ----- | ----- |
| 設定管理方法 | etc/\*.cfg, Blueprint | application.properties / yaml |
| プロファイル切替 | Fabric8 / ConfigAdmin | Spring profiles（`--spring.profiles.active`） |
| ランタイム変更の反映 | OSGiバンドル再起動／reload | 再起動／Spring Cloud Configを併用可能 |
| 複数設定の管理 | Feature定義、フラグメントバンドル | プロファイル別設定ファイル |

この章では、Spring Boot上でのCamelアプリケーションのセットアップ手順、XML DSLルートの登録方法、Karaf環境からの構成管理の移行方法を整理しました。  
 次章では、XML IO DSLでのルート定義方法や主要EIPの記述方法について詳しく解説します。

---

# 

# 第5章：ルート開発（XML DSL） {#第5章：ルート開発（xml-dsl）}

本章では、Camel 4.10 において **XML IO DSL（http://camel.apache.org/schema/io）** を使用してルートを定義する方法について解説します。特に Karaf 環境で使用されていた **Blueprint DSL** との違いや、**EIP（エンタープライズ統合パターン）** の記述方式の変化、**例外処理や REST 定義の実装上の注意点** に焦点を当てます。

* ## 5.1 基本構文と構造 {#5.1-基本構文と構造}

**XML IO DSL** では、Camel のルートは `<routes>` 要素の中に `<route>` を記述することで構成されます。Spring に依存しない構造で、Camel JBang や Spring Boot などのランタイムに直接ロードされます。

#### **最小構成の例** {#最小構成の例}

```xml
<routes xmlns="http://camel.apache.org/schema/io">
  <route id="hello-route">
    <from uri="timer:hello?period=5s"/>
    <setBody>
      <constant>Hello from Camel XML IO DSL</constant>
    </setBody>
    <log message="Message: ${body}"/>
  </route>
</routes>
```

#### **読み込み方法（Spring Boot環境）** {#読み込み方法（spring-boot環境）}

この XML ファイル（例: routes/hello-route.xml）を src/main/resources 配下に配置し、application.yaml または application.properties に以下を設定することで自動的に読み込まれます：

```
camel:
  xml-routes:
    paths: classpath:routes/hello-route.xml
```

✅ `application.yaml` を使用して XML IO DSL を読み込む際には、`camel-xml-io-dsl` の依存が POM に含まれていることを確認してください。

* ## 5.2 Karaf Blueprint DSLとの相違点 {#5.2-karaf-blueprint-dslとの相違点}

**XML IO DSL** は Camel 4 で推奨される新しい記法であり、Blueprint DSL と構文的に似ているため移行は比較的スムーズですが、以下の点に注意が必要です。

| 比較項目 | Karaf Blueprint DSL | XML IO DSL |
| ----- | ----- | ----- |
| 名前空間 | `http://camel.apache.org/schema/blueprint` | `http://camel.apache.org/schema/io` |
| コンテキストルート定義 | `<camelContext id="...">` | `<routes>` タグ内に `<route>` を直接記述 |
| Bean定義 | OSGi参照（`<reference interface="..."/>` など） | 明示的な Spring 連携なし。Java定義の Bean を `ref` で参照 |
| Property参照 | `<property-placeholder>`（Blueprint構文） | Spring Boot の `application.yaml` 等により制御可能 |
| OSGiサービス利用 | `<reference>` による OSGiサービス参照が可能 | OSGiに依存せず、SpringコンテキストまたはCamel DIのみ対応 |

Blueprint 固有の構文（例：`<reference>` や OSGiのサービスバインディング）は XML IO DSL ではサポートされておらず、**CamelContext 単体で完結するシンプルな構成に置き換える必要があります**。

✅ **ポイント**：

* Blueprint DSL → XML IO DSL への移行では、OSGi 依存部分をすべて削除またはSpring Boot構成に変換。

* XML IO DSL は Camel の組込みDIとルート定義に特化しており、ランタイムに依存しない構成が可能です（Camel JBang / camel-main / Spring Boot いずれにも対応）。

* ## 5.3 EIPの使い方と変更点（InOnlyの廃止と属性化） {#5.3-eipの使い方と変更点（inonlyの廃止と属性化）}

Camel 4 では、XML IO DSL を用いた EIP（Enterprise Integration Pattern）の定義において、いくつかの構文変更が導入されています。

#### **代表的なEIPの例** {#代表的なeipの例}

```xml
<routes xmlns="http://camel.apache.org/schema/io">
  <route id="content-based-router">
    <from uri="direct:start"/>
    <choice>
      <when>
        <simple>${body} contains 'Camel'</simple>
        <to uri="log:camel"/>
      </when>
      <otherwise>
        <to uri="log:other"/>
      </otherwise>
    </choice>
  </route>
</routes>
```

#### **変更点：`<inOnly>` / `<inOut>` 要素の削除** {#変更点：<inonly>-/-<inout>-要素の削除}

Camel 4.10 以降、`<inOnly>` および `<inOut>` 要素は **XML IO DSL では非サポート** となり、代わりに `<to>` 要素に `pattern` 属性を付与する方法へ移行されました。

##### **✅ 旧（Blueprint DSLやSpring XML DSL）:** {#✅-旧（blueprint-dslやspring-xml-dsl）:}

```xml
<inOnly uri="seda:asyncQueue"/>
```

✅ 新（XML IO DSL）:

```xml
<to uri="seda:asyncQueue" pattern="InOnly"/>
```

この変更により、**ExchangePattern の設定はすべて `<to>` の属性で明示**する必要があります。

#### **移行アドバイス** {#移行アドバイス}

Blueprint DSL や Spring XML DSL で `<inOnly>` / `<inOut>` を使っていたルートは、XML IO DSL に移行する際にはすべて以下のように変換してください：

* `<inOnly uri="..."/>` → `<to uri="..." pattern="InOnly"/>`  
* `<inOut uri="..."/>` → `<to uri="..." pattern="InOut"/>`

✅ **ポイント**：

* Camel 4 の XML IO DSL では、構文の一貫性とシンプルさが重視され、要素ではなく属性による制御に移行。  
* 記法変更により、ExchangePattern を XML構文上でも統一的に管理可能となります。

* ## 5.4 errorHandler／onExceptionの定義方法 {#5.4-errorhandler／onexceptionの定義方法}

Camelでは、XML IO DSLを用いて、グローバルまたはルート単位での例外処理を柔軟に定義できます。

#### **✅ グローバルエラーハンドラの定義（XML IO DSL）** {#✅-グローバルエラーハンドラの定義（xml-io-dsl）}

```xml
<routes xmlns="http://camel.apache.org/schema/io">
  <errorHandler id="defaultErrorHandler" type="DefaultErrorHandler">
    <redeliveryPolicy maximumRedeliveries="3" redeliveryDelay="2000"/>
  </errorHandler>

  <route id="example">
    <from uri="direct:start"/>
    <to uri="bean:someProcessor"/>
  </route>
</routes>
```

* `errorHandler` 要素は `<routes>` の直下に定義します。  
* `type` 属性で使用するエラーハンドラの種類を指定できます（例: `DefaultErrorHandler`, `DeadLetterChannel`, `NoErrorHandler` など）。  
* `redeliveryPolicy` を組み込むことで再送制御も可能です。

#### ✅ onExceptionの定義例 {#✅-onexceptionの定義例}

```xml
<routes xmlns="http://camel.apache.org/schema/io">
  <onException>
    <exception>java.lang.Exception</exception>
    <handled>
      <constant>true</constant>
    </handled>
    <log message="Exception occurred: ${exception.message}"/>
    <setBody>
      <simple>Error: ${exception.message}</simple>
    </setBody>
  </onException>

  <route id="example">
    <from uri="direct:start"/>
    <to uri="bean:someProcessor"/>
  </route>
</routes>
```

* `<onException>` も `<routes>` の直下に記述します。  
* `handled` に `true` を設定することで、例外を捕捉してルートを継続させることができます。  
* `continued` を使えば、例外はログに記録されるものの、ルート処理を続行可能です。

✅ **補足ポイント**：

| 機能 | 説明 |
| ----- | ----- |
| `<errorHandler>` | 全ルート共通のエラーハンドリング設定 |
| `<onException>` | 特定の例外クラスに対応する個別の処理ロジック |
| `handled` / `continued` | Camel 4でも引き続き利用可能で、フロー制御の柔軟性が向上 |

---

💡 Blueprint DSL では `<camelContext>` 直下に定義していましたが、**XML IO DSLでは `<routes>` 直下に統一**されています。構文上の違いに注意してください。

* ## 5.5 REST DSLと`<jetty>`等の代替に関する実装注意点 {#5.5-rest-dslと<jetty>等の代替に関する実装注意点}

Camel 4.10では、Karafで使用されていた `<jetty>` コンポーネントや `<servlet>` コンポーネントは削除または非推奨になっており、代替として **Spring Bootの組み込みサーバ（Tomcatなど）** や REST DSL を使用します。

#### **REST DSLの使用例（XML IO DSL）** {#rest-dslの使用例（xml-io-dsl）}

```xml
<routes xmlns="http://camel.apache.org/schema/io">

  <restConfiguration component="platform-http" port="8080"/>

  <rest path="/hello">
    <get uri="/{name}">
      <to uri="direct:sayHello"/>
    </get>
  </rest>

  <route id="say-hello-route">
    <from uri="direct:sayHello"/>
    <setBody>
      <simple>Hello, ${header.name}!</simple>
    </setBody>
  </route>

</routes>
```

#### **注意点** {#注意点}

* `jetty` や `servlet` を使っていた既存ルートは、**REST DSL \+ Spring Boot組み込みサーバ構成**へ移行する必要があります。  
* URIパスのルーティングやCORS設定などはSpring Boot側で行うことが望ましい場合があります。

✅ Camel 4では `camel-undertow` または REST DSL の利用が公式に推奨されています。

---

この章では、Camel 4.10における XML IO DSLによるルート開発の基本、Blueprintからの差異、主要EIPの書き方、例外処理の定義方法、RESTの構築方法について解説しました。次章では、ルートのテスト、ログ出力、トラブルシュートの方法に進みます。

# 第6章：テストとトラブルシューティング {#第6章：テストとトラブルシューティング}

本章では、Camel for Spring Bootアプリケーションの品質保証および障害解析の手法として、**単体テストの実施方法**、**ログ構成の管理**、**ヘルスチェックの実装**、および**Camel組込みのトレース機能**について解説します。Karafとは異なり、Spring Bootでは統一されたログおよびテスト基盤を活用できます。

* ## 6.1 camel-test-spring \+ JUnit5 の活用 {#6.1-camel-test-spring-+-junit5-の活用}

Camelルートの単体テストは、`camel-test-spring-junit5` を利用することで容易に実現できます。

#### **依存関係（pom.xml）** {#依存関係（pom.xml）}

```xml
<dependency>
  <groupId>org.apache.camel.springboot</groupId>
  <artifactId>camel-test-spring-junit5</artifactId>
  <scope>test</scope>
</dependency>
```

#### **テスト例** {#テスト例}

```java
@CamelSpringBootTest
@SpringBootTest
public class SampleRouteTest {

  @Autowired
  private ProducerTemplate producerTemplate;

  @Test
  void testRoute() {
    String result = producerTemplate.requestBody("direct:hello", "Keiji", String.class);
    assertEquals("Hello Keiji", result);
  }
}
```

* `@CamelSpringBootTest` により CamelContext を起動  
* `ProducerTemplate` を使ってルートを呼び出し  
* 実行結果を `assertEquals()` で検証

✅ Blueprint環境でのテストより簡潔で、DIやMockを使ったテストも容易です。

* ## 6.2 Spring Bootログ構成（access, gc, consoleログなど） {#6.2-spring-bootログ構成（access,-gc,-consoleログなど）}

Karafでは `console.log` や `fuse.log` などに分かれていましたが、Spring Bootでは logback（または log4j2）により一元的に管理されます。

#### **標準構成ファイル：`logback-spring.xml`** {#標準構成ファイル：logback-spring.xml}

```xml
<configuration>
  <appender name="CONSOLE" class="ch.qos.logback.core.ConsoleAppender">
    <encoder>
      <pattern>%d{yyyy-MM-dd HH:mm:ss} [%thread] %-5level %logger{36} - %msg%n</pattern>
    </encoder>
  </appender>

  <root level="INFO">
    <appender-ref ref="CONSOLE"/>
  </root>

  <logger name="org.apache.camel" level="DEBUG"/>
</configuration>
```

#### **ログ種別と対応表** {#ログ種別と対応表}

| 目的 | Karafログファイル例 | Spring Bootでの扱い |
| ----- | ----- | ----- |
| アクセスログ | access.log | filterやinterceptorで明示的に出力 |
| Web UIアクセスログ | web\_mng\_access.log | N/A（外部UIに依存） |
| GCログ | gc\_pid～.log | JVM引数で出力設定（例：`-Xlog:gc`） |
| コンソールログ | console.log | 標準出力（logback構成で制御） |
| Camelログ | fuse.log / camel.log | logback上で `org.apache.camel` を制御 |

✅ 特殊ログ（GCなど）は JVM レベルで制御、Camel特有のログは logger 名で細かく出力レベルを調整できます。

* ## 6.3 Actuatorによるヘルスチェック {#6.3-actuatorによるヘルスチェック}

Spring Bootでは、`spring-boot-starter-actuator` を導入することで `/actuator/health` 等のエンドポイントが有効になり、プローブや監視が容易になります。

#### **依存追加（pom.xml）** {#依存追加（pom.xml）}

```xml
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-actuator</artifactId>
</dependency>
```

#### **設定（application.yaml）** {#設定（application.yaml）}

```
management:
  endpoints:
    web:
      exposure:
        include: health,info
  endpoint:
    health:
      show-details: always
```

#### **出力例** {#出力例}

```json
{
  "status": "UP",
  "components": {
    "camel": {
      "status": "UP",
      "details": {
        "context": "camel-springboot-app",
        "status": "Started",
        "uptime": "5 minutes"
      }
    }
  }
}
```

✅ Camelの状態も `/actuator/health` に統合されており、ルートの死活監視にも活用可能です。

* ## 6.4 BacklogTracer／Tracerの使い方と変更点 {#6.4-backlogtracer／tracerの使い方と変更点}

Camelには、ルート実行のトレース機能として `Tracer` および `BacklogTracer` が用意されています。

#### **BacklogTracer：ルートの「最後のメッセージ」を記録** {#backlogtracer：ルートの「最後のメッセージ」を記録}

```java
BacklogTracer tracer = camelContext.getExtension(BacklogTracer.class);
tracer.setEnabled(true);
tracer.setBacklogSize(100);
tracer.dumpTracedMessages("myRouteId").forEach(System.out::println);
```

#### **Tracer：全メッセージの逐次トレース（パフォーマンスへの影響大）** {#tracer：全メッセージの逐次トレース（パフォーマンスへの影響大）}

```java
Tracer tracer = new Tracer();
tracer.setLogName("my.camel.trace");
tracer.setLogLevel("INFO");
camelContext.addInterceptStrategy(tracer);
```

#### **注意点（Camel 4.10での変更）** {#注意点（camel-4.10での変更）}

* `camelContext.getBacklogTracer()` は非推奨  
* `getExtension()` で明示的にトレース機能を取得する必要あり  
* パフォーマンスへの影響があるため、本番環境では原則無効化推奨

#### **Spring Bootでの有効化例（application.properties）** {#spring-bootでの有効化例（application.properties）}

```
camel:
  springboot:
    backlogTracerEnabled: true
    backlogTracerBacklogSize: 200
```

✅ トレースはユニットテストやPoC検証時に有効。運用ではActuatorやログでの代替監視を推奨。

この章では、Camel for Spring Bootにおけるテスト実装、ログ管理、ヘルスチェック、ルートトレースの手法を整理しました。次章では、アプリケーションのビルド、実行、運用監視に関する具体的な手順と推奨設定を解説します。

# 第7章：運用・監視・ビルド・デプロイ {#第7章：運用・監視・ビルド・デプロイ}

本章では、Camel for Spring Boot アプリケーションの運用・監視・デプロイに関する基本的な方法を解説します。従来のKarafとは異なり、Spring Bootでは fat JAR を用いた単一プロセスの起動や Actuator ベースの監視が主流となります。また、外部構成管理やJMX、HawtIOの活用についても触れます。

* ## 7.1 Fat JARの作成と実行方法 {#7.1-fat-jarの作成と実行方法}

Camel for Spring Boot アプリケーションは、すべての依存を含む **Fat JAR（実行可能JAR）** としてビルド・配布されます。

#### **Fat JAR作成手順（Maven）** {#fat-jar作成手順（maven）}

1. `pom.xml` に Spring Boot Plugin を追加済みであることを確認：

```xml
<build>
  <plugins>
    <plugin>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-maven-plugin</artifactId>
    </plugin>
  </plugins>
</build>
```

2. 以下のコマンドでビルド：

```shell
mvn clean package
```

3. 作成されたJARファイルを実行：

```shell
java -jar target/camel-springboot-app-1.0.0.jar
```

✅ `application.properties` で `server.port` を変更すれば任意のポートで起動可能です。

* ## 7.2 Mavenビルドと依存関係管理 {#7.2-mavenビルドと依存関係管理}

Camel 4.10では、コンポーネントベースでの依存管理が徹底されています。使用するコンポーネントを明示的に `pom.xml` に定義する必要があります。

#### **依存の定義例（HTTP, CXF, XML DSL）** {#依存の定義例（http,-cxf,-xml-dsl）}

```xml
<dependencies>
  <!-- Camel Spring Boot エンジン -->
  <dependency>
    <groupId>org.apache.camel.springboot</groupId>
    <artifactId>camel-spring-boot-starter</artifactId>
  </dependency>

  <!-- XML IO DSL 用ライブラリ -->
  <dependency>
    <groupId>org.apache.camel</groupId>
    <artifactId>camel-xml-io-dsl</artifactId>
  </dependency>

  <!-- 必要なコンポーネント -->
  <dependency>
    <groupId>org.apache.camel.springboot</groupId>
    <artifactId>camel-http-starter</artifactId>
  </dependency>
  <dependency>
    <groupId>org.apache.camel.springboot</groupId>
    <artifactId>camel-cxf-starter</artifactId>
  </dependency>
</dependencies>
```

✅ Karafの features.xml による依存管理とは異なり、**ビルド時にすべての依存を解決**するのがSpring Boot方式です。

* ## 7.3 外部プロパティ管理（ConfigMap非使用前提） {#7.3-外部プロパティ管理（configmap非使用前提）}

Spring Bootでは、環境ごとに設定ファイルを分けることができます。外部構成管理にKubernetesのConfigMapなどを使わない場合でも、以下のように柔軟に管理できます。

#### **プロファイルごとのプロパティファイル** {#プロファイルごとのプロパティファイル}

```
# application-dev.yaml
spring:
  config:
    activate:
      on-profile: dev
server:
  port: 8081
camel:
  springboot:
    name: camel-app-dev
```

```
# application-prod.yaml
spring:
  config:
    activate:
      on-profile: prod
server:
  port: 8080
camel:
  springboot:
    name: camel-app-prod
```

#### **実行時にプロファイル指定** {#実行時にプロファイル指定}

```shell
java -jar target/app.jar --spring.profiles.active=dev
```

#### **コマンドラインからの上書きも可能** {#コマンドラインからの上書きも可能}

```shell
java -jar app.jar --server.port=9090
```

✅ OSGi ConfigAdminや `.cfg` ファイルのような構成は不要。単一ファイルかつ明快な優先順位制御（コマンドライン \> プロファイル \> デフォルト）で管理できます。

* ## 7.4 JMX監視とHawtIO（Spring Bootでの使い方） {#7.4-jmx監視とhawtio（spring-bootでの使い方）}

KarafではJMXおよびKaraf Console経由でのルート監視が一般的でしたが、Spring BootでもJMX監視と **HawtIO** を組み合わせて利用できます。

#### **JMX有効化** {#jmx有効化}

`application.yaml` に以下を追加：

```
spring:
  jmx:
    enabled: true
camel:
  springboot:
    jmxEnabled: true
```

JMXを有効化すると、`jconsole` や `VisualVM` で CamelContext、ルート、プロセッサなどのメトリクスを確認可能になります。

#### **HawtIO導入（埋め込み）** {#hawtio導入（埋め込み）}

1. 依存追加：

```xml
<dependency>
  <groupId>io.hawt</groupId>
  <artifactId>hawtio-springboot</artifactId>
  <version>4.2.0.redhat-00034</version>
</dependency>
```

2. `application.yaml` 設定例：

```
hawtio:
  authenticationEnabled: false
  proxyAllowed: true

management:
  endpoints:
    web:
      exposure:
        include: "*"
```

3. アクセス方法：

```
http://localhost:8080/hawtio
```

ログイン不要でCamelルートの状態や履歴、メッセージのトレースなどが可視化できます。

✅ Karafと同様の視認性をHawtIOで確保可能。軽量なUIツールとしてローカル環境・検証環境に最適です。

---

この章では、Camel for Spring Bootアプリケーションのビルド方法（fat JAR）、構成管理、JMX・HawtIOを活用した監視・運用管理の方法について解説しました。  
 次章では、Blueprint DSLから XML IO DSLへの実践的な移行演習に進みます。

# 

# 第8章：移行演習 {#第8章：移行演習}

本章では、Karaf環境で作成されたBlueprint DSLベースのCamelルートを、Camel 4.10＋Spring Boot 3.x環境で動作する**XML IO DSL**へ変換する演習を通じて、実践的な移行スキルを養います。特に、`<cxf:cxfEndpoint>` を含むSOAPルートや、例外処理付きのルート構造再現を中心に解説します。

* ## 8.1 Blueprintルート → XML IO DSLへの変換演習 {#8.1-blueprintルート-→-xml-io-dslへの変換演習}

#### **Blueprint DSL での例（before）** {#blueprint-dsl-での例（before）}

```xml
<blueprint xmlns="http://www.osgi.org/xmlns/blueprint/v1.0.0"
           xmlns:camel="http://camel.apache.org/schema/blueprint">

  <camel:camelContext id="ctx">
    <camel:route id="sample-route">
      <camel:from uri="direct:start"/>
      <camel:setBody>
        <camel:constant>Hello Blueprint</camel:constant>
      </camel:setBody>
      <camel:to uri="log:result"/>
    </camel:route>
  </camel:camelContext>
</blueprint>
```

#### **XML IO DSL での例（after）** {#xml-io-dsl-での例（after）}

```xml
<routes xmlns="http://camel.apache.org/schema/xmlio">
  <route id="sample-route">
    <from uri="direct:start"/>
    <setBody>
      <constant>Hello Blueprint</constant>
    </setBody>
    <to uri="log:result"/>
  </route>
</routes>
```

#### **変換ポイント** {#変換ポイント}

| 項目 | Blueprint DSL | XML IO DSL（Camel 4） |
| ----- | ----- | ----- |
| **名前空間** | `http://camel.apache.org/schema/blueprint` | `http://camel.apache.org/schema/xmlio` |
| **ルート配置** | `<camelContext>` を `<blueprint>` 配下に配置 | `<routes>` がルート定義のトップレベル |
| **DI構文** | `<reference>` による OSGi サービス参照 | Bean定義はJavaコードまたは外部DI（Spring Boot等）に依存 |
| **CamelContext定義** | `<camelContext>` 要素あり（明示的） | `<routes>` が CamelContext に直接バインドされる |
| **property読み込み** | `<property-placeholder>` 要素で設定 | DSL内では非対応（外部設定：application.yaml 等） |
| **XSD/Schema構文** | Blueprint用の XSD を使用 | `camel-xml-io.xsd` に準拠 |

✅ **補足ポイント**：

* XML IO DSL では `camelContext` タグは不要で、ルートは `<routes>` 配下に直接記述されます。  
* DI（依存性注入）は、Blueprintの `<reference>` とは異なり、Camel XML IO DSL には存在せず、代わりに Spring Boot 側の Bean 定義に委ねられます。  
* 設定プロパティ（`property-placeholder` 相当）は、application.yaml/properties などの外部ファイルで定義します。  
* CamelContext の設定や onException、errorHandler なども `<routes>` 内で定義可能です。

* ## 8.2 `cxf:` コンポーネントを含むSOAPルートの移行 {#8.2-cxf:-コンポーネントを含むsoapルートの移行}

Karafで `cxf:` を使っていたSOAPルートは、Camel 4.10 \+ Spring Boot 環境においても `camel-cxf-starter` と `camel-xml-io-dsl` を利用することで移行可能です。。

#### **Blueprint DSL の例（before）** {#blueprint-dsl-の例（before）}

```xml
<camel:route id="soapRoute" xmlns:camel="http://camel.apache.org/schema/blueprint">
  <camel:from uri="cxf:/hello?serviceClass=com.example.HelloService"/>
  <camel:to uri="bean:helloProcessor"/>
</camel:route>
```

#### **XML IO DSL の例（after）** {#xml-io-dsl-の例（after）}

```xml
<routes xmlns="http://camel.apache.org/schema/xmlio">
  <route id="soapRoute">
    <from uri="cxf:/hello?serviceClass=com.example.HelloService"/>
    <to uri="bean:helloProcessor"/>
  </route>
</routes>
```

#### **🔍 補足と注意点** {#🔍-補足と注意点}

* cxf:/hello のように URIベースで直接定義可能。  
* Blueprint DSLで用いていた \<cxf:cxfEndpoint\> 要素は不要。  
* serviceClass には JAX-WS アノテーションを付与したインターフェースを指定。  
* camel-cxf-starter を pom.xml に追加する必要があります。

```xml
<dependency>
  <groupId>org.apache.camel.springboot</groupId>
  <artifactId>camel-cxf-starter</artifactId>
</dependency>
```

✅ **Camel 4 \+ Spring Boot \+ XML IO DSL** 環境でも `cxf:` は引き続き利用可能です。Blueprint DSL からの移行も比較的簡単に実現できます。必要に応じて `application.yaml` でJMXやログ、エンドポイントの調整を行ってください。

* ## 8.3 ルーティングとException処理の構造再現演習 {#8.3-ルーティングとexception処理の構造再現演習}

Camel ルートの移行では、ルーティングだけでなく `<onException>` による例外処理構造も重要です。XML IO DSL を使用することで、Blueprint DSL と同様の例外ハンドリング構造を再現できます。

#### **Blueprint DSL の例（before）** {#blueprint-dsl-の例（before）-1}

```xml
<onException>
  <exception>java.lang.Exception</exception>
  <handled>
    <constant>true</constant>
  </handled>
  <to uri="log:exception"/>
</onException>

<route id="exception-test">
  <from uri="direct:start"/>
  <to uri="bean:faultyProcessor"/>
  <to uri="log:success"/>
</route>
```

#### **XML IO DSL の例（after）** {#xml-io-dsl-の例（after）-1}

```xml
<routes xmlns="http://camel.apache.org/schema/xmlio">

  <onException>
    <exception>java.lang.Exception</exception>
    <handled>
      <constant>true</constant>
    </handled>
    <to uri="log:exception"/>
  </onException>

  <route id="exception-test">
    <from uri="direct:start"/>
    <to uri="bean:faultyProcessor"/>
    <to uri="log:success"/>
  </route>

</routes>
```

* ### **🔍 移行ポイント（XML IO DSL向け）** {#🔍-移行ポイント（xml-io-dsl向け）}

| 項目 | 説明 |
| ----- | ----- |
| `<onException>` | Blueprint DSL と構文は共通で、XML IO DSL でもほぼそのまま利用可能です。 |
| `<routes>` ルート定義 | `camelContext` の代わりに `<routes xmlns="http://camel.apache.org/schema/xmlio">` を使用します。 |
| DIの構文 | `bean:` URIで呼び出されるクラスは、Spring Boot 側のコンテキストまたは手動登録で解決されます。 |
| Camel 4の補足 | `DefaultErrorHandler` や `retryWhile` など、より強力な例外処理構成が利用可能です。 |

✅ **XML IO DSL** では、Karaf Blueprint DSL と同様の `<onException>` 構造がサポートされています。Camel 4 でのマイグレーション時は、ルート本体とあわせて例外処理の再現性も検証してください。

---

この章では、Blueprint DSL で作成されたルートを XML IO DSL に移行する実践演習を通じて、構文変換・依存関係・ルート設計の再現方法を習得しました。最終章では、Camel for Spring Boot を本番運用する際の設計上のベストプラクティスをまとめます。

# 

# 第9章：まとめと今後の運用設計 {#第9章：まとめと今後の運用設計}

本章では、Camel 4.10 \+ Spring Boot \+ **XML IO DSL** の導入によるメリットを整理し、Karaf環境からの段階的な移行と、移行後の運用設計のポイントについてまとめます。

* ## 9.1 Camel for Spring Bootの導入効果 {#9.1-camel-for-spring-bootの導入効果}

Camel for Spring Bootを採用することで、以下のような効果が期待できます：

| 項目 | 導入効果 |
| ----- | ----- |
| 開発効率 | Spring Boot の自動構成と Camel のシンプルな XML IO DSL により、設定が簡素化 |
| テスト容易性 | `camel-test-spring-junit5` によるユニットテスト構成が容易 |
| ビルド／デプロイ | Fat JAR 形式での配布により、Karafの OSGi構成不要 |
| 構成管理 | `application.yaml` による一元管理とプロファイル切替 |
| 運用・監視 | Actuator、HawtIO、JMX による柔軟な監視体制 |
| 将来性 | Camel 4.x \+ Spring Boot 3.x の最新標準スタックに準拠 |

✅ Blueprint DSLやKarafに依存しない設計により、**保守性とポータビリティが向上**します。

* ## 9.2 段階的移行戦略（モジュール単位移行、ゼロダウンタイム等） {#9.2-段階的移行戦略（モジュール単位移行、ゼロダウンタイム等）}

KarafからSpring Bootへの全面移行は一括ではなく、段階的に進めることでリスクを最小化できます。

#### **推奨される段階的移行アプローチ** {#推奨される段階的移行アプローチ}

1. **現行Blueprint DSLの棚卸し**

   * 各バンドルの責務と依存関係を明確化

2. **ルート単位でのXML IO DSL移行**

   * 検証済みルートから順次Spring Boot化し、ユニットテストを追加

3. **段階的切り替え**

   * ルート単位でRESTエンドポイントやQueueなどのI/F経由で段階的に新旧を併用

4. **ゼロダウンタイム戦略**

   * Blue/Green デプロイまたは Canary Release を活用し、切り替えを段階化

5. **運用チームへのドキュメント展開**

   * KarafとSpring Bootの違いを整理した運用マニュアルの整備

✅ モジュール単位での移行を行い、逐次テストとリリースを繰り返すことで、システム全体への影響を抑えたスムーズな移行が可能です。

* ## 9.3 継続的改善に向けた運用設計のポイント {#9.3-継続的改善に向けた運用設計のポイント}

移行後も持続的な運用改善を実現するには、以下の運用設計ポイントを押さえることが重要です。

#### **1\. 可視化と監視の強化** {#1.-可視化と監視の強化}

* /actuator/health でのルート状態確認  
* HawtIOやJMXによるトレースの活用  
* JSON構造化ログ導入によるログ標準化

#### **2\. プロパティと機密情報の管理** {#2.-プロパティと機密情報の管理}

* `application.yaml` による環境切替

#### **3\. CI/CDの整備** {#3.-ci/cdの整備}

* Maven \+ GitHub Actions による自動テスト＋ビルド  
* 静的解析やセキュリティスキャン統合

#### **4\. アップグレード対応と技術継承** {#4.-アップグレード対応と技術継承}

* Camelアップデートに伴う廃止機能の確認  
* XML IO DSLの書式チェックやXSDバリデーション導入

✅ 「移行して終わり」ではなく、**移行後に安定した運用と将来の拡張性を担保する設計**が、成功の鍵となります。

**最終まとめ**

本トレーニングを通じて、Apache Camel 4.10 \+ Spring Boot 環境で XML IO DSL によるルート設計・構成・運用を体系的に習得しました。

Blueprint DSL からの移行、設定の近代化、監視体制の確立により、開発と保守の両面で高い柔軟性と生産性が期待できます。

今後は、以下のステップでさらなる改善・活用を目指しましょう：

* 自動テスト／監視の体制整備（CI＋可視化）  
* カスタムEIPやProcessorの適用拡大  
* チーム標準の整備（構成／実装／ドキュメント）

