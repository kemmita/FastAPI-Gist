1. Get no args
```py
from fastapi import FastAPI

app = FastAPI()

arr = {
    "name":"Russell",
     "age":31, 
     "languages": 
     [
         "C#",
         "Python"
     ]
}

@app.get("/")
async def hello_world():
    # This dict will be retruned as a json response
    return arr
```
2. Get with args
```py
from fastapi import FastAPI

app = FastAPI()

arr = {
    "name":"Russell",
     "age":31, 
     "languages": 
     [
         "C#",
         "Python"
     ]
}

@app.get("/component/{name}")
async def get_component(name):
    return [x for x in arr if x['name'] == 'Kara']
```
3. Get with query param
```py
from fastapi import FastAPI

app = FastAPI()

@app.get("/component/")
async def read_component(number):
    return{"number":number}
```
4. Post
```py
from fastapi import FastAPI
from pydantic import BaseModel

class Employee(BaseModel):
    name: str
    age: int


app = FastAPI()


@app.post('/employee/add')
async def post_employee(employee:Employee):
    return employee
```
