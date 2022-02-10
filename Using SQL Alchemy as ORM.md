1. install alchemy
```
pip install sqlalchemy
```
2. Create database.py
```py
from sqlalchemy import create_engine
from sqlalchemy.orm import sessionmaker
from sqlalchemy.ext.declarative import declarative_base

SQLALCHEMY_DATABASE_URL = 'sqlite:///./satellite.db'

engine = create_engine(
    SQLALCHEMY_DATABASE_URL,
    connect_args={'check_same_thread': False}
)

SessionLocal = sessionmaker(autocommit=False, autoflush=False, bind=engine)

Base = declarative_base()
```
3. Create models.py
```py
from sqlalchemy import Column, Integer, String, Float
from database import Base


class Satellite(Base):
    __tablename__ = "Satellites"

    id = Column(Integer, primary_key=True, index=True, autoincrement=True)
    name = Column(String)
    location = Column(String)
    miles_from_earth = Column(Float)
```
4. Scaffold tables in db using main.py, running command <uvicorn main:app --reload>
```py
import models
from fastapi import FastAPI, Depends
from database import engine, SessionLocal
from sqlalchemy.orm import Session

app = FastAPI()

models.Base.metadata.create_all(bind=engine)
```
5. Use FASTAPI dependincies to ensure get_db func is ran before the function is executed
```py
import models
from fastapi import FastAPI, Depends
from database import engine, SessionLocal
from sqlalchemy.orm import Session

app = FastAPI()



def get_db():
    try:
        db = SessionLocal()
        yield db
    finally:
        db.close()


@app.get('/')
async def get_satellites(db: Session = Depends(get_db)):
    return db.query(models.Satellite).all()

```
