```python
class Vocab:
    def __init__(self,tokens = None):
        # 使用列表存储所有的标记，从而可根据索引值获取相应的标记
        self.idx_to_token = list()
        # 使用字典实现标记到索引值的映射
        self.token_to_idx = dict()

        if tokens is not None:
            if "<unk>" not in tokens:
                tokens = tokens + ["<unk>"]
            for token in tokens:
                self.idx_to_token.append(token)
                self.token_to_idx[token] = len(self.idx_to_token) - 1

            self.unk = self.token_to_idx["<unk>"]

    # 创建词表，text包含若干句子，每个句子由若干标记构成
    @classmethod
    def build(cls,text,min_freq = 1 , reserved_tokens = None):
        # 存储标记及其出现次数的映射字典
        token_freqs = defaultdict(int)
        # 无重复地进行标记
        for token in text:
            token_freqs[token] += 1

        # 用户自定义的预留标记
        uniq_tokens = ["<unk>"] + (reserved_tokens if reserved_tokens else [])
        uniq_tokens += [token for token,freq in token_freqs.items() if freq >= min_freq and token != "<unk>"]

        return cls(uniq_tokens)

    # 返回词表的大小，即词表中有多少个互不相同的标记
    def __len__(self):
        return len(self.idx_to_token)

    # 查找输入标记对应的索引值
    def __getitem__(self, token):
        return self.token_to_idx.get(token,self.unk)

    # 查找一系列输入标记对应的索引值
    def convert_tokens_to_ids(self,tokens):
        return [self[token.lower()] for token in tokens]

    # 查找一系列索引值对应的标记
    def convert_ids_to_tokens(self,indices):
        return [self.idx_to_token[index] for index in indices]
```
