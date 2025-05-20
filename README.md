## Flutterとは
Flutterは、Googleによって開発されたオープンソースのUIツールキット。  
単一のコードベースから、モバイル、ウェブ、デスクトップ向けのネイティブ同様のアプリケーションを構築できる。  
[Flutter - Build apps for any screen](https://flutter.dev/)

## Dart文法
### 1. 基本構造と実行  
Dartプログラムは `main()` 関数から実行が開始される。
```dart
void main() {
  print("Hello, Dart!");
}
```
コメントは `//`  複数行コメントは `/* ... */` で記述する。  
Pythonとは違い、基本的には文の終わりに `;` を付ける。  
Dartでは、全ての値がオブジェクトとして扱われる。（`Object` クラスを継承）  
<br>

### 2. 変数
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
変数が有効な範囲は、基本的に波括弧 `{}` で囲まれたブロック内。
```dart
int count; // null

{
  int n = 21;
}
print(n); // エラー
```
<br>

### 3. 基本型
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
<br>

### 4. 定数
`final` は実行時に一度だけ値を代入でき、その後は変更できない。
```dart
final String appName = "My Awesome App";
appName = "Another App"; // エラー
```
`const` はコンパイル時に値が確定するため、ビルド時点で値が決まっている必要がある。
```dart
const double pi = 3.14159;
```
<br>

## Flutter環境構築

## 執筆したFlutterに関する記事
FlutterでSupabase Authを使ってGoogle認証を実装する  
https://zenn.dev/saisana299/articles/5f9d2426896423

## 参考文献  
掌田津耶乃 著. マルチプラットフォーム対応最新フレームワークFlutter 3入門, 秀和システム, 2022.12. 978-4-7980-6852-7. https://ndlsearch.ndl.go.jp/books/R100000002-I032482071
