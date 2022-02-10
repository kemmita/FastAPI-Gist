1. Define routes to the controllers
```py
from fastapi import FastAPI
from controllers import planet_controller, satellite_controller

app = FastAPI()

app.include_router(
    prefix='/planets',
    tags=['planets'],
    router=planet_controller.api
)
app.include_router(
    prefix='/satellites',
    tags=['satellites'],
    router=satellite_controller.api
)
```
2. Now create your controllers
```py
# Planies controller file
from fastapi import APIRouter

api = APIRouter()


@api.get('/')
def get_planets():
    return 'all planets'
    
# satellite controller file
from fastapi import APIRouter

api = APIRouter()


@api.get('/')
def get_satellites():
    return 'all satellites'
```
