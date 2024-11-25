# 多跳任务的评测集构建
1. 首先使用跟nano-graphrag一致的split逻辑对指定路径（input_dir）下所有txt文档进行chunk
    ```bash
    cd graphrag-project/graphrag/nano-graphrag/generate-data
    sh split_txt.sh
    ```
    
2. 构建多跳评测集pipeline[pipeline](../image/multihop.png)（参考[MultiHop](https://arxiv.org/abs/2401.15391)论文）
    ```bash
    sh gen_data_multihop.sh
    ```
    > 得到两个json文件，一个是得到的最终的多跳评测数据，另一个是分组后的结果（pipeline中的一个流程）

3. input_dir路径下的txt文本执行insert函数，并将输出保存至WORKING_DIR。对于每个问题执行query函数得到graphrag的回答（其中有两种mode，local-mode的表现优于global-mode，建议使用local-mode）

4. 评估graphrag回答的表现，并保存回答错误的样本以便后续进一步展开对应的优化工作
    ```bash
    sh eval.sh
    ```
    > 其中multihop要设置为True，我们使用LLM as a Judge技术得到评测集的准确率，评测模型是Qwen2.5-7B-Instruct-GPTQ-Int8
