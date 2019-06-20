## オートローダー (PSR-4 Autoloader)

PSR-4 は、 PHP でのファイルの読み込み・クラスのオートロードを行う仕様について記載されています。

### 概要

この PSR は、ファイルパスからクラスをオートロードするための仕様を記述しています。これは完全に相互運用可能で、 PSR-0 を含む他の自動ロード仕様に加えて使用できます。この PSR は、仕様に従って自動ロードされるファイルの配置場所も記述します。

### 仕様

「クラス」という用語は、クラス、インタフェース、トレイト、および他の同様の構造を指します。

#### 完全修飾クラス名の形式

完全修飾クラス名は下記の形式になります。

```php
\<NamespaceName>(\<SubNamespaceNames>)*\<ClassName>
```

- 完全修飾クラス名には、トップレベルの名前空間名（「ベンダー名前空間」とも呼ばれます）が必要です。
- 完全修飾クラス名は、 1つ 以上のサブ名前空間名を持つことができます。
- 完全修飾クラス名には、終了クラス名が必要です。
- アンダースコアは、完全修飾クラス名のどの部分にも特別な意味を持ちません。
- 完全修飾クラス名のアルファベット文字は、小文字と大文字の任意の組み合わせです。
- 大文字小文字を区別してすべてのクラス名を参照する必要があります。

#### ファイルのロード

- 完全修飾クラス名（名前空間接頭辞）内の先行する名前空間区切り文字を含まない 1つ 以上の先頭の名前空間およびサブ名前空間名の連続したシリーズは、少なくとも 1つ の「ベースディレクトリ」に対応します。
- 「名前空間接頭辞」の後の隣接するサブ名前空間名は、「ベースディレクトリ」内のサブディレクトリに対応し、名前空間区切り記号はディレクトリ区切り記号を表す。 サブディレクトリ名は、サブ名前空間名の大文字と小文字を区別しなければならない。
- 終了クラス名は、 `.php` で終わるファイル名に対応します。
- ファイル名は終端クラス名の大文字と一致する必要があります。

#### 注意

オートローダーの実装は、例外をスローしてはならず、どのレベルのエラーも発生してはなりません。また値を返すべきではありません。

#### 完全修飾クラス名～ファイルパス例

| 完全修飾クラス名             | 名前空間接頭辞  | ベースディレクトリ     | 結果のファイルパス                        |
| ---------------------------- | --------------- | ---------------------- | ----------------------------------------- |
| \Acme\Log\Writer\File_Writer | Acme\Log\Writer | ./acme-log-writer/lib/ | ./acme-log-writer/lib/File_Writer.php     |
| \Aura\Web\Response\Status    | Aura\Web        | /path/to/aura-web/src/ | /path/to/aura-web/src/Response/Status.php |
| \Symfony\Core\Request        | Symfony\Core    | ./vendor/Symfony/Core/ | ./vendor/Symfony/Core/Request.php         |
| \Zend\Acl                    | Zend            | /usr/includes/Zend/    | /usr/includes/Zend/Acl.php                |

### 実装例

#### クロージャ

```php

<?php
/**
 * クロージャでの実装例
 *
 * このオートロード機能をSPLに登録した後、次の行は、関数が
 * \Foo\Bar\Baz\Qux クラスを
 * /path/to/project/src/Baz/Qux.phpからロードしようとします：
 *
 *      new \Foo\Bar\Baz\Qux;
 *
 * @param string $class 完全修飾クラス名
 * @return void
 */
spl_autoload_register(function ($class) {

    // プロジェクト固有の名前空間接頭辞
    $prefix = 'Foo\\Bar\\';

    // 名前空間接頭辞のベースディレクトリ
    $base_dir = __DIR__ . '/src/';

    // クラスは名前空間接頭辞を使用しますか？
    $len = strlen($prefix);
    if (strncmp($prefix, $class, $len) !== 0) {
        // いいえ、次に登録されたオートローダーに移動します。
        return;
    }

    // 相対クラス名を取得する
    $relative_class = substr($class, $len);

    // 名前空間接頭辞をベースディレクトリに置き換え、
    // 名前空間区切り文字を相対クラス名のディレクトリ区切り文字で置き換え、
    //.phpで追加します
    $file = $base_dir . str_replace('\\', '/', $relative_class) . '.php';

    // ファイルが存在する場合は読み込む
    if (file_exists($file)) {
        require $file;
    }
});
```

