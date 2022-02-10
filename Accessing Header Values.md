```py
from fastapi import FastAPI,Header
from typing import Optional



app = FastAPI()

@app.get('/header')
async def read_header(random_header: Optional[str] = Header(None)):
    return {'rand_header': random_header}
```
