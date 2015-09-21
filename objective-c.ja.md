# Objective-Cスタイルガイド

改定日：2015/09/21

## はじめに

本ドキュメントは、GyazSquareにおけるObjective-Cスタイルガイドを定めるものである。
ただし、既存のプログラムに関しては、このためだけに修正を強制するものではない。

以下はAppleからのスタイルガイドに関するドキュメントである。
基本はAppleのスタイルを踏襲するため、これらのドキュメンとは必ず一度は目を通すこと。

* [Coding Guidelines for Cocoa](https://developer.apple.com/library/ios/documentation/Cocoa/Conceptual/CodingGuidelines/CodingGuidelines.html)（[Cocoa向け コーディングガイドライン](https://developer.apple.com/jp/devcenter/ios/library/documentation/CodingGuidelines.pdf)）

    Cocoaコーディング規約。コード命名の基本、メソッドの命名、関数の命名、プロパティとデータタイプの命名、許容された略語一覧が記載されている。

* [Objective-C Feature Availability Index](https://developer.apple.com/Library/ios/releasenotes/ObjectiveC/ObjCAvailabilityIndex/index.html)

    Modern Objective-Cの機能の可用性インデックス。iOS 5以降で利用可能。

* [Adopting Modern Objective-C](https://developer.apple.com/library/ios/releasenotes/ObjectiveC/ModernizationObjC/AdoptingModernObjective-C/AdoptingModernObjective-C.html)

    Modern Objective-C (WWDC 2013)の採用についてのドキュメント。

* [Programming with Objective-C](https://developer.apple.com/library/ios/documentation/Cocoa/Conceptual/ProgrammingWithObjectiveC/Introduction/Introduction.html)（[Objective-Cによるプログラミング](https://developer.apple.com/jp/devcenter/ios/library/documentation/ProgrammingWithObjectiveC.pdf)）

    OS X / iOSにおける最新のObjective-Cリファレンス的ドキュメント。

* [64-Bit Transition Guide for Cocoa Touch](https://developer.apple.com/library/ios/documentation/General/Conceptual/CocoaTouch64BitGuide/Introduction/Introduction.html)（[Cocoa Touch 64ビット移行ガイド](https://developer.apple.com/jp/devcenter/ios/library/documentation/CocoaTouch64BitGuide.pdf)）

    Cocoa Touchアプリの64-bit移行ガイド。

* [App Programming Guide for iOS](https://developer.apple.com/library/ios/documentation/iphone/conceptual/iphoneosprogrammingguide/Introduction/Introduction.html)（[iOSアプリケーション プログラミングガイド](https://developer.apple.com/jp/devcenter/ios/library/documentation/iPhoneAppProgrammingGuide.pdf)）

    アプリがiOSで適切に動作するようにするために考慮すべき主な事項の解説。

## スタイル

### インデント

* 半角スペース4文字を使用すること。
* タブは使用しないこと。
* 制御文やブロックのインデントについてはXcodeの仕様に準拠すること。

### 改行

読みやすく、かつ検索や比較がしやすいよう、改行は次のようにおこなう。
改修やレビューにさしつかえるほど長くならないよう工夫すること。

* 次で改行する。
    * `;` の直後
    * `{` の直後
    * `}` の前はインデントのみとなるよう改行
    * 長い条件式
        * 括弧で囲んだ評価優先部分は、途中で改行しない
        * 改行は`&&`、あるいは`||`の前でおこなう
        * 条件の行頭がそろうようインデントする
* 次は途中で改行しない。
    * メソッド
        * メソッドの宣言
        * メソッドの定義の開始部分（開始の`{`までを1行にする）
        * メソッドの呼出（ただしブロック引数は`{}`の改行ルールに従う）
    * リテラル`@""`、`@()`、`@[]`、`@{}`（この`@{}`は`{}`の改行ルールを適用しない）
    * 定数の定義

        ```objc
        NSString * const message = @"Think Different.";
        ```

    * 三項演算子
    * クラス宣言やクラス拡張の開始部分

         ```objective-c
         @interface SubClass : SuperClass <FirstProtocol, SecondProtocol, ThirdProtocol>
         ```

例）長い条件式の改行

```objective-c
if ((condition1 && condition2)
    || (condition3 && condition4 && condition5)
    || !(condition6 && condition7 && condition8 && condition9)) {
    // do something
}
```

例）メソッド宣言

```objective-c
- (NSRange)rangeOfString:(NSString *)aString options:(NSStringCompareOptions)mask range:(NSRange)searchRange locale:(NSLocale *)locale;
```

例）メソッド定義の開始部分

```objective-c
- (NSRange)rangeOfString:(NSString *)aString options:(NSStringCompareOptions)mask range:(NSRange)searchRange locale:(NSLocale *)locale {
    // do something
}
```

例）メソッド呼出（ブロック引数なし）

```objective-c
NSRange range2 = [string1 rangeOfString:string2 options:NSCaseInsensitiveSearch range:range1 locale:nil];
```

例）メソッド呼出（ブロック引数あり）

```objective-c
[aSet enumerateObjectsUsingBlock:^(id obj, BOOL *stop) {
    if ([obj localizedCaseInsensitiveCompare:string] == NSOrderedSame) {
        found = YES;
        *stop = YES;
    }
}];
```

例）定数の定義

```objective-c
NSString * const ICSErrorDomain = @"Generic Error Domain";
```

例）三項演算子

```objective-c
return (string && string.length > 0) ? @"Succeeded." : @"Failed.";
```

例）クラス宣言の開始部分

```objective-c
@interface ICSViewController : UIViewController <UITableViewDataSource, UITableViewDelegate>
```

例）クラス拡張の開始部分

```objective-c
@interface ICSViewController () <UITableViewDataSource, UITableViewDelegate>
```

### フォーマット

* メソッド／関数の中括弧`{}`や条件文の中括弧（`if`/`else`/`switch`/`while`など）は常に文（statement）と同じ行から開き、新しい行で閉じること。
* `if`や`while`の条件や`switch`の式の前後に不要なスペースを挿入しないこと。
* `if`や`while`の文が1行であっても`{}`は省略しないこと。
* `if`や`while`の文が0行であっても`{}`は省略せず、`// do nothing`コメントを記載すること。

    → 書き忘れではないことを明示するため。

* 不要なスペースや空行の繰り返しは挿入しないこと。

    ただし、可読性をあげるための最低限のスペース挿入は許容する。

**Right:**

```objective-c
if (condition) {
    // do something
}
```

```objective-c
if (condition) {
    // do something
} else if (condition) {
    // do something
} else {
    // do nothing
}
```

```objective-c
typedef NS_OPTIONS(NSUInteger, NSEnumerationOptions) {
    NSEnumerationConcurrent = (1UL << 0),
    NSEnumerationReverse = (1UL << 1),
};
```

```objective-c
typedef NS_ENUM(NSInteger, UIInterfaceOrientation) {
    UIInterfaceOrientationUnknown            = UIDeviceOrientationUnknown,
    UIInterfaceOrientationPortrait           = UIDeviceOrientationPortrait,
    UIInterfaceOrientationPortraitUpsideDown = UIDeviceOrientationPortraitUpsideDown,
    UIInterfaceOrientationLandscapeLeft      = UIDeviceOrientationLandscapeRight,
    UIInterfaceOrientationLandscapeRight     = UIDeviceOrientationLandscapeLeft
};
```

**Wrong:**

```objective-c
if (condition)
{
    // do something
}
```

```
#!objective-c
if ( condition )
{
    // do something
}
```

```objective-c
if (condition)
    // do something
else if (condition)
    // do something else if
else
    // do something else
```

```objective-c
if ( condition ) {

    // do something

}
```

* 演算子の前後やカンマ、セミコロンの後にはスペースを1文字挿入すること。
* 前置単項演算子の後ろ、後置単項演算子の前にはスペースは挿入しないこと。
* 括弧`()` / `{}` / `[]`の両端にはスペースを挿入しない。

**Right:**

```objective-c
y = m * x + b;
f(a, b);
c = a | b;
return (condition ? 1 : 0);
extern NSString * const WKErrorDomain;
i++;
++i;
@1
@{@"key1": @"value1", @"key2": @"value2"}
@[@"value1", @"value2"]
```

**Wrong:**

```objective-c
y=m*x+b;
f(a,b);
c = a|b;
return (condition ? 1:0);
extern NSString *const WKErrorDomain;
i ++;
++ i;
@( 1 )
@{ @"key1" : @"value1", @"key2" : @"value2" }
@[ @"value1", @"value2" ]
```

* 優先順位の異なる演算子が混在する式では括弧`()`を利用して演算子の優先順位を明確にすること。

**Right:**

```objective-c
if ((a == b) && (c == d)) {
```

**Wrong:**

```objective-c
if (a == b && c == d) {
```

### if / while / for文

### case文

### ポインタ宣言

## 命名規則

### プレフィックス

### API要素の命名規則

### ハンガリアン記法

## コメント

## 変数

### 変数の初期化

## Modern Objective-C

## ARC

### 変数修飾子

### __block変数

### __weak変数

## プロパティ

### プロパティの属性

### プロパティのアクセス

## カテゴリー

## エラー処理

### 通常のエラー処理

### アサーション

## 警告

## サブクラス

### ヘッダファイル

### ソースファイル

## Method Swizzling

## Objective-C++

## Swift対応

### Null許容性

### 軽量ジェネリクス

## コード構成

### インポートファイル例

### ヘッダファイル

### ソースファイル

## その他

* * *

## 改定履歴
