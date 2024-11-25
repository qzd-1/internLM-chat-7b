# 常见任务的评测集构建
1. 首先使用跟nano-graphrag一样的split逻辑对指定路径（input_dir）下所有txt文档进行chunk
    ```bash
    cd graphrag-project/graphrag/nano-graphrag/generate-data
    sh split_txt.sh
    ```

2. 构建评测集（三种不同的任务：简单事实类、推理类、全文理解类）
    ```bash
    sh gen_data.sh
    ```
    构建question的同时会得到answer
3. input_dir路径下的txt文本执行insert函数，并将输出保存至WORKING_DIR。对于每个问题执行query函数得到graphrag的回答（其中有两种mode，local-mode的表现优于global-mode，建议使用local-mode）

4. 评估graphrag回答的表现，并保存回答错误的样本以便后续进一步展开对应的优化工作
    ```bash
    sh eval.sh
    ```
    我们使用LLM as a Judge技术得到评测集的准确率，其中使用的大模型api跟graphrag是一样的。
