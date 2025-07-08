# Flutter Tips
メモ程度の内容ですので正確な情報については要確認です。

## 目次
- [Flutterとは](#flutterとは)
- [Dart文法](#dart文法)
- [Flutter環境構築](#flutter環境構築)
- [Flutterのプロジェクトを作成する](#flutterのプロジェクトを作成する)
- [main.dartについて](#maindartについて)
- [ウィジェット関連](#ウィジェット関連)
  - [ボタンウィジェット](#ボタンウィジェット)
  - [入力系ウィジェット](#入力系ウィジェット)
  - [アラートとダイアログ](#アラートとダイアログ)
- [ナビゲーション・ルーティング](#ナビゲーション・ルーティング)
- [執筆したFlutterに関する記事](#執筆したFlutterに関する記事)
- [参考文献](#参考文献)  

## Flutterとは
Flutterは、Googleによって開発されたオープンソースのUIツールキット。  
単一のコードベースから、モバイル、ウェブ、デスクトップ向けのネイティブ同様のアプリケーションを構築できる。  
[Flutter - Build apps for any screen](https://flutter.dev/)

## Dart文法  
<details>
<summary>クリックして展開</summary>
<br>
<details>
<summary><strong>1. 基本構造と実行</strong></summary>
<br>
  
Dartプログラムは `main()` 関数から実行が開始される。
```dart
void main() {
  print("Hello, Dart!");
}
```
コメントは `//`  複数行コメントは `/* ... */` で記述する。  
Pythonとは違い、基本的には文の終わりに `;` を付ける。  
Dartでは、全ての値がオブジェクトとして扱われる。（`Object` クラスを継承）  
</details>

<details>
<summary><strong>2. 変数</strong></summary>
<br>

`var` 型推論により変数の型が決定される。一度型が決定されると、異なる型の値は代入できない。  
```dart
var name = "Dart"; // String型と推論される
name = 123; // エラー
```
型を明示的に指定することも可能。
```dart
String language = "Dart";
int version = 3;
```
`dynamic` はあらゆる型の値を代入可能だが、型安全性が低下する。
```dart
dynamic anything = "Hello";
anything = 100; // OK
```
初期化されていない変数のデフォルト値は `null` 。  
変数が有効な範囲は、基本的に波括弧 `{}` で囲まれたブロック内。（ローカルスコープ）
```dart
int count; // null

{
  int n = 21;
}
print(n); // エラー
```
</details>

<details>
<summary><strong>3. 基本型</strong></summary>
<br>
  
数値型
```dart
int integer = 123; // 整数
double point = 3.14; // 浮動小数点数
num parent = 10; // intとdoubleの親クラス
```
文字列型（シングルクォート `'...'` またはダブルクォート `"..."` で囲む）
```dart
String message = "Hello";
String response = 'Hi';
```
真偽値型
```dart
bool enabled = true;
bool isAuto = false;
```
コレクション型
```dart
// 順序あり、重複あり、インデックスで検索可
List<int> nums = [1, 2, 3];
// 順序の保証無し、重複無し
Set<String> colors = {"Red", "Green", "Blue"};
// キーと値のペアを保持する
Map<String, int> ranks = {
  "Red": 1,
  "Green": 2,
  "Blue": 3
};
```
</details>

<details>
<summary><strong>4. 定数</strong></summary>
<br>
  
`final` は実行時に一度だけ値を代入でき、その後は変更できない。
```dart
final String appName = "My Awesome App";
appName = "Another App"; // エラー
```
`const` はコンパイル時に値が確定するため、ビルド時点で値が決まっている必要がある。
```dart
const double pi = 3.14159;
```
</details>

<details>
<summary><strong>5. 関数</strong></summary>
<br>

関数の戻り値の型、関数名、パラメータリスト、関数本体で構成される。
```dart
int add(int a, int b) {
  return a + b;
}
```
関数本体が単一の式のみである場合、アロー構文で簡潔に書ける。
```dart
int multiply(int a, int b) => a * b;
```
戻り値が無い場合は `void` を指定する。
```dart
void printMessage(String message) {
  print(message);
}
```
名前付き引数  
`{}` で囲み、呼び出し時に引数名を指定する。順序は任意。デフォルト値を設定できる。
```dart
void enableFlags({bool bold = false, bool hidden = false}) {
  // ...
}
enebaleFlags(hidden: true); // boldはデフォルト値のfalse
```
オプション  
`[]` で囲み、オプションとする。
```dart
String say(String from, String msg, [String? device]) {}
```
無名関数（ラムダ式/クロージャ）  
名前を持たない関数で、他の関数の引数として渡したり、変数に代入したりできる。
```dart
var add = (int a, int b) {
  return a + b;
};
```
</details>

<details>
<summary><strong>6. 演算子</strong></summary>
<br>

* **算術演算子:** `+`, `-`, `*`, `/` (浮動小数点数除算), `~/` (整数除算), `%` (剰余)。
* **代入演算子:** `=`, `+=`, `-=`, `*=`, `/=`, `~/=`, `%=`。
* **null関連演算子:**
    * `??`: 左辺が `null` の場合に右辺の値を返す。(例: `String? name; print(name ?? 'Guest');`)
    * `??=`: 左辺が `null` の場合に右辺の値を代入する。(例: `int? count; count ??= 0;`)
* **条件演算子 (三項演算子):** `条件式 ? 真の場合の値 : 偽の場合の値`。
* **比較演算子:** `==` (等価), `!=` (非等価), `>`, `<`, `>=`, `<=`。
* **論理演算子:** `!` (NOT), `||` (OR), `&&` (AND)。
* **型テスト演算子:**
    * `is`: オブジェクトが指定した型である場合に `true`。
    * `is!`: オブジェクトが指定した型でない場合に `true`。

</details>

<details>
<summary><strong>7. 制御フロー文</strong></summary>
<br>

* **`if-else` 文:** 条件に基づいて処理を分岐する。
    ```dart
    if (score >= 80) {
      print('Great!');
    } else if (score >= 60) {
      print('Good.');
    } else {
      print('Try again.');
    }
    ```
* **ループ文:**
    * **`for` ループ:**
        ```dart
        for (int i = 0; i < 5; i++) {
          print(i);
        }
        ```
    * **`for-in` ループ:** リストやセットなどのコレクションの要素を反復処理する。
        ```dart
        var fruits = ['apple', 'banana', 'orange'];
        for (var fruit in fruits) {
          print(fruit);
        }
        ```
    * **`forEach` メソッド:** `List` などのコレクションが持つメソッドで、各要素に対して処理を行う。
        ```dart
        fruits.forEach((fruit) => print(fruit.toUpperCase()));
        ```
    * **`while` ループ:** 条件が真である間、処理を繰り返す。
        ```dart
        int count = 0;
        while (count < 3) {
          print('Count is $count');
          count++;
        }
        ```
    * **`do-while` ループ:** 処理を一度実行してから条件を評価し、真であれば繰り返す。
        ```dart
        int num = 0;
        do {
          print('Number is $num');
          num++;
        } while (num < 0); // 少なくとも一度は実行される
        ```
* **`break` と `continue`:**
    * `break`: ループ処理を中断する。
    * `continue`: 現在のループをスキップし、次のループに進みます。
* **`switch-case` 文:** 特定の値に基づいて処理を分岐する。`case` には定数式を使用する。各 `case` の最後には通常 `break` を記述します。Dart3からfall throughしなくなった。空のcaseのみfall throughする。
    ```dart
    var command = 'OPEN';
    switch (command) {
      case 'OPEN':
        print('Opening...');
        break;
      case 'CLOSED':
        print('Closing...');
        // fall throughしない
      case 'def': // これはfall throughする
      default:
        print('Unknown command');
    }
    ```
</details>

<details>
<summary><strong>8. 例外処理</strong></summary>
<br>

予期しないエラーが発生した場合の処理を記述する。

* **`throw`:** 例外を意図的に発生させる。任意のオブジェクトを `throw` できるが、`Exception` や `Error` のサブクラスが一般的。
    ```dart
    // throw FormatException('Invalid format.');
    // throw 'Something went wrong!';
    ```
* **`try-catch-finally`:**
    * `try`: 例外が発生する可能性のあるコードを記述する。
    * `on <例外型>`: 特定の型の例外をキャッチする。
    * `catch (e, s)`: `e` に例外オブジェクト、`s` にスタックトレースオブジェクトが渡される。型を指定しない `catch` はあらゆる例外をキャッチする。
    * `finally`: 例外の発生有無にかかわらず、最後に必ず実行される処理を記述する。
    ```dart
    try {
      var result = 100 ~/ 0; // ゼロ除算エラー
      print(result);
    } on IntegerDivisionByZeroException {
      print('Cannot divide by zero.');
    } catch (e, s) {
      print('An unexpected error occurred: $e');
      print('Stack trace:\n$s');
    } finally {
      print('Cleanup actions.');
    }
    ```
</details>

<details>
<summary><strong>9. クラス（オブジェクト指向）</strong></summary>
<br>

Dartはクラスベースのオブジェクト指向言語。

* **定義:** `class` キーワードを使ってクラスを定義する。
    ```dart
    class Point {
      double x = 0; // インスタンス変数（プロパティ）
      double y = 0;

      // コンストラクタ
      Point(double xValue, double yValue) {
        this.x = xValue;
        this.y = yValue;
      }

      // 名前付きコンストラクタ
      Point.origin() {
        this.x = 0;
        this.y = 0;
      }

      // メソッド
      double distanceTo(Point other) {
        var dx = x - other.x;
        var dy = y - other.y;
        return (dx * dx + dy * dy); // Math.sqrtを使いたいが、ここでは簡略化
      }
    }
    ```
* **インスタンス化:** `new` キーワードは任意。
    ```dart
    var p1 = Point(10, 20);
    var p2 = Point.origin();
    print(p1.x); // メンバーへのアクセスはドット(.)
    print(p1.distanceTo(p2));
    ```
* **コンストラクタ:** インスタンス生成時に初期化処理を行う。クラス名と同じ名前のメソッドで定義する。`this` は現在のインスタンスを指す。
    * **シンタックスシュガー:** コンストラクタの引数を直接インスタンス変数に代入できる。
        ```dart
        class Point {
          double x, y;
          Point(this.x, this.y); // this.x = x; this.y = y; と同等
        }
        ```
    * **イニシャライザリスト:** コンストラクタ本体が実行される前にインスタンス変数を初期化する。`final` 変数の初期化や、スーパークラスのコンストラクタ呼び出しなどに使う。
        ```dart
        class Rectangle {
          final double left, top, width, height;
          final double area;

          Rectangle(this.left, this.top, this.width, this.height)
              : area = width * height; // イニシャライザリスト
        }
        ```
* **Getter と Setter:** プロパティへのアクセスを制御するために、`get` と `set` キーワードを使って特別なメソッドを定義できる。
    ```dart
    class Rectangle {
      double left, top, width, height;
      Rectangle(this.left, this.top, this.width, this.height);

      double get right => left + width;
      set right(double value) => left = value - width;
    }
    ```
* **プライベートメンバー:** 変数名やメソッド名の先頭にアンダースコア `_` を付けると、そのメンバーはライブラリプライベート（同じファイル内からのみアクセス可能）になる。
* **継承 (`extends`):** あるクラスの機能を引き継いで新しいクラスを作成する。スーパークラスのコンストラクタは `super()` で呼び出す。
    ```dart
    class Animal {
      String name;
      Animal(this.name);

      void speak() {
        print('Animal sound');
      }
    }

    class Dog extends Animal {
      Dog(String name) : super(name); // スーパークラスのコンストラクタを呼び出す

      @override // メソッドのオーバーライド
      void speak() {
        print('$name says Woof!');
      }
    }
    ```
* **インターフェース (`implements`):** Dartには明示的な `interface` キーワードはない。すべてのクラスは暗黙的にインターフェースを定義する。他のクラスのインターフェースを実装するには `implements` キーワードを使用し、そのインターフェースのすべてのメソッドを実装する必要がある。
* **抽象クラス (`abstract`):** インスタンス化できないクラスで、サブクラスに共通のインターフェースや部分的な実装を提供する。抽象メソッド（本体を持たないメソッド）を持つことができる。
* **ミックスイン (`mixin`, `with`):** クラスの機能を他のクラスに「混ぜ込む」仕組み。継承と異なり、階層構造を作らずにコードを再利用できる。ミックスインとして使用するクラスは特定の制約（コンストラクタを持たないなど）がある。
    ```dart
    mixin Swimmer {
      void swim() {
        print('Swimming');
      }
    }

    class Duck with Swimmer {
      // DuckはSwimmerの機能を使える
    }
    ```
</details>

<details>
<summary><strong>10. ライブラリとインポート</strong></summary>
<br>

コードをモジュール化し、再利用するためにライブラリを使用する。

* **`import`:** 他のライブラリの機能を利用するために使用する。
    * Dart標準ライブラリ: `import 'dart:math';`
    * パッケージ (Pubリポジトリから): `import 'package:http/http.dart' as http;` (`as` でプレフィックスを指定可能)
    * ローカルファイル: `import 'src/my_utility.dart';` (相対パスまたは絶対パス)
</details>

<details>
<summary><strong>11. 非同期処理</strong></summary>
<br>

時間のかかる処理（ネットワーク通信、ファイルI/Oなど）を、メインの処理をブロックせずに行うための仕組み。

* **`Future`:** 非同期処理の最終的な結果（成功した値またはエラー）を表すオブジェクト。処理が完了すると `Future` の状態が変化する。
    ```dart
    Future<String> fetchData() {
      return Future.delayed(Duration(seconds: 2), () => 'Data fetched');
    }
    ```
* **`async` と `await`:** 非同期コードを同期的なコードのように記述しやすくするためのキーワード。
    * `async`: 関数が非同期であることを示し、`Future` を返すことを暗黙的に示す（戻り値の型は `Future<T>`）。
    * `await`: `Future` の処理が完了するまで待機する。`await` は `async` とマークされた関数内でのみ使用できる。
    ```dart
    Future<void> printData() async {
      print('Fetching data...');
      String data = await fetchData(); // fetchDataの完了を待つ
      print(data);
    }
    ```
* **`Stream`:** 一連の非同期イベントを扱うためのオブジェクト。データが断続的に複数回送られてくるような場合（ユーザーの連続したクリック、ファイルの読み込みなど）に使用する。
</details>
</details>

## Flutter環境構築
<details>
<summary>クリックして展開</summary>
Visual Studio Code を使用して構築する。


1. 拡張機能を導入する  
https://marketplace.visualstudio.com/items?itemName=Dart-Code.flutter
2. メッセージでFlutterSDKの導入を促されるのでそれに従う

詳細は公式のDocsを参照: https://docs.flutter.dev/get-started/install/windows/mobile#prompt-vs-code-to-install-flutter

</details>

## Flutterのプロジェクトを作成する
- ### VScodeの場合
  コマンドパレット（`Ctrl` + `Shift` + `P`）で `flutter` と入力。  
  `Flutter: New Project` を選択し、`Application` を選ぶ。  
  プロジェクトの保存先を指定し、名前を設定したらプロジェクトが生成される。  

- ### コマンドの場合
  プロジェクトを作りたいディレクトリで `flutter create <プロジェクト名>` と入力。

> 作成されたプロジェクトには様々なフォルダが生成される。  
`lib` フォルダ内にある `main.dart` というスクリプトを変更していく。

## main.dartについて
最初にFlutterアプリのUIウィジェットがまとめられているパッケージをインポートしている。  
`MaterialApp` や `Scaffold` などを利用するためにも必要。
```dart
import 'package:flutter/material.dart';
```
main関数が、アプリを起動する際に呼び出される関数。  
`runApp` はFlutterに表示してほしいウィジェットを伝える命令。
```dart
void main() {
  runApp(const myApp());
}
```
MyAppクラスは状態を保存しない `StatelessWidget` を継承している。  
これは一度表示されら基本的に内容が変わることはない。  
<br>
`build` はMyAppウィジェットがどのように表示されるかを定義するメソッド。  
`MaterialApp` で基本的な設定をしている。  
ここでは home に `MyHomePage` を指定しているため、アプリを起動して最初に表示されるのは MyHomePage の画面となる。
```dart
class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Flutter Demo',
      theme: ThemeData(
        colorScheme: ColorScheme.fromSeed(seedColor: Colors.deepPurple),
      ),
      home: const MyHomePage(title: 'Flutter Demo Home Page'),
    );
  }
}
```
MyHomePage は内部に状態を持つ `StatefulWidget` を継承していて、ユーザーの操作やデータの変更によって画面の内容が変わる可能性がある。  
```dart
class MyHomePage extends StatefulWidget {
  const MyHomePage({super.key, required this.title});

  final String title;

  @override
  State<MyHomePage> createState() => _MyHomePageState();
}
```
`build` ではアプリの見た目などを定義している。  
`Scaffold` は土台という意味があり、引数にappBar、body、その他の要素を設定して画面を構成していく。  
<br>
このコードでは、`FloatingActionButton` が押されたら `_incrementCounter` 関数が呼ばれ、`_counter` が加算される。  
`setState` によって変数の値が変更されるたびに、`build` が実行されて画面の内容が更新される。

```dart
class _MyHomePageState extends State<MyHomePage> {
  int _counter = 0; // ① 状態 (State)

  void _incrementCounter() { // ② 状態を変更するメソッド
    setState(() {
      _counter++;
    });
  }

  @override
  Widget build(BuildContext context) { // ③ UIを構築するメソッド
    return Scaffold(
      appBar: AppBar(
        backgroundColor: Theme.of(context).colorScheme.inversePrimary,
        title: Text(widget.title), // ④ MyHomePageからtitleを受け取る
      ),
      body: Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: <Widget>[
            const Text('You have pushed the button this many times:'),
            Text(
              '$_counter', // ⑤ 状態を表示
              style: Theme.of(context).textTheme.headlineMedium,
            ),
          ],
        ),
      ),
      floatingActionButton: FloatingActionButton(
        onPressed: _incrementCounter, // ⑥ ボタンが押されたら②を呼ぶ
        tooltip: 'Increment',
        child: const Icon(Icons.add),
      ),
    );
  }
}
```

## ウィジェット関連
画面のレイアウト作成にはflutterstudioを活用するといい。  

> Flutter Studio  
https://flutterstudio.app/

## ボタンウィジェット
### [TextButton](https://api.flutter.dev/flutter/material/TextButton-class.html)
デフォルトでは文字が表示されるだけのボタン。  
`onPressed` でボタンをクリックした時の処理を指定できる。  
`child` は内部に組み込むウィジェット。
```dart
TextButton(
  onPressed: 関数,
  child: ウィジェット
)
```

### [ElevatedButton](https://api.flutter.dev/flutter/material/ElevatedButton-class.html)
少し立体的なボタン。他はTextButtonと変わらない。

### [IconButton](https://api.flutter.dev/flutter/material/IconButton-class.html)
アイコンをボタンとして押せる機能を追加したウィジェット。  
`icon` は必須のパラメータで、表示する画像を指定する。
```dart
IconButton(
  icon: const Icon(Icons.insert_emotion),
  iconSize: 100.0,
  color: Colors.red,
  onPressed: buttonPressed,
)
```

### [FloatingActionButton](https://api.flutter.dev/flutter/material/FloatingActionButton-class.html)
右下に常に表示しておくボタンなどに使用されているウィジェット。  
```dart
FloatingActionButton(
  child: Icon(Icons.android),
  onPressed: buttonPressed
),
```

### [RawMaterialButton](https://api.flutter.dev/flutter/material/RawMaterialButton-class.html)
テーマの影響を受けない全て初期状態のボタン。  
色などは自分で設定して利用する。  
```dart
RawMaterialButton(
  fillColor: Colors.white,
  elevation: 10.0,
  padding: EdgeInsets.all(10.0),
  child: Text(
    "Push me!",
    style: TextStyle(frontSize:32.0,
      color: const Color(0xff000000),
      fontWeight: FontWeight.w400,
      fontFamily: "Roboto"
    ),
  ),
),
```

## 入力系ウィジェット
### [TextField](https://api.flutter.dev/flutter/material/TextField-class.html)
テキストを一行入力するためのウィジェット。フォームや検索バーなどで広く利用される。  
`controller` を用いて入力内容を管理し、`decoration` で見た目をカスタマイズすることが可能。  
todo
```dart
const TextField(
  decoration: InputDecoration(
    border: OutlineInputBorder(),
    labelText: '名前を入力',
  ),
)
```

### [Checkbox](https://api.flutter.dev/flutter/material/Checkbox-class.html)
オンとオフ、二つの状態を持つチェックボックス。  
利用規約への同意や、複数の選択肢からいくつかを選ぶ場面で使われる。  
`value` でチェック状態を、`onChanged` で状態の変更を処理する。
```dart
Checkbox(
  value: true,
  onChanged: (bool? newValue) {
    // 状態が変更されたときの処理
  },
)
```

### [Switch](https://api.flutter.dev/flutter/material/Switch-class.html)
オンとオフを切り替えるスイッチ。設定画面などで、特定の機能の有効・無効を切り替えるのに適している。  
基本的な利用方法は `Checkbox` と同様。
```dart
Switch(
  value: false,
  onChanged: (bool newValue) {
    // スイッチが切り替わったときの処理
  },
)
```

### [Radio](https://api.flutter.dev/flutter/material/Radio-class.html)
複数の選択肢から一つだけを選択するためのラジオボタン。  
`groupValue` で選択されている値を管理し、`value` に各ボタン固有の値を設定する。
```dart
Radio(
  value: 1,
  groupValue: 1, // 現在選択されている値
  onChanged: (int? value) {
    // 選択が変更されたときの処理
  },
)
```

### [DropdownButton](https://api.flutter.dev/flutter/material/DropdownButton-class.html)
タップすると選択肢のリストがドロップダウン形式で表示されるボタン。  
項目が多い場合にスペースを節約できるという利点がある。
```dart
DropdownButton<String>(
  value: 'Apple',
  items: <String>['Apple', 'Banana', 'Grape', 'Orange']
      .map<DropdownMenuItem<String>>((String value) {
    return DropdownMenuItem<String>(
      value: value,
      child: Text(value),
    );
  }).toList(),
  onChanged: (String? newValue) {
    // 新しい項目が選択されたときの処理
  },
)
```

### [PopupMenuButton](https://api.flutter.dev/flutter/material/PopupMenuButton-class.html)
ボタン（通常は三点リーダーのアイコン）をタップすると、メニュー項目がポップアップ表示されるウィジェット。  
限られた画面スペースに多くの操作を格納したい場合に有用。
```dart
PopupMenuButton<String>(
  onSelected: (String result) {
    // メニュー項目が選択されたときの処理
  },
  itemBuilder: (BuildContext context) => <PopupMenuEntry<String>>[
    const PopupMenuItem<String>(
      value: 'Profile',
      child: Text('プロフィール'),
    ),
    const PopupMenuItem<String>(
      value: 'Settings',
      child: Text('設定'),
    ),
  ],
)
```

### [Slider](https://api.flutter.dev/flutter/material/Slider-class.html)
スライダーをドラッグすることで、一定範囲の値を連続的に選択できるウィジェット。  
音量や明るさの調整といった、直感的な数値入力に用いられる。
```dart
Slider(
  value: 0.5,
  min: 0.0,
  max: 1.0,
  onChanged: (double newValue) {
    // 値が変更されたときの処理
  },
)
```

## アラートとダイアログ
### [showDialog]()
`showDialog` は、現在の画面コンテンツの上にダイアログを表示するための関数。  
`builder` プロパティに関数（通常は `AlertDialog` や `SimpleDialog` などのダイアログウィジェットを返すもの）を渡して使用する。  

ユーザーの操作を要求するモーダル（画面を覆う）表示に不可欠で、背後のUI操作を一時的に停止させる役割を持つ。
```dart
showDialog(
  context: context,
  builder: (BuildContext context) {
    return AlertDialog(
      title: const Text('アラート'),
      content: const Text('これはAlertDialogのサンプルです。'),
      actions: <Widget>[
        TextButton(
          child: const Text('OK'),
          onPressed: () {
            Navigator.of(context).pop(); // ダイアログを閉じる
          },
        ),
      ],
    );
  },
);
```

### [SimpleDialog]()
ユーザーに複数の単純な選択肢を提示するためのダイアログ。  
通常、タイトルとリスト形式の `SimpleDialogOption` ウィジェットを表示する。  
ユーザーがオプションをタップするとダイアログが閉じられ、選択に応じたアクションが実行される。

複雑な入力が不要で、明確な選択肢をいくつか提示したい場合に最適。
```dart
showDialog(
  context: context,
  builder: (BuildContext context) {
    return SimpleDialog(
      title: const Text('アカウントを選択'),
      children: <Widget>[
        SimpleDialogOption(
          onPressed: () { Navigator.pop(context, 'user1@example.com'); },
          child: const Text('user1@example.com'),
        ),
        SimpleDialogOption(
          onPressed: () { Navigator.pop(context, 'user2@example.com'); },
          child: const Text('user2@example.com'),
        ),
      ],
    );
  },
);
```


## ナビゲーション・ルーティング

## 執筆したFlutterに関する記事
> FlutterでSupabase Authを使ってGoogle認証を実装する  
https://zenn.dev/saisana299/articles/5f9d2426896423

## 参考文献  
> 掌田津耶乃 著. マルチプラットフォーム対応最新フレームワークFlutter 3入門, 秀和システム, 2022.12. 978-4-7980-6852-7. https://ndlsearch.ndl.go.jp/books/R100000002-I032482071  

> Flutter documentation  
https://docs.flutter.dev/
