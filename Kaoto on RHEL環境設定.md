# Kaoto on RHEL環境設定

RHEL 10 環境で **Kaoto を使って Camel ルートを編集し、`jbang` \+ `camel` CLI でローカル実行する環境**を構築するための手順を以下にガイドします。

---

## **✅ 構成目的**

| 項目 | 内容 |
| ----- | ----- |
| 開発支援ツール | Kaoto（ルートGUI設計） |
| 実行エンジン | JBang \+ Camel CLI（`camel run`） |
| Camel DSL | XML IO DSL（.xml でルート定義） |
| IDE | VS Code（Kaoto 拡張付き） |

---

## **🧭 手順ガイド：Kaoto \+ JBang \+ Camel CLI**

---

### **① 前提：JDK 17+ がインストール済（JDK 21 可）**

```shell
java -version
# openjdk version "21" ... ならOK
```

---

### **② JBang のインストール**

```shell
curl -Ls https://sh.jbang.dev | bash
echo 'export PATH="$HOME/.jbang/bin:$PATH"' >> ~/.bashrc
```

その後、`~/.bashrc` や `~/.zshrc` に自動で PATH が追加されるので、反映：

```shell
source ~/.bashrc
jbang version
```

---

### **③ Camel CLI のインストール（JBang経由）**

```shell
jbang app install camel@apache/camel
```

確認：

```shell
camel --version
# Apache Camel JBang: 4.10.x など
```

---

### **④ Kaoto（VS Code 拡張）の導入**

VS Code を起動：

```shell
code &
```

拡張機能を検索してインストール：

* ✅ `Kaoto`

* ✅ `Language Support for Java(TM) by Red Hat`

* ✅ `YAML`, `XML`, `Maven for Java`

---

### **⑤ Kaoto でルートを作成（XML IO DSL）**

1. VS Code の Kaoto タブで新規ルート作成

2. `from → setBody → to` などを追加し構築

3. 「ソース」タブで XML に切り替え

4. 保存時に例：

```xml
<routes xmlns="http://camel.apache.org/schema/io">
  <route id="hello-kaoto">
    <from uri="timer:tick?period=5s"/>
    <setBody>
      <constant>Hello from Kaoto + Camel CLI</constant>
    </setBody>
    <to uri="log:kaoto-log"/>
  </route>
</routes>
```

---

### **⑥ camel CLI で XML ルートを実行**

保存したルートファイル（例: `kaoto-route.xml`）があるディレクトリで：

```shell
camel run kaoto-route.xml
```

出力例：

```
[INFO] ... Hello from Kaoto + Camel CLI
```

---

## **✅ 便利なオプションと補足**

| コマンド | 説明 |
| ----- | ----- |
| `camel init --dsl xml` | 雛形プロジェクト生成 |
| `camel run src/routes/` | ディレクトリごと実行可能 |
| `camel export --gav` | JBang ルートから Maven プロジェクトへ変換可能 |

---

## **📦 おすすめディレクトリ構成例**

```
kaoto-camel/
├── kaoto-route.xml
├── README.md
```

`README.md` に curl や `camel run` コマンドを書いておくと Kaoto GUI との連携説明がしやすくなります。

---

## **✅ まとめ**

| ステップ | 内容 |
| ----- | ----- |
| 1 | JDK 21 \+ JBang を導入 |
| 2 | `camel@apache/camel` を JBang 経由で install |
| 3 | VS Code に Kaoto 拡張を導入 |
| 4 | XML IO DSL ルートを Kaoto で作成・編集 |
| 5 | `camel run myroute.xml` で即実行 |

