explain result について

## 内容
https://docs.mongodb.com/manual/reference/explain-results/#explain.queryPlanner.winningPlan.inputStages

の内容理解。

- 2019/04/30 

explain.executionStats.executionStagesに書かれている内容。
精読したが理解が３0％程度。必要になったら理解に務める。

>executionStages.works:
>Specifies the number of “work units” performed by the query execution stage. 

stage実行による`work units`の数

>executionStages.advanced
>The number of intermediate results returned, or advanced, by this stage to its parent stage.

親ステージに返却された中間時点のstage返却数

>executionStages.needTime
>The number of work cycles that did not advance an intermediate result to its parent stage

親ステージに進むことができなかった中間結果の`work cycles`の数

>executionStages.needYield ?
>The number of times that the storage layer requested that the query stage suspend processing and yield its locks.

stageが中断しロックを開放をストレージ層が要求した数

>executionStages.saveState ?
>The number of times that the query stage suspended processing and saved its current execution state, for example in preparation for yielding its locks.

stageが保存した処理を中断し保存した回数。例えばロック開放するための準備など。

>executionStages.restoreState
>The number of times that the query stage restored a saved execution state, for example after recovering locks that it had previously yielded.

stageが保存した実行状態を保存したクエリ状態を復元した回数。例えば以前に開放したロックを回復した後など。


>executionStages.isEOF
>Specifies whether the execution stage has reached end of stream:

stageがストリームの終わりに達したかどうか

>executionStages.inputStage.keysExamined
>For query execution stages that scan an index (e.g. IXSCAN), keysExamined is the total number of in-bounds and out-of-bounds keys that are examined in the process of the index scan. 

インデックスを利用し走査した件数。(インバウンド/アウトバウンドキーの総数)


>inputStage.docsExamined
>Specifies the number of documents scanned during the query execution stage.

stageが走査したドキュメント数

>executionStages.inputStage.seeks
>The number of times that we had to seek the index cursor to a new position in order to complete the index scan.

インデックススキャン完了のためシーク位置を変更した回数


