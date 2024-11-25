## 安装 LLaMA Factory

> [!IMPORTANT]
> 此步骤为必需。

```bash
git clone --depth 1 https://github.com/hiyouga/LLaMA-Factory.git
cd LLaMA-Factory
pip install -e ".[torch,metrics]"
```
## api
如果您需要使用 api 进行批量推理，您只需指定模型、适配器（可选）、模板、微调方式等信息。

下面是一个配置文件的示例：
```yaml
model_name_or_path: /data1/graphrag_project/plms/llm_model/Qwen2.5-7B-Instruct-GPTQ-Int8
template: qwen
```
#### vllm推理框架
若使用vllm推理框架，请在配置中指定： `infer_backend` 与 `vllm_enforce_eager`。
```yaml
model_name_or_path: /data1/graphrag_project/plms/llm_model/Qwen2.5-7B-Instruct-GPTQ-Int8
template: qwen
infer_backend: vllm
vllm_enforce_eager: true
vllm_maxlen: 32768
```

下面是一个启动并调用 api 服务的示例：

首先进入到LLaMA-Factory文件夹下面，您可以使用 `API_PORT=8000 CUDA_VISIBLE_DEVICES=0 llamafactory-cli api examples/inference/qwen2.5_vllm.yaml` 启动 api 服务并运行以下示例程序进行调用：
```python
# api_call_example.py
from openai import OpenAI
client = OpenAI(api_key="0",base_url="http://0.0.0.0:8000/v1")
messages = [{"role": "user", "content": "Who are you?"}]
result = client.chat.completions.create(messages=messages, model="meta-llama/Meta-Llama-3-8B-Instruct")
print(result.choices[0].message)
```
