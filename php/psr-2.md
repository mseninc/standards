## コーディングスタイルガイド (PSR-2 Coding Style Guide)

PSR-2 は、 PHP における基本的なコーディング標準である PSR-1 を拡張し、 PHP コードをどのようにフォーマットするかについての共通の規則を定めています。

このベーシックなガイドラインをメンバーと共有しプロジェクトを進める事で、コードの共通化や、ソースコードリーディングの時間短縮を図る事ができます。

### コーディングスタイル・形式

本セクションでは、基本のコーディングスタイルやファイルの形式について定めています。

#### 基本的なコーディング標準

コードは、 [PSR-1 （基本コーディング標準）](./psr-1.md) に記載されているすべてのルールに従う。

#### ファイル

- すべての PHP ファイルは Unix LF （改行）行末を使用する。
- すべての PHP ファイルは空白行で終わらせる。
- PHP のみのファイルの場合、終了タグ ?> は省略する。

#### 行

本セクションでは、行レベルでの記述について定めています。

##### 文字数

- 1 行にコーディングする文字数は厳密に制限を設けない。
- 行の長さのソフトリミットは 120 文字を上限とします。 自動スタイルチェッカーはソフトリミットで警告しなければなりませんが、エラーを出してはいけません。
- 1 行の文字数は 80 文字を超えないように努める。それよりも長い場合は、複数行に分割して記述する。

##### 空白

- 行の末尾に空白を入力しない。
- 可読性を向上させる為、関連するコードブロックを示す為に空白行を追加する事は良しとする。

##### ステートメント

- 1 行に複数のステートメント（宣言）は書かない。

#### インデント

インデントはタブではなく、 4 つの半角空白スペースを使用する。

#### 予約語と True / False / Null

