# Camel on SpringBoot on RHEL環境設定

## **✅ 構成概要**

| 項目 | 内容 |
| ----- | ----- |
| OS | RHEL 10（仮想環境） |
| JDK | JDK 21 |
| ビルド | Maven 3.9.9 |
| IDE | Visual Studio Code（GUIあり） |
| Camel | Camel 4.10.x on Spring Boot 3.4.x |
| DSL | XML IO DSL（`camel-xml-io-dsl-starter`） |

---

## **🧭 手順ガイド**

---

### **① Java 21 のインストール（RHEL 10 標準）**

```shell
sudo dnf install -y java-21-openjdk java-21-openjdk-devel
```

確認：

```shell
java -version
```

---

### **② Maven 3.9.9 のインストール（手動）**

```shell
cd /opt
sudo curl -LO https://dlcdn.apache.org/maven/maven-3/3.9.11/binaries/apache-maven-3.9.11-bin.tar.gz
sudo tar -xzf apache-maven-3.9.11-bin.tar.gz
sudo ln -s apache-maven-3.9.11 maven
```

環境変数（`~/.bashrc` または `~/.zshrc`）に追加：

```shell
export MAVEN_HOME=/opt/maven
export PATH=$MAVEN_HOME/bin:$PATH
```

反映：

```shell
source ~/.bashrc
mvn -v
```

---

### **③ VS Code のインストール（GUI上）**

```shell
sudo rpm --import https://packages.microsoft.com/keys/microsoft.asc
sudo sh -c 'echo -e "[code]\nname=VS Code\nbaseurl=https://packages.microsoft.com/yumrepos/vscode\nenabled=1\ngpgcheck=1\ngpgkey=https://packages.microsoft.com/keys/microsoft.asc" > /etc/yum.repos.d/vscode.repo'
sudo dnf install -y code
```

起動：

```shell
code &
```

---

### **④ VS Code 拡張機能（おすすめ）**

VS Code 内で以下を検索してインストール：

* ✅ `Language Support for Java(TM) by Red Hat`

* ✅ `Kaoto`

* ✅ `Maven for Java`

* ✅ `XML`

* ✅ `YAML`

* ⬜（必要に応じて）`REST Client` や `Debugger for Java`

---

### **⑤ Camel Spring Boot プロジェクトの作成**

すでに GitLab や GitHub にある場合は clone：

```shell
git clone https://github.com/keijijin/camel-handson.git
cd camel-handson/camel-handson-training
```

または Maven で新規作成：

```shell
mvn archetype:generate \
  -DarchetypeGroupId=org.apache.camel.archetypes \
  -DarchetypeArtifactId=camel-archetype-spring-boot \
  -DarchetypeVersion=4.10.3.redhat-00023 \
  -DgroupId=com.example \
  -DartifactId=camel-demo \
  -Dversion=1.0.0-SNAPSHOT \
  -DinteractiveMode=false
```

依存に以下を追加（pom.xml）：

```xml
<dependency>
  <groupId>org.apache.camel.springboot</groupId>
  <artifactId>camel-xml-io-dsl-starter</artifactId>
</dependency>
<dependency>
  <groupId>org.apache.camel.springboot</groupId>
  <artifactId>camel-platform-http-starter</artifactId>
</dependency>
<dependency>
  <groupId>org.apache.camel.springboot</groupId>
  <artifactId>camel-http-starter</artifactId>
</dependency>
```

---

### **⑥ `application.yaml` 設定**

```
camel:
  springboot:
    routes-include-pattern: classpath:routes/*.xml
```

---

### **⑦ ルートファイル例（`src/main/resources/routes/my-routes.xml`）**

```xml
<routes xmlns="http://camel.apache.org/schema/io">
  <route id="hello-route">
    <from uri="platform-http:/hello"/>
    <setBody>
      <constant>Hello from Camel on RHEL 10!</constant>
    </setBody>
  </route>
</routes>
```

---

### **⑧ 実行と確認**
- 開発・テスト用
```shell
mvn clean spring-boot:run
```
- 本番用
```shell
mvn clean package
java -jar target/camel-handson-training-1.0.0-SNAPSHOT.jar
```

動作確認：

```shell
curl http://localhost:8080/hello
```

---

## **✅ 補足：便利なコマンドやツール**

| ツール | 内容 |
| ----- | ----- |
| `camel run routes.xml` | JBang \+ Camel CLI で軽量実行も可能（任意） |
| `HawtIO` | 管理UI（別途導入可能） |
| `Kaoto` | CamelルートのGUI設計（VS Code拡張可） |


