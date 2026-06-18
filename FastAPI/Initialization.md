```
uv init .
uv add fastapi
uv add python-dotenv
uv add fastapi-users[sqlalchemy]
uv add imagekitio
uv add uvicorn[standard]
uv run main.py
```

- Uvicorn is a web server in Python that allows us to server our fast API 
- Ensure all project dependencies are listed in a `requirements.txt` file (generated via `pip freeze > requirements.txt`) so the deployment environment can install them automatically.