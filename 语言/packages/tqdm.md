tqdm

```py
from tqdm import tqdm
tqdm(iterator)

# pandas
import pandas
df = pandas.read_csv(filepath, sep="\t", header=None, names=[])
for index, row in tqdm(df.iterrows(), total=df.shape[0]):
   print("index",index)
   print("row",row)
```

作用：是一个快速、扩展性强的进度条工具库

