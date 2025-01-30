---
title: "[Discord.py] Discord Botのコマンドを簡単に定義！コマンド拡張機能の使い方"
emoji: "🐍"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: [python, discord, discord.py]
published: true
---

# はじめに
` discord.py `のコマンド拡張機能は，ボット開発者が効率的にコマンドを定義・管理し，サブコマンドを含む複雑なコマンド体系を構築することを容易にしてくれます．これを活用することで，ボットの機能を柔軟かつ強力に拡張することができます．
この記事では，そんな` discord.py `のコマンド拡張機能の使い方について解説します．なお，本記事では` discord.py `の基礎的な使い方については触れませんので，ご了承ください．

# コマンドの定義
コマンドは，Pythonの関数にデコレータを適用することで定義されます．例えば，以下のように` foo `というコマンドを定義できます．

```python
import discord
from discord.ext import commands # コマンド拡張機能をインポート

# Botのインスタンスを作成
intents = discord.Intents.default()
intents.message_content = True
bot = commands.Bot(command_prefix='!', intents=intents)

# コマンドを定義
@bot.command()
async def foo(ctx):
    await ctx.send('Hello, world!')
```
` @bot.command() `デコレータを使うことで，関数をコマンドとして登録することができます．この場合，以下のように` !foo `というコマンドを実行すると，` Hello, world! `というメッセージが送信されます．
![fooコマンド](/images/20250126-discord-command-ext/foo.png)

コマンドを登録する方法は主に，2つあります．一つは上記のように` @bot.command() `デコレータを使用する方法，もう一つは` @commands.command() `デコレータを使用し，` bot.add_command() `メソッドで手動で追加する方法です．どちらの方法も本質的には同等ですが，前者の方が簡潔で理解しやすいため，一般的に推奨されています．

また，デコレータには` Command `クラスのコンストラクタ因数を渡すことができます．例えば，コマンド名を関数名と異なるものにしたい場合，以下のように設定できます:

```python
@bot.command(name='bar')
async def foo(ctx):
    await ctx.send('Hello, world!')
```
この例では，` !bar `というコマンドで` foo `関数を実行できます．
![barコマンド](/images/20250126-discord-command-ext/bar.png)

# サブコマンドの定義
サブコマンドとは，コマンドの中で更に細かいコマンドを定義することです．
サブコマンドは，` @commands.group() `デコレータを使用して定義します．サブコマンドを持つコマンドは，` @commands.command() `デコレータを使用して定義した関数と同様に，` @コマンドグループ名.command() `デコレータを使用してサブコマンドを定義します．
以下の例では，` foo `というコマンドグループに` bar `と` baz `というサブコマンドを定義しています．

```python
@bot.group()
async def foo(ctx):
    if ctx.invoked_subcommand is None:
        await ctx.send('Invalid subcommand.')

@foo.command()
async def bar(ctx):
    await ctx.send('This is bar command.')

@foo.command()
async def baz(ctx):
    await ctx.send('This is baz command.')
```
この例では，` !foo bar `と` !foo baz `というコマンドで，それぞれ` This is bar command. `と` This is baz command. `というメッセージが送信されます．
![サブコマンド](/images/20250126-discord-command-ext/subcommand.png)

# パラメータの取り扱い
コマンド関数の引数は，ユーザからの入力に対応することができます．基本的な位置関数から可変調引数，キーワード専用引数まで，さまざまな形式の引数をサポートしています．

## 位置引数
最も基本的な引数は引数です．例えば，以下のように定義します:

```python
@bot.command()
async def greet(ctx, name: str):
    await ctx.send(f'Hello, {name}!')
```
ユーザが` !greet Alice `というコマンドを実行すると，` Hello, Alice! `というメッセージが送信されます．複数の単語を含む引数を渡す場合は，引用符で囲む必要があります．

## 可変調引数
ユーザから不特定多数の引数を受け取りたい場合，可変長引数を使用できます．例えば，以下のように定義します:

```python
@bot.command()
async def echo(ctx, *args):
    response = ' '.join(args)
    await ctx.send(response)
```
ユーザが` !echo Hello, world! `というコマンドを実行すると，` Hello, world! `というメッセージが送信されます．

## キーワード専用引数
キーワード専用引数は，引数にデフォルト値を設定できる引数です．例えば，以下のように定義します:

```python
@bot.command()
async def repeat(ctx, times: int = 3, content='repeating...'):
    for i in range(times):
        await ctx.send(content)
```
ユーザが` !repeat `というコマンドを実行すると，` repeating... `というメッセージが3回送信されます．また，` !repeat 5 Hello! `というコマンドを実行すると，` Hello! `というメッセージが5回送信されます．

## コンバータ
` discord.py `のコマンド拡張は，ユーザからの入力を特定の形に変換するためのコンバータを提供しています．これにより，入力の検証や変換を簡単に行うことができます．
例えば，引数を整数として受け取りたい場合，関数の引数に型ヒントを指定します:

```python
@bot.command()
async def add(ctx, a: int, b: int):
    await ctx.send(a + b)
```
ユーザが` !add 3 5 `というコマンドを実行すると，` 8 `というメッセージが送信されます．もし，ユーザが整数以外の値を入力した場合，ボットは自動的にエラーを検知し，適切なエラーメッセージを送信します．

さらに，` discord.py `は特定のDiscordオブジェクト（ユーザ，チャンネル，ロールなど）への変換もサポートしています．例えば，ユーザをメンションで指定し，そのユーザオブジェクトを取得することができます:

```python
@bot.command()
async def info(ctx, user: discord.Member):
    await ctx.send(f'Hello, {user.display_name}!')
```
ユーザが` !info @Alice `というコマンドを実行すると，` Hello, Alice! `というメッセージが送信されます．

# まとめ
この記事では，` discord.py `のコマンド拡張機能を使ったコマンドの定義方法について解説しました．コマンド拡張機能を使うことで，ボットの機能を柔軟かつ強力に拡張することができます．

コマンドの定義方法，サブコマンドの定義方法，パラメータの取り扱い方法について理解し，自分のボットに応じたコマンドを定義してみてください．

### 参考文献
[discord.py Documentation - Commands](https://discordpy.readthedocs.io/en/stable/ext/commands/commands.html)