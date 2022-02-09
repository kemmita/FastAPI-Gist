1. Below is the default Exception handling we can use.
```py
import uuid
from uuid import UUID
from fastapi import FastAPI, HTTPException, Request
from pydantic import BaseModel, Field
from typing import List
from starlette.responses import JSONResponse


class Satellite(BaseModel):
    id: UUID
    name: str = Field(min_length=1)
    location: str = Field(min_length=3, max_length=100)
    mass: float


app = FastAPI()

Satellites: List[Satellite] = [
    Satellite(id=uuid.uuid1(), name='Harald', location='Burham', mass=2356.76)
]


@app.get('/satellites/{id}')
async def get_satellite(id: UUID):
    for i in Satellites:
        if i.id == id:
            return i
    raise HTTPException(status_code=404, detail="Satellite not found.")
```

2. Now we will create a custom HttpException
```py
import uuid
from uuid import UUID
from fastapi import FastAPI, HTTPException, Request
from pydantic import BaseModel, Field
from typing import List
from starlette.responses import JSONResponse


class NegativeNumberException(Exception):
    def __init__(self, satellites_to_return):
        self.satellites_to_return = satellites_to_return


class Satellite(BaseModel):
    id: UUID
    name: str = Field(min_length=1)
    location: str = Field(min_length=3, max_length=100)
    mass: float


app = FastAPI()

Satellites: List[Satellite] = [
    Satellite(id=uuid.uuid1(), name='Harald', location='Burham', mass=2356.76)
]


@app.exception_handler(NegativeNumberException)
async def negative_number_exception_handler(request: Request, exception: NegativeNumberException):
    return JSONResponse(
        status_code=418,
        content={'message': f"invalid mass value {exception.satellites_to_return}"}
    )


@app.post('/satellite/create')
async def create_satellite(satellite: Satellite):
    if satellite.mass < 0:
        raise NegativeNumberException(satellite)
    Satellites.append(satellite)
    return Satellites

```
