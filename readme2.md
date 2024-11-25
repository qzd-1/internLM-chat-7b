## 快速开始

> [!TIP]
>
> **Please set OpenAI API key in environment: `export OPENAI_API_KEY="sk-..."`.** 

> [!TIP]
> If you're using Azure OpenAI API, refer to the [.env.example](./.env.example.azure) to set your azure openai. Then pass `GraphRAG(...,using_azure_openai=True,...)` to enable.


下载查尔斯·狄更斯的《圣诞颂歌》副本：

```shell
curl https://raw.githubusercontent.com/gusye1234/nano-graphrag/main/tests/mock_data.txt > ./book.txt
```

使用下面的 python 代码片段：

```python
from nano_graphrag import GraphRAG, QueryParam

graph_func = GraphRAG(working_dir="./dickens")

with open("./book.txt") as f:
    graph_func.insert(f.read())

# Perform global graphrag search
print(graph_func.query("What are the top themes in this story?"))

# Perform local graphrag search (I think is better and more scalable one)
print(graph_func.query("What are the top themes in this story?", param=QueryParam(mode="local")))
```

下次您从同一 `working_dir` 初始化 `GraphRAG` 时，它将自动重新加载所有上下文。

#### 批量插入

```python
graph_func.insert(["TEXT1", "TEXT2",...])
```

<details>
<summary> 增量插入</summary>

`nano-graphrag` 支持增量插入，不会添加重复计算或数据：

```python
with open("./book.txt") as f:
    book = f.read()
    half_len = len(book) // 2
    graph_func.insert(book[:half_len])
    graph_func.insert(book[half_len:])
```

> `nano-graphrag` 使用内容的md5哈希作为密钥，因此不存在重复的块。
>
> 但是，每次插入时，都会重新计算图的社区并重新生成社区报告

</details>

<details>
<summary> 简单的 RAG</summary>

`nano-graphrag` 同样是支持简单的 RAG 插入和查询：

```python
graph_func = GraphRAG(working_dir="./dickens", enable_naive_rag=True)
...
# Query
print(rag.query(
      "What are the top themes in this story?",
      param=QueryParam(mode="naive")
)
```
</details>


### 异步

对于每个方法 `NAME(...)` , 都有一个相应的异步方法 `aNAME(...)`

```python
await graph_func.ainsert(...)
await graph_func.aquery(...)
...
```

### 可用参数

