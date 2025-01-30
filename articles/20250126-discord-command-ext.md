---
title: "[Discord.py] Discord Botのコマンドを簡単に定義！コマンド拡張機能の使い方"
emoji: "🐍"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: [python, discord, discord.py]
published: false
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