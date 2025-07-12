# BitzDoc

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

BitzDocは、BitzLabsエコシステムの中核をなすライブラリです。ドキュメントの論理構造(`DocAST`)を定義し、Markdownをパースし、そして他の専門ライブラリ(`BitzPlot`, `BitzMath`など)で生成されたリッチコンテンツを単一のドキュメントに統合します。

## 主な機能

-   **`DocAST`の定義**: 段落、見出し、リストといった基本的な文書構造と、他の専門ASTを埋め込むための`EmbeddedBlockNode`を定義します。
-   **2つの入力ルート**:
    1.  **Markdownパーサー**: Markdownテキストを`DocAST`に変換します。コードブロックの言語情報に基づいて適切な専門パーサーを呼び出し、リッチコンテンツを自動的にパースして埋め込みます。
    2.  **専用API (Builder API)**: C#やTypeScriptのコードから、`DocAST`をプログラム的に構築できます。動的なレポート生成などに利用できます。
-   **HTMLコンパイラ (第1目標)**: `DocAST`を受け取り、HTML文字列を生成します。埋め込まれた専門ASTは、対応するライブラリのレイアウトエンジンとレンダラーを呼び出してSVGなどに変換し、HTML内に埋め込みます。
-   **(第2目標) レイアウトエンジン**: `DocAST`と`CompositionAST`を元に、高品質な固定レイアウト出力のための`LayoutAST`を生成します。

## ✅ 初期開発ToDoリスト

1.  **`DocAST`の型定義**:
    *   `IDocNode`インターフェース(`IAstNode`継承)を定義。
    *   `ParagraphNode`, `HeadingNode`, `EmbeddedBlockNode`など、主要なASTノードクラスの「殻」を作成。
2.  **Markdownパーサーの骨格**:
    *   `MarkdownParser`クラスを作成。既存のMarkdownライブラリ(例: Markdig)のラッパーとして実装。
    *   ` ```plot `のようなコードブロックを検知し、対応する専門パーサーを呼び出すための拡張ポイント（フック）を実装。
3.  **専用API (ビルダー) の設計**:
    *   `DocumentBuilder`クラスを設計。`.AddHeading("Title")`, `.AddParagraph("...")`のような基本的なメソッドを定義。
4.  **HTMLコンパイラのインターフェース**:
    *   `HtmlCompiler`クラスを作成し、`Compile(IDocNode node)`メソッドを定義。
    *   初期実装として、`HeadingNode`を`<h1>`に、`ParagraphNode`を`<p>`に変換するロジックを実装。
    *   `EmbeddedBlockNode`を処理するロジックの骨格を実装。内部で専門エンジンを呼び出し、レンダラー(例: `BitzRenderers.SvgRenderer`)でSVG文字列を取得し、HTMLに埋め込む流れを構築。

## このライブラリの位置づけ

`BitzDoc`は、BitzLabsエコシステムの「司令塔」です。様々な専門ライブラリをオーケストレーションし、最終的なドキュメントを組み立てる責務を担います。

### 依存関係

-   `BitzAstCore`, `BitzParser`, `BitzLayout`
-   (動的に解決) `BitzMath`, `BitzPlot`, `BitzGraph`, `BitzFlow`, `BitzUi` など

---