`GraphRAG` 和 `QueryParam` 是 Python 中的 `dataclass`。使用 `help(GraphRAG)` 和 `help(QueryParam)` 查看所有可用参数！或者查看 [Advances](#Advances) 部分以查看一些选项。


## 可自定义的组件

以下是您可以使用的组件：

| Type            |                             What                             |                       Where                       |
| :-------------- | :----------------------------------------------------------: | :-----------------------------------------------: |
| LLM             |                      Qwen `llama-factory`                    |   Built-in(default/[doc](./docs/llm_deploy.md))   |
|                 |                            OpenAI                            |                     Built-in                      |
|                 |                           DeepSeek                           |              [examples](./examples)               |
| Embedding       |                Bce-base(Sentence-transformers)               |                 Built-in(default)                 |
|                 |                            OpenAI                            |                     Built-in                      |
| Vector DataBase |  [`milvus-lite`](https://github.com/milvus-io/milvus-lite)   |                 Built-in(default)                 |
|                 |        [`hnswlib`](https://github.com/nmslib/hnswlib)        |         Built-in, [examples](./examples)          |
|                 | [faiss](https://github.com/facebookresearch/faiss?tab=readme-ov-file) |              [examples](./examples)               |
| Graph Storage   |       [`nebula`](https://github.com/vesoft-inc/nebula)       | Built-in(default/[doc](./docs/use_nebula_for_graphrag.md)) |
|                 | [`networkx`](https://networkx.org/documentation/stable/index.html) |                     Built-in                      |
|                 |                [`neo4j`](https://neo4j.com/)                 | Built-in([doc](./docs/use_neo4j_for_graphrag.md)) |
| Chunking        |                        by token size                         |                     Built-in                      |
|                 |                       by text splitter                       |                     Built-in                      |
|                 |                by recursivecharacter splitter                |                     Built-in                      |

- `Built-in` 代表内部是有实现的。 `examples` 代表我们是在 [examples](./examples) 文件夹下面实现。

## 进阶


<details>
<summary>只查询相关上下文</summary>

`graph_func.query` 返回最终答案而不进行流式传输。 

如果您想在项目中交互 `nano-graphrag` ，您可以使用 `param=QueryParam(..., only_need_context=True,...)`，它只会返回从图中检索到的上下文，例如：

````
# Local mode
-----Reports-----
```csv
id,	content
0,	# FOX News and Key Figures in Media and Politics...
1, ...
```
...

# Global mode
----Analyst 3----
Importance Score: 100
Donald J. Trump: Frequently discussed in relation to his political activities...
...
````

您可以将该上下文集成到您的自定义提示中。

</details>

<details>
<summary>Prompt</summary>

所有prompts都存放在`nano_graphrag.prompt.PROMPTS`字典对象中，可以自定义。

一些重要prompts:

- `PROMPTS["entity_extraction"]` 用于从文本块中提取实体和关系。
- `PROMPTS["community_report"]` 用于组织和总结图集群的描述。
- `PROMPTS["local_rag_response"]` 是本地搜​​索生成的系统提示模板。
- `PROMPTS["global_reduce_rag_response"]` 是全局搜索生成的系统提示模板。
- `PROMPTS["fail_response"]` 是当没有任何内容与用户查询相关时的后备响应。

</details>

<details>
<summary>大模型函数</summary>

目前总共用到了两种不同的llm模型，一个是用于入库的模型（`Qwen2.5-7B-Instruct-GPTQ-Int8`），一个是用于查询的模型（`zhiyan-Int8`）。

您可以在脚本中设置 `api_key`、`api_base`、`MODEL` 参数使用自定义的模型。

```python
MODEL = "zhiyan-v2.6-chat-int8"
api_key = 'Zhiyan123'
base_url="http://192.168.200.211:8100/v1"

MODEL_QWEN = "QWEN"
api_key_qwen = '0'
base_url_qwen="http://127.0.0.77:8012/v1/"

async def qwen2_complete(
    prompt, system_prompt=None, history_messages=[], **kwargs
) -> str:
    return await openai_complete_if_cache(
        MODEL_QWEN,
        prompt,
        api_key_qwen, 
        base_url_qwen,
        system_prompt=system_prompt,
        history_messages=history_messages,
        **kwargs,
    )

async def zhiyan_complete(
    prompt, system_prompt=None, history_messages=[], **kwargs
) -> str:
    return await openai_complete_if_cache(
        MODEL,
        prompt,
        api_key, 
        base_url,
        system_prompt=system_prompt,
        history_messages=history_messages,
        **kwargs,
    )
```

用自定义llm替换默认的gpt-4o模型:

```python
# Adjust the max token size or the max async requests if needed
def insert():
  GraphRAG(best_model_func=qwen2_complete, cheap_model_func=qwen2_complete, best_model_max_token_size=..., best_model_max_async=...)

def query():
  GraphRAG(best_model_func=zhiyan_complete, cheap_model_func=zhiyan_complete, best_model_max_token_size=..., best_model_max_async=...)
```

#### Json格式输出

除了gpt-4o等闭源模型能够很好的输出json格式，大部分开源模型不能稳定的输出json格式。本项目在`convert_response_to_json`中借助 [json_repair](https://github.com/mangiucugna/json_repair)` 库来修复llm返回的json字符串

</details>



<details>
<summary>嵌入函数</summary>

您可以将默认嵌入函数替换为任何 `_utils.EmbedddingFunc` 实例.

例如借助[sentencetransfomer](https://www.sbert.net/)库使用开源的网易 `bce-base_v1` 模型:

```python
EMBED_MODEL_PATH = "/data/tangbo/plms/bce-embedding-base_v1"

EMBED_MODEL = SentenceTransformer(
    EMBED_MODEL_PATH, cache_folder=WORKING_DIR, device="cpu"
)

@wrap_embedding_func_with_attrs(
    embedding_dim=EMBED_MODEL.get_sentence_embedding_dimension(),
    max_token_size=EMBED_MODEL.max_seq_length,
)
async def local_embedding(texts: list[str]) -> np.ndarray:
    return EMBED_MODEL.encode(texts, normalize_embeddings=True)
```

替换默认的向量模型函数:

```python
GraphRAG(embedding_func=local_embedding, embedding_batch_num=..., embedding_func_max_async=...)
```

You can refer to an [example](./examples/using_local_embedding_model.py) that use `sentence-transformer` to locally compute embeddings.
</details>


<details>
<summary>存储组件</summary>

你可以自定义所有跟存储相关的组件, 主要使用了三种存储组件:

**`base.BaseKVStorage` 用于存储数据键值对** 

- 默认情况下，我们使用磁盘文件存储作为后端
- `GraphRAG(.., key_string_value_json_storage_cls=YOURS,...)`

**`base.BaseVectorStorage` 用于索引嵌入**

- 默认使用 [`milvus-lite`](https://github.com/milvus-io/milvus-lite) 作为后端（不支持windows）。
- `GraphRAG(.., vector_db_storage_cls=YOURS,...)`

**`base.BaseGraphStorage` 用于存储知识图谱**

- 默认使用 [`nebula`](https://github.com/vesoft-inc/nebula) 作为后端，完整流程请查看[教程](./docs/use_nebula_for_graphrag.md)。
- `GraphRAG(.., graph_storage_cls=YOURS,...)`

您可以参考 `nano_graphrag.base` 查看各个组件的详细接口.
</details>



## 评测

- [常见任务的评测教程](./docs/eval_for_infer_global_sf.md)。
- [多跳任务的评测教程](./docs/eval_for_multihop.md)。