#### クラス

以下は単一の名前空間接頭辞に対して複数のベースディレクトリを許可するオプション機能を含む汎用実装の例です。

次のパスのファイルシステムにクラスの foo-bar パッケージがあるとします。

```
/path/to/packages/foo-bar/
    src/
        Baz.php             # Foo\Bar\Baz
    Qux/
        Quux.php            # Foo\Bar\Qux\Quux
    tests/
        BazTest.php         # Foo\Bar\BazTest
        Qux/
            QuuxTest.php    # Foo\Bar\Qux\QuuxTest
```

\Foo\Bar\namespace 接頭辞のクラスファイルへのパスを次のように追加します。

```php
<?php
// ローダをインスタンス化する
$loader = new \Example\Psr4AutoloaderClass;

// オートローダを登録する
$loader->register();

// 名前空間接頭辞のベースディレクトリを登録する
$loader->addNamespace('Foo\Bar', '/path/to/packages/foo-bar/src');
$loader->addNamespace('Foo\Bar', '/path/to/packages/foo-bar/tests');
```

次の行は、オートローダーが /path/to/packages/foo-bar/src/Qux/Quux.php から \Foo\Bar\Qux\Quux クラスをロードしようとします。

```php
<?php
new \Foo\Bar\Qux\Quux;
```

次の行は、オートローダーが /path/to/packages/foo-bar/tests/Qux/QuuxTest.php から \Foo\Bar\Qux\QuuxTest クラスをロードしようとします。

```php
<?php
new \Foo\Bar\Qux\QuuxTest;
```

次に、複数の名前空間を処理するためのクラス実装の例を示します。

```php
<?php
namespace Example;

class Psr4AutoloaderClass
{
    /**
     * キーが名前空間プレフィックスであり、値がその名前空間内の
     * クラスの基本ディレクトリの配列である連想配列
     *
     * @var array
     */
    protected $prefixes = array();

    /**
     * ローダーをSPLオートローダスタックに登録する
     *
     * @return void
     */
    public function register()
    {
        spl_autoload_register(array($this, 'loadClass'));
    }

    /**
     * 名前空間接頭辞のベースディレクトリを追加する
     *
     * @param string    $prefix    名前空間接頭辞
     * @param string    $base_dir  名前空間内のクラスファイルのベースディレクトリ
     * @param bool      $prepend   trueの場合はベースディレクトリを追加するのでは
     *                              なくスタックに追加します。 これにより、最後に
     *                              検索されるのではなく最初に検索されます。
     * @return void
     */
    public function addNamespace($prefix, $base_dir, $prepend = false)
    {
        // 名前空間接頭辞を正規化する
        $prefix = trim($prefix, '\\') . '\\';

        // 後続の区切り文字でベースディレクトリを正規化する
        $base_dir = rtrim($base_dir, DIRECTORY_SEPARATOR) . '/';

        // 名前空間接頭辞配列を初期化する
        if (isset($this->prefixes[$prefix]) === false) {
            $this->prefixes[$prefix] = array();
        }

        // 名前空間接頭辞のベースディレクトリを保持する
        if ($prepend) {
            array_unshift($this->prefixes[$prefix], $base_dir);
        } else {
            array_push($this->prefixes[$prefix], $base_dir);
        }
    }

    /**
     * 指定されたクラス名のクラスファイルを読み込む
     *
     * @param string $class 完全修飾クラス名
     * @return mixed 成功：マップされたファイル名　失敗：false。
     */
    public function loadClass($class)
    {
        // 現在の名前空間接頭辞
        $prefix = $class;

        // マップされたファイル名を見つけるために、完全修飾クラス名の名前空間名を処理する
        while (false !== $pos = strrpos($prefix, '\\')) {

            // 接頭辞に後続する名前空間の区切りを保持する
            $prefix = substr($class, 0, $pos + 1);

            // 相対クラス名
            $relative_class = substr($class, $pos + 1);

            // 接頭辞と相対クラスのマップされたファイルを読み込もうとする
            $mapped_file = $this->loadMappedFile($prefix, $relative_class);
            if ($mapped_file) {
                return $mapped_file;
            }

            // strrpos（）の次の反復の後続の名前空間区切りを削除する
            $prefix = rtrim($prefix, '\\');
        }

        return false;
    }

    /**
     * 名前空間接頭辞と相対クラスのマッピングされたファイルを読み込む
     *
     * @param   string  $prefix 名前空間接頭辞
     * @param   string  $relative_class 相対クラス名
     * @return  mixed Boolean false ロードされたファイル名（マップされたファイルがロードできない場合はfalse）
     */
    protected function loadMappedFile($prefix, $relative_class)
    {
        // 渡された名前空間プレフィックスのベースディレクトリがなければfalseを返す
        if (isset($this->prefixes[$prefix]) === false) {
            return false;
        }

        // 渡された名前空間接頭辞のベースディレクトリを調べる
        foreach ($this->prefixes[$prefix] as $base_dir) {

            // 名前空間接頭辞をベースディレクトリに置き換え、
            // 名前空間区切り文字を相対クラス名のディレクトリ区切り文字で置き換え、
            // .phpで追加する
            $file = $base_dir
                . str_replace('\\', '/', $relative_class)
                . '.php';

            // マップされたファイルが存在する場合は読み込む
            if ($this->requireFile($file)) {
                return $file;
            }
        }

        return false;
    }

    /**
     * ファイルが存在する場合はファイルシステムから読み込む
     *
     * @param string $file  必要なファイル
     * @return bool True    ファイルが　存在する：false　存在しない：false
     */
    protected function requireFile($file)
    {
        if (file_exists($file)) {
            require $file;
            return true;
        }
        return false;
    }
}
```

