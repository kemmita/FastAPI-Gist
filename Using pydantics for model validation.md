1. Use pydantics fields to ensure model field integrity
```py
from uuid import UUID
from fastapi import FastAPI
from pydantic import BaseModel, Field


class Satellite(BaseModel):
    id: UUID
    name: str = Field(min_length=1)
    location: str = Field(min_length=3, max_length=100)
    mass: float = Field(gt=0.99)


app = FastAPI()

Satellites: list = []


@app.post('/satellite/create')
async def create_satellite(satellite: Satellite):
    Satellites.append(satellite)
    return Satellites
```
