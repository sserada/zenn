---
title: "[DeepSeek R1]ローカルで簡単に！Open WebUIとOllamaを使った環境構築手順"
emoji: "🧠"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: [ollama, DeepSeek, Open WebUI, LLM]
published: true
---

# はじめに
[DeepSeek R1](https://github.com/deepseek-ai/DeepSeek-R1)は，中国のAIスタートアップであるDeepSeekが開発した高度な大規模言語モデルです．このモデルは，数学，コード生成，自然言語推論などの複雑なタスクにおいて優れた性能を発揮し，OpenAIの最新モデル（2025年1月時点）と比較しても遜色ない能力を持っています．
さらに，DeepSeek R1はMITライセンスの下でオープンソースとして公開されており，誰でも自由に利用・改良・商用化が可能です．
本記事では，DeepSeek R1を[Open WebUI](https://github.com/open-webui/open-webui)と[Ollama](https://ollama.com/)を用いてローカル環境で使用するための手順を解説します．

https://github.com/open-webui/open-webui
https://ollama.com/

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

# Open WebUIのインストール
Open WebUIは，Webブラウザ上で拡張性，機能の豊富性が優れた完全にオフラインで動作するように設計されたセルフホスト型AIプラットフォームです．
以下のコマンドを実行して，Open WebUIをインストールします：
※Python 3.11以降が必要です．
```bash
pip install open-webui
```
インストールが完了したら，以下のコマンドで正しくインストールが行われているか確認してください．
```bash
open-webui --version
```

# Open WebUIの起動
以下のコマンドを実行して，Open WebUIを起動します：
```bash
open-webui serve
```
起動時の出力の中に` Uvicorn running on http://~~~~ (Press CTRL+C to quit) `のような文章があると思いますので，そのURLにブラウザでアクセスしてください（デフォルトでは` http://localhost:8080 `）．
Open WebUIの初期設定画面が表示されていれば，正しく起動しています．
初期設定完了後のチャット画面で，左上からモデルを選択することができます．その中に先ほどダウンロードしたDeepSeek R1のモデルがあれば完了です．選択して，チャットを送信してみてください．
![Open WebUI モデル選択画面](/images/20250130-deepseek-r1-ollama/select.png)

# まとめ
本記事では，DeepSeek R1をOpen WebUIとOllamaを用いてローカル環境で使用するための手順を解説しました．ぜひ本記事の手順を参考に，ローカル環境での高度なAIモデルの活用に挑戦してみてください．

# 参考文献
- [GitHub - DeepSeek R1](https://github.com/deepseek-ai/DeepSeek-R1)
- [GitHub - Open WebUI](https://github.com/open-webui/open-webui)
- [Ollama](https://ollama.com/)