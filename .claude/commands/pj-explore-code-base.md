---
allowed-tools: Bash(ls:*), Bash(find:*), Bash(grep:*), Bash(tree:*), Bash(rg:*), Fetch(*)
description: "引数で指定した数のサブエージェントでコードベースを探索する"
---

指示:
このコードベースを入力のサブエージェントで並列に探索してください

条件:
入力がない場合はサブエージェントは利用しない

入力:
$ARGUMENTS