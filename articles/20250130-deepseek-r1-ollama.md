---
title: "[DeepSeek R1]ローカルで簡単に⁉︎Open WebUIとOllamaを使った環境構築手順"
emoji: "🧠"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: [ollama]
published: false
---

# はじめに
DeepSeek R1は，中国のAIスタートアップであるDeepSeekが開発した高度な大規模言語モデルです．このモデルは，数学，コード生成，自然言語推論などの複雑なタスクにおいて優れた性能を発揮し，OpenAIの最新モデル（2025年1月時点）と比較しても遜色ない能力を持っています．
さらに，DeepSeek R1はMITライセンスの下でオープンソースとして公開されており，誰でも自由に利用・改良・商用化が可能です．
本記事では，DeepSeek R1を[Open WebUI](https://github.com/open-webui/open-webui)と[Ollama](https://ollama.com/)を用いてローカル環境で使用するための手順を解説します．

https://github.com/open-webui/open-webui
https://ollama.com/

# 前提条件
DeepSeek R1の小型バージョン（1.5Bパラメータ）を使用する場合でも，最低でも8GB程度のメモリは必要らしいです．
筆者はメモリ256GB，VRAM144GBの環境で実行しています．

# Ollamaのインストール
[Ollamaの公式サイト](https://ollama.com/)からインストーラをダウンロードし，実行する．もしくは，以下のコマンドを実行することでインストールを行えます：
```bash
curl -fsSL https://ollama.com/install.sh | sh
```
インストールが完了したら，以下のコマンドで正しくインストールが行われているか確認してください．
```bash
ollama --version
```

# DeepSeek R1のダウンロード
` ollama run `コマンドを使ってDeepSeek R1を起動します．起動に際して，モデルがダウンロードされます．
```bash
ollama run deepseek-r1:1.5B
```
ここでは，1.5BパラメータのDeepSeek R1をダウンロードしています．より大きなモデルを使用する場合は，` deepseek-r1:14B `などのように指定してください．
2回目以降の起動時には，モデルのダウンロードは行われず，直ちに起動されます．
ここまでで，DeepSeek R1をCLIで使用することが可能になりました．一度，適当なプロンプトを送ってみてください．正しく起動していれば，応答が返ってくるはずです．

![DeepSeek R1 CLI起動イメージ](/images/20250130-deepseek-r1-ollama/cli.png)
