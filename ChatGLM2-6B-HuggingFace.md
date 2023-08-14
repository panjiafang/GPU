# HuggingFace调用ChatGLM2-6B模型

ChatGLM2-6B在Github官方文档是通过HuggingFace调用的。[文档地址](https://github.com/THUDM/ChatGLM2-6B/tree/main)

> 因为在国内，我的网络不太好，这里说一下我的操作步骤

1. 下载HuggingFace调用库

```bash
git clone https://huggingface.co/THUDM/chatglm2-6b
```
因为模型文件太大，使用了Git LFS。我就不安装了，想办法绕过去

使用[克隆仓库](https://github.com/panjiafang/chatglm2-6b-huggingface)这里把LFS相关文件全部删除了。

```bash
git clone https://github.com/panjiafang/chatglm2-6b-huggingface chatglm2-6b/
```

> 这里保存到了`chatglm2-6b`文件夹下

2. 本地下载模型文件, 将模型下载在`chatglm2-6b`文件夹

[下载地址](https://cloud.tsinghua.edu.cn/d/674208019e314311ab5c/)

以下是临时地址，不保证可用

```bash
curl https://cloud.tsinghua.edu.cn/seafhttp/files/7cc3cd6f-f02c-49b2-a373-efd42f446746/pytorch_model-00001-of-00007.bin >pytorch_model-00001-of-00007.bin

curl https://cloud.tsinghua.edu.cn/seafhttp/files/7dec58ee-b32d-4146-96fb-8f067d8643ff/pytorch_model-00002-of-00007.bin >pytorch_model-00002-of-00007.bin

curl https://cloud.tsinghua.edu.cn/seafhttp/files/980fedfc-2160-4614-873f-10860f4aa942/pytorch_model-00003-of-00007.bin >pytorch_model-00003-of-00007.bin

curl https://cloud.tsinghua.edu.cn/seafhttp/files/625dd7a8-aa3d-48df-9352-d194117234ae/pytorch_model-00004-of-00007.bin >pytorch_model-00004-of-00007.bin

curl https://cloud.tsinghua.edu.cn/seafhttp/files/334ed81e-9f7b-4978-918d-8338aed966f2/pytorch_model-00005-of-00007.bin >pytorch_model-00005-of-00007.bin

curl https://cloud.tsinghua.edu.cn/seafhttp/files/66c98716-74e4-4b33-b2f5-6dd3e2791baf/pytorch_model-00006-of-00007.bin >pytorch_model-00006-of-00007.bin

curl https://cloud.tsinghua.edu.cn/seafhttp/files/ca1a6b5a-cde9-42e5-9d2a-0ca5188428e8/pytorch_model-00007-of-00007.bin >pytorch_model-00007-of-00007.bin

curl https://cloud.tsinghua.edu.cn/seafhttp/files/29763829-55d2-4bdb-b7ad-0c4482b4416a/tokenizer.model >tokenizer.model
``` 

3. 测试

新建`api`文件夹，与`chatglm2-6b`同级，从ChatGLM2-6B的官方文档中下载代码`requirements.txt`和`openai_api.py`，[下载地址](https://github.com/THUDM/ChatGLM2-6B/tree/main)

4. 修改`openai_api.py`文件

```python
tokenizer = AutoTokenizer.from_pretrained("THUDM/chatglm2-6b", trust_remote_code=True)
# "THUDM/chatglm2-6b" -> "../chatglm2-6b"
model = AutoModel.from_pretrained("THUDM/chatglm2-6b", trust_remote_code=True).cuda()
# "THUDM/chatglm2-6b" -> "../chatglm2-6b"

uvicorn.run(app, host='0.0.0.0', port=8000, workers=1)
# 这里8000端口，我改成8080
```

> 因为使用本地模型文件，所以需要修改为模型的相对路径`../chatglm2-6b`

5. 运行

```bash
python openai_api.py
```

6. 接口测试

POST http://xx.xx.xx.xx:8080/v1/chat/completions

Body:
```json
{
    "model":"chatglm2-6b",
    "messages": [{
        "role": "user",
        "content": "你好"
    }]
}
```