#### ユニットテスト

次の例は上記クラスローダーの単体テストを行う 1つ の方法です。

```php
<?php
namespace Example\Tests;

class MockPsr4AutoloaderClass extends Psr4AutoloaderClass
{
    protected $files = array();

    public function setFiles(array $files)
    {
        $this->files = $files;
    }

    protected function requireFile($file)
    {
        return in_array($file, $this->files);
    }
}

class Psr4AutoloaderClassTest extends \PHPUnit_Framework_TestCase
{
    protected $loader;

    protected function setUp()
    {
        $this->loader = new MockPsr4AutoloaderClass;

        $this->loader->setFiles(array(
            '/vendor/foo.bar/src/ClassName.php',
            '/vendor/foo.bar/src/DoomClassName.php',
            '/vendor/foo.bar/tests/ClassNameTest.php',
            '/vendor/foo.bardoom/src/ClassName.php',
            '/vendor/foo.bar.baz.dib/src/ClassName.php',
            '/vendor/foo.bar.baz.dib.zim.gir/src/ClassName.php',
        ));

        $this->loader->addNamespace(
            'Foo\Bar',
            '/vendor/foo.bar/src'
        );

        $this->loader->addNamespace(
            'Foo\Bar',
            '/vendor/foo.bar/tests'
        );

        $this->loader->addNamespace(
            'Foo\BarDoom',
            '/vendor/foo.bardoom/src'
        );

        $this->loader->addNamespace(
            'Foo\Bar\Baz\Dib',
            '/vendor/foo.bar.baz.dib/src'
        );

        $this->loader->addNamespace(
            'Foo\Bar\Baz\Dib\Zim\Gir',
            '/vendor/foo.bar.baz.dib.zim.gir/src'
        );
    }

    public function testExistingFile()
    {
        $actual = $this->loader->loadClass('Foo\Bar\ClassName');
        $expect = '/vendor/foo.bar/src/ClassName.php';
        $this->assertSame($expect, $actual);

        $actual = $this->loader->loadClass('Foo\Bar\ClassNameTest');
        $expect = '/vendor/foo.bar/tests/ClassNameTest.php';
        $this->assertSame($expect, $actual);
    }

    public function testMissingFile()
    {
        $actual = $this->loader->loadClass('No_Vendor\No_Package\NoClass');
        $this->assertFalse($actual);
    }

    public function testDeepFile()
    {
        $actual = $this->loader->loadClass('Foo\Bar\Baz\Dib\Zim\Gir\ClassName');
        $expect = '/vendor/foo.bar.baz.dib.zim.gir/src/ClassName.php';
        $this->assertSame($expect, $actual);
    }

    public function testConfusion()
    {
        $actual = $this->loader->loadClass('Foo\Bar\DoomClassName');
        $expect = '/vendor/foo.bar/src/DoomClassName.php';
        $this->assertSame($expect, $actual);

        $actual = $this->loader->loadClass('Foo\BarDoom\ClassName');
        $expect = '/vendor/foo.bardoom/src/ClassName.php';
        $this->assertSame($expect, $actual);
    }
}
```