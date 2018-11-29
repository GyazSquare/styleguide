# Objective-Cスタイルガイド

改定日：2018/11/29

## はじめに

本ドキュメントは、GyazSquareにおけるObjective-Cスタイルガイドを定めるものである。
ただし、既存のプログラムに関しては、このためだけに修正を強制するものではない。

以下はAppleからのスタイルガイドに関するドキュメントである。
基本はAppleのスタイルを踏襲するため、これらのドキュメンとは必ず一度は目を通すこと。

* [Coding Guidelines for Cocoa](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/CodingGuidelines/CodingGuidelines.html)（[Cocoa向け コーディングガイドライン](https://developer.apple.com/jp/devcenter/ios/library/documentation/CodingGuidelines.pdf)）

    Cocoaコーディング規約。コード命名の基本、メソッドの命名、関数の命名、プロパティとデータタイプの命名、許容された略語一覧が記載されている。

* [Objective-C Feature Availability Index](https://developer.apple.com/Library/ios/releasenotes/ObjectiveC/ObjCAvailabilityIndex/index.html)

    Modern Objective-Cの機能の可用性インデックス。iOS 5以降で利用可能。

* [Adopting Modern Objective-C](https://developer.apple.com/library/ios/releasenotes/ObjectiveC/ModernizationObjC/AdoptingModernObjective-C/AdoptingModernObjective-C.html)

    Modern Objective-C (WWDC 2013)の採用についてのドキュメント。

* [Programming with Objective-C](https://developer.apple.com/library/ios/documentation/Cocoa/Conceptual/ProgrammingWithObjectiveC/Introduction/Introduction.html)（[Objective-Cによるプログラミング](https://developer.apple.com/jp/devcenter/ios/library/documentation/ProgrammingWithObjectiveC.pdf)）

    OS X / iOSにおける最新のObjective-Cリファレンス的ドキュメント。

* [64-Bit Transition Guide for Cocoa Touch](https://developer.apple.com/library/ios/documentation/General/Conceptual/CocoaTouch64BitGuide/Introduction/Introduction.html)（[Cocoa Touch 64ビット移行ガイド](https://developer.apple.com/jp/devcenter/ios/library/documentation/CocoaTouch64BitGuide.pdf)）

    Cocoa Touchアプリの64-bit移行ガイド。

* [App Programming Guide for iOS](https://developer.apple.com/library/ios/documentation/iphone/conceptual/iphoneosprogrammingguide/Introduction/Introduction.html)（[iOSアプリケーション プログラミングガイド](https://developer.apple.com/jp/documentation/iPhone/Conceptual/iPhoneOSProgrammingGuide/Introduction/Introduction.html)）

    アプリがiOSで適切に動作するようにするために考慮すべき主な事項の解説。

## スタイル

### インデント

* 半角スペース4文字を使用すること。
* タブは使用しないこと。
* 制御文やブロックのインデントについてはXcodeの仕様に準拠すること。

### 改行

読みやすく、かつ検索や比較がしやすいよう、改行は次のようにおこなう。
なお、改修やレビューにさしつかえるほど長くならないよう[1行80文字](https://en.wikipedia.org/wiki/Characters_per_line#In_programming)を目処に工夫すること。

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
    * 三項演算子
    * クラス宣言やクラス拡張の開始部分

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
    ...
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
NSString * const GSLErrorDomain = @"Generic Error Domain";
```

例）三項演算子

```objective-c
return (string && string.length > 0) ? @"Succeeded." : @"Failed.";
```

例）クラス宣言の開始部分

```objective-c
@interface GSLViewController : UIViewController <UITableViewDataSource, UITableViewDelegate>
```

例）クラス拡張の開始部分

```objective-c
@interface GSLViewController () <UITableViewDataSource, UITableViewDelegate>
```

### フォーマット

* メソッド／関数の中括弧`{}`や条件文の中括弧（`if`/`else`/`switch`/`while`など）は常に文（statement）と同じ行から開き、新しい行で閉じること。
* `if`や`while`の条件や`switch`の式の前後に不要なスペースを挿入しないこと。
* `if`や`while`の文が1行であっても`{}`は省略しないこと。
* `if`や`while`の文が0行であっても`{}`は省略せず、`// do nothing`コメントを記載すること。
    * 書き忘れではないことを明示するため。
* 不要なスペースや空行の繰り返しは挿入しないこと。
    * ただし、可読性をあげるための最低限のスペース挿入は許容する。

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

```objective-c
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

* 式は第三者に説明できる論理式であること。
* 式にはシンプルな処理（状態不変・低コスト）を記述すること。

### case文

* `case`文で複数行の処理をブロックスコープで囲う必要がある場合は`case`文と同じ行から開き、`break`の次の行で閉じること。
* フォールスルー（次の`case`と同じ処理を実行する）の場合はコメントで`fall-through`と記述すること。

```objective-c
switch (expression) {
    case 1:
        // ...
        break;
    case 2: {
        // ...
        // Multi-line example using braces
        break;
    }
    case 3:
        // fall-through
    case 4:
        // code executed for 3 and 4
        break;
    default:
        // ...
        break;
}
```

* `switch`で`enum`を取り扱う場合、取りうる値をすべて記述していないときは必ず`default`節を記述すること。
* `switch`で`enum`以外（`int`など）を取り扱う場合、必ず`default`節を記述すること。

### ポインタ宣言

* ポインタ宣言時に付けるアスタリスク`*`は変数側に付けること。
* 型指定やキャスト時は変数型からスペース一つ空けること。
* `const`が存在する場合には型と`const`の中間に付けること。

**Right:**

```objective-c
static NSMutableArray *sKnownRegions = nil;
+ (instancetype)bundleWithPath:(NSString *)path;
extern NSString * const GSLIMAPProtocolErrorDomain;
```

**Wrong:**

```objective-c
static NSMutableArray* sKnownRegions = nil;
+ (instancetype)bundleWithPath:(NSString*)path;
extern NSString *const GSLIMAPProtocolErrorDomain;
```

## 命名規則

### プレフィックス

* アプリやコンポーネントのパッケージでは、Appleの命名規則に従ったプレフィックスを使用すること。
* プレフィックスは2〜3文字の英大文字を必ず利用すること。
    * 3文字が不自然な場合は4文字以上でも問題ない。
* Appleの定義しているプレフィックスは決して利用しないこと。
    * 実行時に予期せぬ挙動を引き起こすため。
    * Appleで予約されているNS、UI、CFなどは利用せず、できれば3文字の英大文字を利用する。
* プレフィックスはあくまでシンボルの衝突を防ぐためのものであり（名前空間の代用）、アプリケーション名やコンポーネント名、会社名、開発者名などから適切なものを選択すること。

### API要素の命名規則

* キャメルケース以外は原則利用しないこと。
* メソッド名、プロパティ名はローワーキャメルケース
* クラス名や関数名や定数はプレフィックス＋アッパーキャメルケース
* キャメルケースとスネークケースの混在は避けること。
    * ただし、バージョン番号など数字の区切りでの下線の利用は可とする。

```objective-c
typedef NS_ENUM(NSUInteger, NSDateFormatterBehavior) {
    NSDateFormatterBehaviorDefault = 0,
#if (TARGET_OS_MAC && !(TARGET_OS_EMBEDDED || TARGET_OS_IPHONE))
    NSDateFormatterBehavior10_0 = 1000,
#endif
    NSDateFormatterBehavior10_4 = 1040,
};
```

```objective-c
enum {
    NSBundleExecutableArchitectureI386      = 0x00000007,
    NSBundleExecutableArchitecturePPC       = 0x00000012,
    NSBundleExecutableArchitectureX86_64    = 0x01000007, // "_"がないと見づらい
    NSBundleExecutableArchitecturePPC64     = 0x01000012
};
```

* Cocoa Touchのインターフェイス全体で一貫した名前を利用すること。
    * Cocoa Touchのヘッダやドキュメントを参考にすること。
    * 名前の対称性を正しく使用すること（add/removeなど）。
* 許可されている略語以外は使用しないこと。
    * destinationSelection: OK
    * destSel: NG
* プロパティ名やメソッド引数、変数がコレクションクラス、またはそれに相当するクラスの場合は複数形を用いること。
    * ただし、不可算名詞などの場合は接尾語にコレクションクラス名を付加すること。

```objective-c
// 引数が単数
+ (instancetype)arrayWithObject:(id)object;

// 引数がコレクション
+ (NSString *)pathWithComponents:(NSArray *)components;

// 引数がコレクションに相当
- (NSArray *)objectsAtIndexes:(NSIndexSet *)indexes;

// NSDataの配列
@property (copy) NSArray *dataArray;
```

### ハンガリアン記法

* プロパティ名にハンガリアン記法は利用しない（`m_`など）。
    * Objective-Cでプロパティはメソッドと同義であり、インスタンス変数ではない。
    * Modern Objective-Cではインスタンス変数の定義・アクセスは基本不要。
* ハンガリアン記法は以下の2つの場合にのみ用いること。
    * `k`：内部でのみ使用する定数（`#define` / `static const`）
    * `s`：static変数

**Right:**

```objective-c
// プロパティの例
@property (readonly) NSUInteger length;
```

```objective-c
// 定数の例
static const long kMaxBufferBytesPerFrame = 1024 * 1024;
```

```objective-c
// static変数の例
static NSMutableArray *sKnownRegions = nil;
```

**Wrong:**

```objective-c
// プロパティ名（メソッド名）にmは付けない
@property (readonly) NSUInteger m_length;
```

```objective-c
// キャメルケースではない
static NSMutableArray *s_knownRegions = nil;
```

## コメント

* コメントは英語で記述すること。
* 冗長／不要なコメントは避けること。
* コードからすぐに理解できるコメントは避けること。
* 曖昧な言い回しを避けること。
* ですます調は避けること。
* デバッグコードをコメントアウトして残さないこと。
* SDKのバグなどによる一時的な処理には必ずコメントを残すこと。
* 以下のケースに該当する場合はコメントにプレフィックスを付けること。
    * `TODO`：やらなければならないこと。
    * `FIXME`：バグがあり、正しく動作しない。
    * `XXX`：バグがあるが、大抵の場合は動作する。
    * `HACK`：ライブラリバグ回避コードなど、改善が必要。

```objective-c
// TODO: Master information advance registration
```

```objective-c
// FIXME: It crashes due to the option to animate the deletion when the bottom line is visible.
```

```objective-c
// XXX: GSLCalendarManager doesn't have the function to reset the access token.
```

```objective-c
// HACK: Not very well written or malformed code to circumvent a problem/bug.
```

## 変数

### 変数の初期化

* 自動変数だけでなく、外部／静的変数も明示的に初期化すること。
* 構造体の初期化には専用関数、または定数を使用すること。
    * ただし、静的変数の初期化の場合は関数は利用できないため、C99の指定イニシャライザを利用すること。

**Right:**

```objective-c
- (void)method {
    NSRange range = NSMakeRange(0, 0);
    // do something
}
```

```objective-c
static NSRange sRange = {.location = 0, .length = 0};
static UIOffset sOffset = UIOffsetZero; 
```

**Wrong:**

```objective-c
- (void)method {
    NSRange range = {0, 0};
    // do something
}
```

```objective-c
static NSRange sRange = {0, 0};
static UIOffset sOffset;
```

## Modern Objective-C

特別な理由がない限り、Modern Objective-Cの利用・記法を利用すること。

* ARC
* `@autoreleasepool`ブロック
* プロパティのインスタンス変数／アクセサメソッドのデフォルト生成
* クラス拡張中のプライベートメソッド定義の省略
* `NSNumber` / `NSDictionary` / `NSArray`リテラル
* `@YES` / `@NO`リテラル
* `NSDictionary` / `NSArray`サブスクリプティング
* `instancetype`
* `NS_ENUM ` / `NS_OPTIONS`マクロ
* `NS_DESIGNATED_INITIALIZER`マクロ
* `NS_PROTOCOL_REQUIRES_EXPLICIT_IMPLEMENTATION `マクロ
* `NS_ASSUME_NONNULL_BEGIN` / `NS_ASSUME_NONNULL_END` マクロ

## ARC

### 変数修飾子

* 変数修飾子は正しい形式で利用すること。
    * `__strong ` / `__weak` / `__unsafe_unretained ` / `__autoreleasing `

```objective-c
ClassName * qualifier variableName;
```

```objective-c
GSLClass * __weak weakReference;
GSLClass * __unsafe_unretained unsafeReference;
```

### __block変数

* `__block`ストレージクラス修飾子が指定された変数はARC環境下では参照が保持されることに注意すること。
    * 使用後は`nil`を必ず代入すること。

```objective-c
__block GSLViewController *viewController = [GSLViewController alloc] init…];
// ...
viewController.completionHandler = ^(NSInteger result) {
    [viewController dismissViewControllerAnimated:YES completion:nil];
    viewController = nil;
};
```

### __weak変数

* `__weak`変数で参照されているオブジェクトはいつ開放されるかわからないため、利用前には`__strong`ローカル変数に値を保持して利用すること。

**Right:**

```objective-c
GSLViewController *viewController = [[GSLViewController alloc] init…];
// ...
GSLViewController * __weak weakViewController = viewController;
viewController.completionHandler = ^(NSInteger result) {
    GSLViewController *strongViewController = weakViewController; // スコープ終了までは保持されることが保証される。
    if (strongViewController) {
        // ...
        [strongViewController dismissViewControllerAnimated:YES completion:nil];
    } else {
        // エラー処理
    }
};
```

**Wrong:**

```objective-c
GSLViewController *viewController = [[GSLViewController alloc] init…];
// ...
GSLViewController * __weak weakViewController = viewController;
viewController.completionHandler =  ^(NSInteger result) {
    // この時点で存在するが、
    if (weakViewController) {
        // この時点で削除されている可能性がある（エラー処理に入らない）。
        // ...
        [weakViewController dismissViewControllerAnimated:YES completion:nil];
    } else {
        // エラー処理
    }
};
```

## ブロック

### ブロックの宣言

```objective-c
int multiplier = 7;
int (^myBlock)(int) = ^(int num) {
    return num * multiplier;
};
```

![blocks.jpg](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/Blocks/Art/blocks.jpg)

* 引数リストには必ず指定すること。引数を取らない場合は `void` を指定すること。
* 特定のシグネチャを持つブロックを複数の場所で使用する場合は、その型を作成すること。

```objective-c
void (^blockReturningVoidWithVoidArgument)(void);
int (^blockReturningIntWithIntAndCharArguments)(int, char);
void (^arrayOfTenBlocksReturningVoidWithIntArgument[10])(int);
```

```objective-c
typedef NSComparisonResult (^NSComparator)(id obj1, id obj2);
```

### ブロックの実装

* 戻り値は自動的に推論されるため、明示的に記述せず省略すること。
* 戻り値が推論され、引数を取らない場合は `(void)` パラメータリストを省略すること。

```objective-c
// 引数を取る場合
int (^oneFrom)(int);

oneFrom = ^(int anInt) {
    return anInt - 1;
};

// 引数を取らない場合
int (^getInt)(void);

getInt = ^{
    int i = 1;
    return i;
};
```

## プロパティ

### プロパティの属性

* プロパティの属性は以下の順番で定義すること（推奨）。デフォルトの属性は記述不要（推奨）。
    * `class`
    * `atomic` (default) / `nonatomic`
    * `readwrite` (default) / `readonly`
    * `strong` (default for object) / `copy` / `assign` (default for non-object) / `weak`
    * `nullable` / `nonnull`
    * `getter`
    * `setter`

```objective-c
// NSStringのプロパティ (atomic, assign)
@property (readonly) NSUInteger length;
```

```objective-c
// NSURLSessionTaskのプロパティ (atomic, readwrite)
@property (copy) NSString *taskDescription
```

```objective-c
// SKUniformのプロパティ (atomic, readwrite, strong)
@property SKTexture *textureValue;
```

* スレッドセーフなプロパティの属性は`atomic`を指定すること。
* 読み取り専用のプロパティの属性には`readonly`を指定すること。
    * 内部のみで読み書きする場合はクラス拡張で`readwrite`属性を指定すること。

```objective-c
// GSLObject.h
@interface GSLObject

// 外部からは読み取り専用
@property (readonly, copy) NSString *name;

@end
```

```objective-c
// GSLObject.m
@interface GSLObject ()

// 内部からは読み書き可能
@property (readwrite, copy) NSString *name;

@end
```

* 可変サブクラス（NSMutableXXX）が存在し、NSCopyingプロトコルに準拠するオブジェクトのプロパティの属性は`copy`を指定すること。

**Right:**

```objective-c
@interface GSLObject

// NSMutableStringをセットするとコピーされた別インスタンスを保持。
@property (copy) NSString *name;

@end
```

**Wrong:**

```objective-c
@interface GSLObject

// NSMutableStringをセットすると外部より変更される可能性あり。
@property NSString *name;

@end
```

* Core Dataの`NSManagedObject`の動的プロパティは必ず`nonatomic`を指定すること。
* 動的プロパティには`weak`を指定せず、またオーバーヘッド削減のため、極力`copy`は避けること。

```objective-c
@interface Employee : NSManagedObject

@property (nonatomic) NSString *firstName, *lastName;

@end
```

```objective-c
@implementation Employee

@dynamic firstName, lastName;

@end
```

### プロパティのアクセス

* プロパティが提供されているものはドット記法を徹底すること。
    * 逆にメソッドは従来の`[]`記法を徹底すること。
* iOS 8 SDKにより、プロパティへ変更されたメソッドが多数存在するため、リファレンスで確認すること。

**Right:**

```objective-c
if (string.length > 0) {
```

```objective-c
NSEnumerator *objectEnumerator = [array objectEnumerator];
```

**Wrong:**

```objective-c
if ([string length] > 0) {
```

```objective-c
NSEnumerator *objectEnumerator = array.objectEnumerator;
```

* インスタンス変数への直接アクセスは極力避け、プロパティアクセスを心がけること。（後述する例外あり。）
* 同一スコープ内で2回以上同一プロパティにアクセスする場合はローカル変数に値を保持して利用すること。
    * スレッドセーフプロパティ（`atomic`）や弱参照プロパティ（`weak`）の場合は複数回のアクセスがすべて同じ値である保証がないため。
* 公開不要なプロパティはクラス拡張に隠蔽すること。
* デフォルトの`getter` / `setter`はオーバーライドせず、プロパティの監視はキー値監視（KVO）を利用すること。
    * `getter` / `setter`をオーバーラードしても、プロパティ取得／設定以上の処理は決して行わないこと。

ただし イニシャライザについてはプロパティの読み書きを次のように避ける。

* スーパークラスのプロパティはドット記法を使用する。
* 自分のクラスのプロパティには、基本的にプロパティのインスタンス変数に直接アクセスする。（または`self`ポインタ。）
    * ただし、自分のクラスで外部公開していない`setter`利用の場合はドット記法で可。
    * この場合`copy`プロパティは`copy`メソッドを利用することを忘れないこと。

## カテゴリー

* カテゴリーでメソッドのオーバーライドは行わないこと。
* カテゴリーでCocoa Touchクラスの拡張は極力行わないこと。
    * 同名のメソッドが定義されている場合、どちらの実装が使われるか実行時にしかわからない。
    * Cocoa Touchではプライベートメソッドが多く利用されており、その場合はコンパイル時に警告すら表示されない。
    * そもそもCocoa Touchに存在しないメソッド＝一般的に不要ということを認識すべきである。
* 下記は`MapKit`で定義されている`NSNumber`非公開カテゴリである。
    * もし同名のメソッドを定義した場合、挙動が異なれば実行時に予期せぬエラーを引き起こす。

```objective-c
@interface NSNumber (CGFloat)
+ (id)numberWithCGFloat:(double)value;
- (double)cgFloatValue;
- (id)initWithCGFloat:(double)value;
@end
```

* カテゴリでCocoa Touchクラスの拡張を行う場合は以下のルールに従うこと。
    * 命名規則で述べたプレフィックス＋アンダーライン（`(プレフィックス)_`）を、メソッド名の先頭に小文字で付与すること。
    * Appleの非公開メソッドやオープンソースで最も利用されている形式のため。

**Right:**

```objective-c
@interface NSNumber (GSLAdditions)
+ (instancetype)gsl_numberWithCGFloat:(double)value;
```

**Wrong:**

```objective-c
@interface NSNumber (GSLAdditions)
+ (instancetype)numberWithCGFloat:(double)value;
```

## エラー処理

### 通常のエラー処理

* エラー処理は必ず実施すること。
* 通常のエラー処理では例外処理を利用しないこと。
* 空文字やエラーコードをメソッドや関数のエラー時の戻り値として使用しないこと。
    * エラー時の戻り値は`nil`、またはBOOL値の`NO`とする。
    * 具体的なエラーの理由が必要な場合は参照渡しの出力引数で`NSError`オブジェクトを返すこと。
* 独自定義のエラーを返す場合は適切なエラードメインとエラーコードを設定すること。

```objective-c
extern NSString * const GSLIMAPProtocolErrorDomain;

typedef NS_ENUM(NSInteger, GSLIMAPProtocolError) {
    GSLIMAPProtocolErrorParseFailure    = -1,
    GSLIMAPProtocolErrorCommandSuccess  =  0,
    GSLIMAPProtocolErrorCommandFailure  =  1,
    GSLIMAPProtocolErrorProtocolError   =  2,
    GSLIMAPProtocolErrorConnectionClose =  3
};
```

* インターフェイスは引数で返す場合とオブジェクトのプロパティとして返す場合が存在する。
    * 一般的にオブジェクトに状態が存在する場合はプロパティとして保存する。
    * そうでない場合は引数で返しプロパティとしては保持しない。
    * 例）`NSStream`はエラー状態が存在するため、`streamError`プロパティを保持する。`NSFileManager`には状態が存在しないため、エラーは引数で返し、プロパティとして保持しない。
* エラー受け取り後に別のエラーを返す場合は`userInfo`プロパティの`NSUnderlyingErrorKey`に元のエラーを設定して返すこと。

### アサーション

* プログラミングバグや論理バグの場合はアサーションを利用すること。
* 配列の範囲外アクセス、無効な引数、内部状態の矛盾、等々
* アサーションには`NSAssert` / `NSParameterAssert`を利用する場合と`NSException`を利用する場合がある。
* `NSAssert` / `NSParameterAssert`はファイルパスがバイナリに含まれるようになるため、リリースビルドでは必ず無効にすること（Enable Foundation Assertions = NO for Release）。
* リリースビルドでも有効にしたいアサーションには`NSException`を利用すること。
    * 一般的にライブラリ系では`NSException`を利用する。一方、アプリでは`NSAssert`を利用する。
* `NSException`の名前はGeneric Exception namesから適切なものを選択すること。
    * Generic Exception namesはFoundation.framework/Headers/NSException.hを参照のこと。
    * 例）不正パラメータ：`NSInvalidArgumentException`<br>
      　　内部状態の矛盾：`NSInternalInconsistencyException`
* アサーションを利用する場合はコメントに事前／事後条件を記載すること。
* 例外は補足しないこと（プログラム中で`@try` ~ `@catch`構文を使用しない）。
    * 例外が発生するのは通常の処理ではあり得ないため。

```objective-c
- (instancetype)initWithData:(NSData *)data {
    NSParameterAssert(data); // リリースビルドでは無効
    self = [super init];
    if (self) {
        // do something
    }
    return self;
}
```

```objective-c
- (instancetype)initWithData:(NSData *)data {
    if (!data) { // リリースビルドでも有効
        [NSException raise:NSInvalidArgumentException format:@"*** %s: nil data parameter", __PRETTY_FUNCTION__];
    }
    self = [super init];
    if (self) {
        // do something
    }
    return self;
}
```

## 警告

* 最新のXcodeのデフォルト警告設定でDebug/Releaseコンフィグレーションともにビルドの警告・エラーは必ず排除すること。
    * ただし、`#warning` / `#error`プリプロセッサ・ディレクティブによる意図的な警告／エラーは除くこととする。
* 警告を隠すためにプロジェクトの設定を変更したり、プラグマをコードに埋め込むことは決して行わないこと。

**Wrong:**

```objective-c
#pragma clang diagnostic push
#pragma clang diagnostic ignored "-Wobjc-protocol-method-implementation"
- (void)addRoundedRectToPath:(CGRect)rect context:(CGContextRef)context ovalWidth:(CGFloat)ovalWidth ovalHeight:(CGFloat)ovalHeight {
    // do something
}
#pragma clang diagnostic pop
```

* Xcodeの静的解析ツールの指摘項目は必ず確認し、必要があれば修正を行うこと。
    * デッドストア（変更後に利用されない変数）、メモリリーク、GCDの不正利用、Objective-C言語仕様不正、セキュリティ危険項目など、指摘項目は多岐に渡り、バグ／潜在的な問題になる可能性が非常に高い。

## サブクラス

* Cocoa Touchクラスで継承禁止のクラスやシングルトンクラス、クラスクラスタは原則継承しないこと。
    * `NSManagedObjectContext`、`NSString`、`NSNotificationCenter`、`NSUserDefaults`等々
    * 継承が必要な場合は必ず公式ドキュメントでオーバーライドが必要なメソッドを必ず確認すること。
* Cocoa Touchクラスを継承し、メソッド／プロパティをオーバーライドする場合は元のメソッドと事前／事後条件が一致するように実装を行うこと。
* 継承クラスにて指定イニシャライザを定義する場合は以下のルールに従うものとする。
    * ヘッダにて指定イニシャライザに`NS_DESIGNATED_INITIALIZER`を付加する。
    * スーパークラスの指定イニシャライザをもれなくオーバライドする。

### ヘッダファイル

```objective-c
@interface GSLSASL : NSObject

- (instancetype)initClientWithService:(NSString *)service serverFQDN:(NSString *)serverFQDN IPLocalPort:(NSString *)IPLocalPort IPRemotePort:(NSString *)IPRemotePort error:(NSError **)error NS_DESIGNATED_INITIALIZER;

```

### ソースファイル

```objective-c
@implementation GSLSASL

// NSObjectの指定イニシャライザをオーバーライドする
- (instancetype)init {
    return [self initClientWithService:nil serverFQDN:nil IPLocalPort:nil IPRemotePort:nil error:NULL];
}

// サブクラスの指定イニシャライザにてスーパークラスの指定イニシャライザを呼び出す
- (instancetype)initClientWithService:(NSString *)service serverFQDN:(NSString *)serverFQDN IPLocalPort:(NSString *)IPLocalPort IPRemotePort:(NSString *)IPRemotePort error:(NSError **)error {
    self = [super init];
    if (self) {
        // ...

```

## Method Swizzling

* 動的にメソッドを入れ替えるMethod Swizzling（メソッド・スウィズリング）はSDKの致命的なバグを回避する目的以外では原則利用しないこと。
* Method Swizzlingの問題点は以下の通り：
    * SDK全体の挙動変更
    * SDKに依存する挙動
    * 非スレッドセーフ
    * 名前衝突の可能性
    * メソッドシグネチャー（引数の型、個数など）変更の可能性
    * 実行順による挙動の変更
    * デバッグが非常に困難

## Objective-C++

* Objective-C++特有の機能が本当に必要な場合以外は決してObjective-C++を利用しないこと。
    * `.mm`ファイルが存在するプロジェクトではリファクタリングなどXcodeの機能が一部利用できなくなるため。
* Objective-CのクラスをObjective-C++`.mm`ファイルに記述し、Cコンパイラでビルドしないこと。
    * リンクできるが、実行時に予期せぬエラーを引き起こすため。

## API可用性

* Base SDKがmacOS 10.13、iOS 11、tvOS 11、watchOS 4 (Xcode 9) 以降の場合は `@available` を利用して実行時システムバージョンをチェックすること。

```objective-c
if (@available(macOS 10.13, iOS 11, *)) {
    // Use macOS 10.13 APIs on macOS, and use iOS 11 APIs on iOS
} else {
    // Fall back to earlier macOS and iOS APIs
}
```

* Base SDKが上記より前の場合は、下記ドキュメントに従い、ウィークリンクを用いるか、システムバージョンに依存する処理の場合は `NSProcessInfo` (`Foundation`)、`UIDevice` (`UIKit`) を用いること。

    * [SDK Compatibility Guide](https://developer.apple.com/library/content/documentation/DeveloperTools/Conceptual/cross_development/Introduction/Introduction.html#//apple_ref/doc/uid/10000163-BCICHGIE)（[SDK互換性ガイド](https://developer.apple.com/jp/documentation/cross_development.pdf)）


```objective-c
NSString *requiredSystemVersion = @"10";
if ([UIDevice.currentDevice.systemVersion compare:requiredSystemVersion options:NSNumericSearch] != NSOrderedDescending) {
    // Use iOS 10 APIs on iOS, and use tvOS 10 APIs on tvOS
} else {
    // Fall back to earlier iOS and tvOS APIs
}
```

```objective-c
NSOperatingSystemVersion requiredSystemVersion = (NSOperatingSystemVersion){10, 0, 0};
if ([NSProcessInfo.processInfo isOperatingSystemAtLeastVersion:requiredSystemVersion]) {
    // Use macOS 10 APIs, iOS 10 APIs, watchOS 10 APIs, and tvOS 10 APIs
} else {
    // Fall back to earlier macOS, iOS, watchOS, and tvOS APIs
}
```

## Swift言語との相互運用性

Swift言語との相互運用性を考慮した実装を心掛けること。

* [Designating Nullability in Objective-C APIs](https://developer.apple.com/documentation/swift/objective-c_and_c_code_customization/designating_nullability_in_objective-c_apis)
* [Renaming Objective-C APIs for Swift](https://developer.apple.com/documentation/swift/objective-c_and_c_code_customization/renaming_objective-c_apis_for_swift)
* [Improving Objective-C API Declarations for Swift](https://developer.apple.com/documentation/swift/objective-c_and_c_code_customization/improving_objective-c_api_declarations_for_swift)
* [Grouping Related Objective-C Constants](https://developer.apple.com/documentation/swift/objective-c_and_c_code_customization/grouping_related_objective-c_constants)
* [Marking API Availability in Objective-C](https://developer.apple.com/documentation/swift/objective-c_and_c_code_customization/marking_api_availability_in_objective-c)
* [Making Objective-C APIs Unavailable in Swift](https://developer.apple.com/documentation/swift/objective-c_and_c_code_customization/making_objective-c_apis_unavailable_in_swift)
* Lightweight Generics, Kindof types
    * [WWDC 2015 Swift and Objective-C Interoperability](https://developer.apple.com/videos/play/wwdc2015/401/)

## コード構成

### インポートファイル例

Cocoa Touchフレームワーク（modules）／C標準ヘッダ／フレームワーク（Cocoa Touch）／フレームワーク（Cocoa Touch以外）／アプリヘッダをアルファベット順で記述すること

```objective-c
// 1. modules対応フレームワーク
@import Foundation;

// 2. C標準ヘッダファイル
#include <stdio.h>

// 3. フレームワーク（Cocoa Touch）
#import <CoreLocation/CLBeacon.h>
#import <CoreLocation/CoreLocation.h>
#import <QuartzCore/QuartzCore.h>

// 4. フレームワーク（Cocoa Touch以外）
#import <utilities/cocoa/UIImage+Extensions.h>
#import <utilities/tools/NSString+Extensions.h>
#import <utilities/tools/UIView+Extensions.h>

// 5. アプリ
#import "GSLImageView.h"
#import "GSLMainView.h"
#import "GSLSubView.h"
```

### ヘッダファイル

* import/includeディレクティブ、`@class`ディレクティブ（Cocoa Touch、アルファベット順）、`@class`ディレクティブ（Cocoa Touch以外、アルファベット順）、定数、クラスインターフェイスの順で記述すること。

```objective-c
//
//  GSLJSONObjectViewController.h
//  GSLDemo
//

@import UIKit.UITableViewController;

@class CBUUID, MKShape;
@class GSLObject;

NS_ASSUME_NONNULL_BEGIN

@interface GSLJSONObjectViewController : UITableViewController

+ (instancetype)JSONObjectViewControllerWithJSONObject:(id)JSONObject;

@property (nonatomic, readonly, copy) id JSONObject;

- (instancetype)initWithJSONObject:(id)JSONObject NS_DESIGNATED_INITIALIZER;

@end

NS_ASSUME_NONNULL_END
```

### ソースファイル

* `import` / `include`ディレクティブ、クラス拡張、定数、クラス実装の順で記述すること。
* クラス実装の記述順については下記の通りとする。（サンプル中のコメントは不要）
    1. クラスメソッド
    2. `init` / `dealloc`メソッド
    3. インスタンスメソッド
    4. スーパークラスメソッド
    5. カテゴリメソッド（Cocoa Touch、アルファベット順）
    6. カテゴリメソッド（Cocoa Touch以外、アルファベット順）
    7. クラス拡張
* スーパークラス／カテゴリの区切りで`#pragma`ディレクティブを挿入すること。
* メソッドの記述順はヘッダの記述順と合わせること。

```objective-c
//
//  GSLJSONObjectViewController.m
//  GSLDemo
//

@import UIKit;

#import "GSLJSONObjectViewController.h"

NS_ASSUME_NONNULL_BEGIN

@interface GSLJSONObjectViewController ()

@property (nonatomic, readwrite, copy) id JSONObject;

@end

NS_ASSUME_NONNULL_END

@implementation GSLJSONObjectViewController

// class methods

+ (instancetype)JSONObjectViewControllerWithJSONObject:(id)JSONObject {
    return [[self alloc] initWithJSONObject:JSONObject];
}

// init/dealloc methods

- (instancetype)initWithStyle:(UITableViewStyle)style {
    return [self initWithJSONObject:nil];
}

- (instancetype)initWithJSONObject:(id)JSONObject {
    self = [self initWithStyle:UITableViewStyleGrouped];
    if (self) {
        self.JSONObject = JSONObject;
    }
    return self;
}

- (void)dealloc {
    [[NSNotificationCenter defaultCenter] removeObserver:self];
}

// instance methods

#pragma mark - UITableViewController

// UITableViewController methods

#pragma mark - UIViewController

// UIViewController methods

#pragma mark - NSKeyValueObserving

// NSKeyValueObserving category methods

#pragma mark - UITableViewDataSource

// UITableViewDataSource category methods

#pragma mark - UITableViewDelegate

// UITableViewDelegate category methods

#pragma mark - Class extensions

// Class extensions methods

@end
```

## その他

* `[[Object alloc] init]`という表現は`[Object new]`と記述すること。
* 不要な非同期処理は決して行わないこと。
    * UI調整用であれば、その旨のコメントを必ず残すこと。
* ANSI Cに存在しない機能は極力避けること（GNU C拡張）。
* 対象となるターゲットを常に意識しプログラミングを行うこと。
    * アーキテクチャー：armv7 / armv7s / arm64 / i386 / x86_64
    * 32-bit / 64-bit

## 改定履歴

* 2018/11/29
    * コメントに `HACK` プレフィックスを追加。
* 2018/11/24
    * Swift言語との相互運用性を追加。
* 2018/04/12
    * ブロックを追加。
    * API可用性を追加。
    * コード構成を修正。
* 2017/11/08
    * リンクを修正。
* 2017/08/14
    * プロパティの属性に `class` と `nullable` / `nonnull` を追加。
* 2015/09/21
    * 『Objective-Cスタイルガイド』の初版。
