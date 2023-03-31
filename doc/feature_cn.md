# 🥳 功能

[English](feature.md) | 中文

- 支持openai普通和流式的聊天请求
- 支持top_k搜索，可以在DataManager创建时进行设置
- 支持多级缓存, 参考: `Cache#next_cache`

```python
bak_cache = Cache()
bak_cache.init()
cache.init(next_cache=bak_cache)
```

- 是否跳过当前缓存，对于请求不进行缓存搜索也不保存chat gpt返回的结果，参考： `Cache#cache_enable_func`
- 缓存系统初始化阶段，不进行缓存搜索，但是保存chat gpt返回的结果，参考： 使用`create`方法时设置`cache_skip=True`参数

```python
openai.ChatCompletion.create(
    model="gpt-3.5-turbo",
    messages=mock_messages,
    cache_skip=True,
)
```

- 像积木一样，所有模块均可自定义，包括：
  - pre-embedding，获取原始请求中的特征信息，如最后一条消息，prompt等
  - embedding，将特征信息转换成向量数据
  - data manager，缓存数据管理，主要包括数据搜索和保存
  - cache similarity evaluation，可以使用相似搜索的距离或者其他更适合使用场景的模型
  - post-process，处理缓存答案列表，比如最相似的，随机或者自定义