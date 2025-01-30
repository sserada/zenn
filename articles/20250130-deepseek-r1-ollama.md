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
DeepSeek R1の小型バージョン（1.5Bパラメータ）を使用する場合でも，最低でも8GB程度のメモリは必要らしいです．大型モデルを動かす場合は，VRAM 84GB以上が推奨されています．
筆者はメモリ256GB，VRAM144GBと冗長な環境で実行しています．

# Ollamaのインストール

