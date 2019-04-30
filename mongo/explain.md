explain result について

## 内容
https://docs.mongodb.com/manual/reference/explain-results/#explain.queryPlanner.winningPlan.inputStages

の内容理解。

- 2019/04/30 

explain.executionStats.executionStagesに書かれている内容。
精読したが理解が３0％程度。必要になったら理解に務める。

>executionStages.works:
>Specifies the number of “work units” performed by the query execution stage. 

>executionStages.advanced
>The number of intermediate results returned, or advanced, by this stage to its parent stage.

>executionStages.needTime
>The number of work cycles that did not advance an intermediate result to its parent stage

>executionStages.needYield ?
>The number of times that the storage layer requested that the query stage suspend processing and yield its locks.
>ストレージレイヤがyeild中断ロック

>executionStages.saveState ?
クエリが中断さつ現在の実行で保存された回数。例えばyieldingロック準備等で。
>The number of times that the query stage suspended processing and saved its current execution state, for example in preparation for yielding its locks.

>executionStages.restoreState
>The number of times that the query stage restored a saved execution state, for example after recovering locks that it had previously yielded.

>executionStages.isEOF
>Specifies whether the execution stage has reached end of stream:

>executionStages.inputStage.keysExamined
>For query execution stages that scan an index (e.g. IXSCAN), keysExamined is the total number of in-bounds and out-of-bounds keys that are examined in the process of the index scan. 

>inputStage.docsExamined
>Specifies the number of documents scanned during the query execution stage.

>executionStages.inputStage.seeks
>The number of times that we had to seek the index cursor to a new position in order to complete the index scan.
