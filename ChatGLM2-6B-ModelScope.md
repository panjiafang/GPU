## 安装ModelScope

```bash
pip install modelscope
```

## 安装依赖

```bash
pip install transformers, accelerate, sentencepiece
```

## 创建Python文件chatglm.py

```python
# 备注：最新模型版本要求modelscope >= 1.7.2
# pip install modelscope -U 

from modelscope.utils.constant import Tasks
from modelscope import Model
from modelscope.pipelines import pipeline
model = Model.from_pretrained('ZhipuAI/chatglm2-6b', device_map='auto', revision='v1.0.7')
pipe = pipeline(task=Tasks.chat, model=model)
inputs = {'text':'你好', 'history': []}
result = pipe(inputs)
inputs = {'text':'介绍下清华大学', 'history': result['history']}
result = pipe(inputs)
print(result)
```

## 运行

```bash
python chatglm.py
```