- PHP の [キーワード](https://php.net/manual/ja/reserved.keywords.php) は小文字で記述する。
- PHP の定数 `true`, `false`, `null` は小文字で記述する。

### 名前空間と宣言

- 名前空間の定義の後に空白行が必要です。
- 全ての use 定義は、名前空間宣言の後に記述する。
- 定義ごとに use 演算子が必要です。
- use 定義のブロックの後には空白行が必要です。

```php
<?php
namespace App\Http\Controllers;

use Illuminate\Http\Request;
use App\Facades\FooNotification AS Foo;

class SampleController extends Controller
{
```

### クラス・プロパティ・メソッド

ここでのクラスは、全ての一般クラス、インターフェイス、トレイトを指しています。

#### 拡張と実装

- extends と implements はクラス名と同じ行で宣言する。
- クラスの開き括弧は次の行に記述しなければなりません。
- クラスの閉じ括弧は本文最後の次の行に記述しなければなりません。

```php
class MyClass extends YouClass implements SheClass, HeClass
{
  
}
```

- implements 定義は、インデントにより揃えることで、複数行に分割しても構いません。その際、最初の定義も次の行からはじめるものとし、 1 行に 1 つのインターフェイスを定義しなければなりません。

```php
class ClassName extends ParentClass implements
    \ArrayAccess,
    \Countable,
    \Serializable
{
```

#### プロパティ

- アクセス修飾子は、全てのプロパティに定義しなければなりません。

```php
class SampleClass
{
    public $foo;
    private $bar;
    protected $baz;
}
```

- プロパティ定義に、 var は使用してはいけません。
- 1 つのステートメントで 2 つ以上のプロパティは定義してはいけません。
- プロパティ名に、 protected または private を示すための接頭辞アンダースコアは使用してはいけません。

```php
/**
 * 適切な例
 */
class SampleClass
{
    public $foo;
    public $bar;
    private $baz;
    protected $qux;
}
```

#### メソッド

- アクセス修飾子は、全てのメソッドに定義しなければなりません。
- メソッド名に、 protected または private を示すための接頭辞アンダースコアは使用してはいけません。
- メソッド名の後ろにスペースを使用してはいけません。
- 開き括弧は次の行に記述しなければなりません。
- 閉じ括弧は本文最後の次の行に記述しなければなりません。
- 開き括弧の後ろや、閉じ括弧の前にスペースがあってはいけません。

```php
class SampleController
{
    public function index($arg1, $arg2)
    {
        // method body
    }
}
```

#### メソッドの引数

- 引数を記述する際には、カンマの前があってはいけません。
- また各カンマの後ろには 1 スペースおかなければなりません。
- デフォルト値を持つ引数は、引数リストの最後に配置しなければなりません。

```php
class SampleController
{
    public function index($arg1, &$arg2, $arg3 = [])
    {
        // method body
    }
}
```

- 引数リストは、インデントにより揃えることで、複数行に分割しても構いません。 その際、最初の定義も次の行からはじめるものとし、 1 行に 1 つの引数を定義しなければなりません。
- 引数リストを複数行の分割配置としている場合、閉じ括弧と開き中括弧の間にはスペースを含め同じ行に配置する必要があります。

```php
class SampleController
{
    public function aVeryLongMethodName(
        ClassTypeHint $arg1,
        &$arg2,
        array $arg3 = []
    ) {
        // method body
    }
}
```

#### abstract / final / static

- abstract と final はアクセス修飾子の前に定義しなければなりません。
- static はアクセス修飾子の後に定義しなければなりません。

```php
abstract class ClassName
{
    protected static $foo;
  
    abstract protected function zim();
  
    final public static function bar()
    {
      // method body
    }
}
```

#### メソッドと関数の呼び出し

- メソッドや関数の呼び出し時は、メソッドや関数名と開き括弧の間にスペースがあってはなりません。
- 開き括弧の後や、閉じ括弧の前にスペースがあってはなりません。
- 引数リストの前にスペースがあってはなりませんが、各カンマの後に 1 スペースが必要です。

```php
bar();
$foo->bar($arg1);
Foo::bar($arg2, $arg3);
```

- 引数リストは、インデントにより揃えることで、複数行に分割しても構いません。 その際、最初の定義も次の行からはじめるものとし、 1 行に 1 つの引数を定義しなければなりません。

```php
$foo->bar(
    $longArgument,
    $longerArgument,
    $muchLongerArgument
);
```

### 制御構造

制御構造とは、if や foreach, try-catch などの、制御を行う処理の構造の事です。

制御構造の一般的なスタイルルールは以下の通りです。

- 制御構造キーワードの後には 1 スペースを設けなければなりません。
- 開き括弧の後にスペースを配置してはなりません。
- 閉じ括弧の前にスペースを配置してはなりません。
- 開き括弧と閉じ中括弧の間にはスペースを挟まなければなりません。
- 構造本文は 1 インデント下げなければなりません。
- 閉じ括弧は構造本文の後に改行して配置しなければなりません。
- 各構造本文は、中括弧で囲わなければなりません。 これは構造の見え方を標準化し、追加実装等が発生した際のエラーを抑えます。

#### if / elseif / else

if 制御は下記のようになります。

```php
if ($expr1) {
    // if body
} elseif ($expr2) {
    // elseif body
} else {
    // else body
}
```

- 括弧、スペース、および中括弧の配置に注意してください。
- else と elseif は前のボディの閉じた終了波括弧と同じ行にある。
- 全ての制御キーワードが単一に見えるように、else if ではなく elseif を使う。

#### switch / case

switch 制御は下記のようになります。

```php
switch ($expr) {
    case 0:
        echo '通常パターン。 break します。';
        break;
    case 1:
        echo 'no break パターン。処理は継続されます。';
        // no break
    case 2:
    case 3:
    case 4:
        echo 'return パターン, break の代わりに return します。';
        return;
    default:
        echo 'Default case';
        break;
}
```

- 括弧、スペース、中括弧の配置に注意してください。
- case 文は switch から 1 インデント下げなければなりません。
- break キーワード（または他の終了キーワード）は case 本体と同じレベルでインデントしなければなりません。
- 意図的に処理スルーさせる場合は「// no break」等、コメントしなければなりません。

#### while / do while

while 制御および do while 制御は下記のようになります。

```php
while ($expr) {
    // structure body
}

do {
    // structure body
} while ($expr);
```

- 括弧、スペース、中括弧の配置に注意してください。

#### for

for 制御は下記のようになります。

```php
for ($i = 0; $i < 10; $i++) {
    // for body
}
```

- 括弧、スペース、中括弧の配置に注意してください。

#### foreach

foreach 制御は下記のようになります。

```php
foreach ($iterable as $key => $value) {
    // foreach body
}
```

- 括弧、スペース、中括弧の配置に注意してください。

#### try catch

foreach 制御は下記のようになります。

```php
try {
    // try body
} catch (FirstExceptionType $e) {
    // catch body
} catch (OtherExceptionType $e) {
    // catch body
}
```

- 括弧、スペース、中括弧の配置に注意してください。


### クロージャー

- function キーワード の後にスペースを、 use キーワード の前後にスペースが必要です。
- 開き括弧は同じ行に記述しなければなりません。また閉じ括弧は本文最後の次の行に記述しなければなりません。
- 引数または変数リストの開き括弧の後にスペースがあってはなりません。 また閉じ括弧の前にスペースがあってもなりません。
- 引数または変数リストの前にスペースがあってはなりませんが、各カンマの後に 1 スペースが必要です。
- デフォルト値を持つ引数は、引数リストの最後に配置しなければなりません。
- 括弧、スペース、中括弧の配置に注意してください。

クロージャ定義は下記のようになります。

```php
$closureWithArgs = function ($arg1, $arg2) {
    // body
};

$closureWithArgsAndVars = function ($arg1, $arg2) use ($var1, $var2) {
    // body
};
```

引数リストと変数リストは複数の行に分割できる。その場合の書式については以下の通り。

引数または変数リストは、インデントにより揃えることで、複数行に分割しても構いません。その場合の書式については以下の通り。

- 最初の定義も次の行からはじめるものとし、 1 行に 1 つの引数または変数を定義しなければなりません。
- 引数または変数リストを複数行の分割配置としている場合、閉じ括弧と開き中括弧の間にはスペースを含め同じ行に配置する必要があります。

複数の行に分割された引数リストと変数リストがある場合とない場合のクロージャ実装は下記のようになります。

```php
$longArgsNoVars = function (
    $longArgument,
    $longerArgument,
    $muchLongerArgument
) {
   // body
};

$noArgsLongVars = function () use (
    $longVar1,
    $longerVar2,
    $muchLongerVar3
) {
   // body
};

$longArgsLongVars = function (
    $longArgument,
    $longerArgument,
    $muchLongerArgument
) use (
    $longVar1,
    $longerVar2,
    $muchLongerVar3
) {
   // body
};

$longArgsShortVars = function (
    $longArgument,
    $longerArgument,
    $muchLongerArgument
) use ($var1) {
   // body
};

$shortArgsLongVars = function ($arg) use (
    $longVar1,
    $longerVar2,
    $muchLongerVar3
) {
   // body
};
```

引数として、クロージャが関数またはメソッドに直接使用される場合もまた、ルールが適用されることに注意してください。

```php
$foo->bar(
    $arg1,
    function ($arg2) use ($var1) {
        // body
    },
    $arg3
);
```

### その他

本スタイルガイドでは、意図的に省略しているスタイルやプラクティスが多くあります。 例えば下記のような幾つかについては、ここでは明記していません。

- グローバル変数と定数について
- 関数群の定義について
- 演算と代入について
- 行間の配置
- コメントとドキュメントブロックについて
- クラス名の接頭辞と接尾辞について
- ベストプラクティスについて

なお、本スタイルガイドは将来的に様々なスタイルやプラクティスの登場に応じて改定・拡張をできるものとします。
