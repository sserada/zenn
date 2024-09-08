---
title: "Svelte Writableストア 概説"
emoji: "❄️"
type: "tech"
topics: [TypeScript, Svelte]
published: true
---

# ストアとは

Svelteにおいて，変数は通常，単一のコンポーネント内でしか使用できない．しかし，アプリケーションが複雑になるにつれて，複数のコンポーネント間で状態を共有したい場面が出てくる．このような場合，Svelteの「**ストア**」という仕組みを使用することで，複数のコンポーネント間で変数を簡単に共有することが可能となる．

Svelteには，用途に応じて使い分けることができるいくつかのストアタイプが用意されている．具体的には，以下の4種類が存在する．

- **Writableストア**: 値の参照および更新が可能なストア．最も基本的でよく使われるストアタイプ．
- **Readableストア**: 値の参照のみが可能なストア．他のコンポーネントや処理から値の変更を防ぎたい場合に利用される．
- **Derivedストア**: 他のストアの値に応じて動的に値が変化するストア．たとえば，あるストアの値に基づいて自動的に計算された結果を保持するために使用する．
- **カスタムストア**: 上記のストアタイプでは対応できない独自のロジックが必要な場合に，Svelteが提供する関数以外の方法で作成されたストア．

当記事では，Writableストアに焦点を当て，その基本的な使い方と応用例について詳しく説明する．

# Writableストア

**Writableストア**は，Svelteのストアの中で最も一般的であり，値の参照と更新の両方が可能なストアである．これにより，複数のコンポーネント間で共有される状態を動的に管理することができる．

まず，基本的なWritableストアの定義と使用方法を見ていく．以下に示すのは，数値型（number型）のWritableストアを使用し，複数のコンポーネント間で数値を共有および操作する例である．これは，Svelte公式で公開されているWritableストアのチュートリアルプログラムをベースに，私が少し変更を加えたものである．

```tsx
import { type Writable, writable } from 'svelte/store';

export const count: Writable<number> = writable(0);
```

このコードでは，`svelte/store`から`writable`関数と型の`Writable`をインポートしている．`writable`関数は，指定した初期値を持つ新しいWritableストアを作成するための関数である．ここでは，初期値として`0`を渡し，それを`count`という名前でエクスポートしている．また，`Writable`型を明示的に指定することで，このストアが`number`型の値を保持することが明確になる．

Writableストアに対しては，以下の3つの主要な操作が可能である．

- **`set(value)`**: ストアの現在の値を`value`に設定する．これにより，ストアを利用しているすべてのコンポーネントに対して新しい値が通知される．
- **`subscribe(callback)`**: ストアを購読し，ストアの値が変化するたびに`callback`関数が呼び出される．`callback`は，ストアの新しい値を引数として受け取る．購読を解除するための関数が戻り値として返されるため，必要に応じて購読を解除することができる．
- **`update(updater)`**: ストアの現在の値をもとに，新しい値を計算し，それをストアに設定する．`updater`は現在の値を引数に受け取り，計算後の新しい値を返す関数である．例えば，カウンターの値を1増加させたい場合には，このメソッドを使用する．

次に，上記で作成した`count`ストアを使って，具体的なプログラムを実装する例を紹介する．以下のようなファイル構成を仮定する．

```bash
src
├── app.d.ts
├── app.html
├── lib
│   ├── components
│   │   ├── Decrementer.svelte
│   │   └── Incrementer.svelte
│   └── stores.ts
└── routes
    └── +page.svelte
```

この構成に基づき，まず`+page.svelte`にて，現在の`count`の値と`Incrementer`および`Decrementer`コンポーネントを表示するコードを以下に示す．

```html
<script lang='ts'>
  import { count } from '$lib/stores.ts';
  import Incrementer from '$lib/components/Incrementer.svelte';
  import Decrementer from '$lib/components/Decrementer.svelte';

  let count_value: number;

  count.subscribe((value) => {
    count_value = value;
  });
</script>

<h1>The count is {count_value}</h1>
<Incrementer />
<Decrementer />
```

このコードでは，`count.subscribe()`を使用して`count`ストアを購読し，ストアの値が変更されるたびに`count_value`が更新されるようになっている．`count_value`は，HTML内の`<h1>`タグを通じて表示され，常に最新の状態を反映する．

次に，`count`ストアの値を変更するための`Incrementer.svelte`コンポーネントを実装する．

```html
<script lang='ts'>
  import { count } from '$lib/stores.ts';

  function increment() {
    count.update((n) => n + 1);
  }
</script>

<button on:click={increment}>+</button>

<style>
  button {
    width: 40px;
    height: 40px;
    border: solid black 1.5px;
    border-radius: 50%;
    background: #fff;
  }
</style>
```

この`Incrementer.svelte`は，ストアに保存されている数値を1増加させるボタンコンポーネントである．`update`メソッドを使用して，現在の値を基に新しい値（ここでは1増加）を計算し，それをストアに設定している．同様に，`Decrementer.svelte`では，現在の値から1を引くボタンコンポーネントを実装する．

これにより，以下のようにストアを介して，複数のコンポーネント間で状態を共有しながら，値の変更を反映するアプリケーションを作成することができる．

![アプリケーションイメージ](/images/20240809-svelte-writable-store/application_image.png)

ここまで，Writableストアの基本的な使い方と具体的な応用例について説明してきた．
ストアを使用することで，複数のコンポーネント間で状態を共有し，リアクティブな動作を容易に実現することが可能となる．
特に，複数人で開発を行う場合や，アプリケーションの規模が大きくなる場合には，ストアを活用することでコードの保守性や可読性を向上させることができる．

**参考**

1. Kyohei Hamaguchi, 小関康裕. 『実践 Svelte入門』. 技術評論社, 2023, pp. 97-112.
2. LEARN.SVELTE.DEV. “Writable stores”. SVELTE.DEV. https://svelte.jp/docs/svelte-store, (2023-08-03).

