# Compositeパターン

---

「Composite」とは「複合物」「合成物」という意味になります

---

## Compositeパターンとは？

---

単一オブジェクトとその集合のどちらも同じように扱えるようにする為のパターンとして説明されています

このパターンは、デザインパターンの中でも「構造に関するパターン」に属すると言われています

---

> Composite パターンを用いるとディレクトリとファイルなどのような、木構造を伴う再帰的なデータ構造を表すことができる。

https://ja.wikipedia.org/wiki/Composite_パターン

（階層構造図）

---

## Compositeパターンの特徴は

---

Compositeパターンを利用することでクライアントは単一のオブジェクト、または
複数のオブジェクトを持つグループオブジェクトに対して同じAPIでアクセスすることが可能になります

---

ディレクトリツリーを例に説明すると、ディレクトリだろうがファイルだろうが同じAPIでアクセスできる、つまり作成・複製・削除などのアクションを行う際に対象とするオブジェクトが単一オブジェクト（ファイル）なのか、グループオブジェクト（フォルダ）なのかを意識する必要がなく行えるということです

---

## サンプルコード

Compositeパターンを使用してフォルダとファイルを構成するクラスを作成してみます

---

```csharp
// 抽象クラス（構成）
public abstract class TreeEntry
{
  private string name;

  public TreeEntry(string name)
  {
    this.name = name;
  }

  public abstract void Add(TreeEntry entry);

  public string Name
  {
    get
    {
      return this.name;
    }
  }
}

// フォルダクラス
public class Folder : TreeEntry
{
  private List<TreeEntry> list;

  public Folder(string name) : base(name)
  {
    list = new List<TreeEntry>(){};
  }

  public overwride void Add(TreeEntry entry)
  {
    list.Add(entry);
  }
}

// ファイルクラス
public class File : TreeEntry
{
  public File(string name) : base(name)
  {
  }

  public overwride void Add(TreeEntry entry)
  {
    throw new Exception("子要素の作成はできませんよ");
  }
}
```

---

* 抽象クラス[TreeEntry]にはAddメソッドを抽象メソッドとして定義しています
* Addメソッドは再帰的な構造を作るために必要であり、引数にTreeEntry型のオブジェクトを指定しているのがポイントです
* フォルダクラスとファイルクラスはともにTreeEntryクラスを継承して作成します
* もし、更にシンボリックリンクにも対応したいと思った時は同じようにTreeEntryクラスを継承して作成すればOKです

---

### クライアント側コード

```csharp
Folder subFolder = new Folder("デザインパターン");
subFolder.Add(new File("Stateパターン"));
subFolder.Add(new File("Bridgeパターン"));
subFolder.Add(new File("Compositeパターン"));

Folder rootFolder = new Folder("ワークショップ");
rootFolder.Add(subFolder);
```

---

* 「デザインパターン」フォルダを作成して各種パターンファイルを追加
* 「ワークショップ」フォルダに「デザインパターン」フォルダを追加しています

このようにAddメソッドの引数にフォルダオブジェクトであろうが、ファイルオブジェクトであろうが意識することなく指定することが可能です

---

### Compositeパターンを用いる際の注意点

データ構造がきちんとツリー構造を保つようにしなければなりません
親子関係が循環してしまった場合、無限ループに陥る可能性があるみたいです（汗）

---

## 最後に

Compositeパターンのメリットについておさらいします

1. クライアント側の記述が書きやすい（APIが共通）
1. 新しい子要素を追加しやすい
1. 再帰的な構造の記述が簡単なのでメンテナンスしやすい

---

Compositeパターンは、「ポリモーフィズム（Polymorphism）」を活用したパターンであると紹介されていました

ポリモーフィズム
https://ja.wikipedia.org/wiki/ポリモーフィズム

> ポリモーフィズム（英: Polymorphism）とは、プログラミング言語の型システムの性質を表すもので、プログラミング言語の各要素（定数、変数、式、オブジェクト、関数、メソッドなど）についてそれらが複数の型に属することを許すという性質を指す。ポリモルフィズム、多態性、多相性、多様性とも呼ばれる。
