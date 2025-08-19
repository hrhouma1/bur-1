# **Mini-service “Todo”** implémenté deux fois, pour comparer clairement :

* **Next.js (App Router)** : endpoints définis par dossiers/fichiers.
* **FastAPI** : endpoints définis par annotations Python.

Les deux versions sont **en mémoire** (pas de base de données), avec les mêmes endpoints :

* `GET /todos` : lister
* `POST /todos` : créer
* `PATCH /todos/:id` : modifier partiellement
* `DELETE /todos/:id` : supprimer



# Version A — Next.js 13+ (App Router, TypeScript)

## Arborescence

```
app/
  api/
    todos/
      route.ts         # /api/todos  (GET, POST)
    todos/
      [id]/
        route.ts       # /api/todos/:id  (PATCH, DELETE)
```

> Remarque : deux dossiers `todos` imbriqués peuvent prêter à confusion. Une alternative propre :

```
app/
  api/
    todos/
      route.ts        # /api/todos
    todos/[id]/
      route.ts        # /api/todos/:id
```

Ci-dessous, le code pour cette structure.

## `app/api/todos/route.ts`

```ts
import { NextRequest, NextResponse } from 'next/server';

type Todo = {
  id: string;
  title: string;
  done: boolean;
};

let todos: Todo[] = [
  { id: '1', title: 'First task', done: false },
];

export async function GET() {
  return NextResponse.json(todos);
}

export async function POST(req: NextRequest) {
  const body = await req.json().catch(() => null);
  if (!body || typeof body.title !== 'string') {
    return NextResponse.json({ error: 'Invalid payload: { title: string }' }, { status: 400 });
  }

  const todo: Todo = {
    id: String(Date.now()),
    title: body.title,
    done: !!body.done,
  };
  todos.push(todo);
  return NextResponse.json(todo, { status: 201 });
}
```

## `app/api/todos/[id]/route.ts`

```ts
import { NextRequest, NextResponse } from 'next/server';

type Todo = {
  id: string;
  title: string;
  done: boolean;
};

declare const todos: Todo[]; // partagé via module cache si importé depuis un util partagé
// Pour la simplicité de l'exemple Next.js, on peut factoriser l'état dans un module unique.
// Ex. créer src/lib/todos.ts pour stocker et importer la liste todos dans les deux fichiers.

export async function PATCH(_req: NextRequest, { params }: { params: { id: string } }) {
  const body = await _req.json().catch(() => null);
  if (!body || (body.title !== undefined && typeof body.title !== 'string') || (body.done !== undefined && typeof body.done !== 'boolean')) {
    return NextResponse.json({ error: 'Invalid payload: { title?: string; done?: boolean }' }, { status: 400 });
  }

  const idx = todos.findIndex(t => t.id === params.id);
  if (idx === -1) return NextResponse.json({ error: 'Not found' }, { status: 404 });

  todos[idx] = { ...todos[idx], ...body };
  return NextResponse.json(todos[idx]);
}

export async function DELETE(_req: NextRequest, { params }: { params: { id: string } }) {
  const idx = todos.findIndex(t => t.id === params.id);
  if (idx === -1) return NextResponse.json({ error: 'Not found' }, { status: 404 });

  const deleted = todos[idx];
  todos.splice(idx, 1);
  return NextResponse.json(deleted);
}
```

### Note importante pour Next.js

Pour que `todos` soit partagé entre `route.ts` collection et item, crée un module unique :

**`src/lib/todos.ts`**

```ts
export type Todo = { id: string; title: string; done: boolean };

export const db = {
  todos: [{ id: '1', title: 'First task', done: false }] as Todo[],
};
```

Puis importe-le dans les deux `route.ts` :

```ts
import { db } from '@/lib/todos';
const todos = db.todos;
```

### Test rapide (depuis la racine Next.js)

```bash
# GET
curl http://localhost:3000/api/todos

# POST
curl -X POST http://localhost:3000/api/todos \
  -H "Content-Type: application/json" \
  -d '{"title":"Write docs","done":false}'

# PATCH
curl -X PATCH http://localhost:3000/api/todos/1 \
  -H "Content-Type: application/json" \
  -d '{"done":true}'

# DELETE
curl -X DELETE http://localhost:3000/api/todos/1
```

---

# Version B — FastAPI (Python)

## Dépendances

```bash
pip install fastapi uvicorn
```

## `main.py`

```python
from fastapi import FastAPI, HTTPException
from pydantic import BaseModel
from typing import Optional, List

app = FastAPI()

class TodoCreate(BaseModel):
    title: str
    done: Optional[bool] = False

class TodoUpdate(BaseModel):
    title: Optional[str] = None
    done: Optional[bool] = None

class Todo(BaseModel):
    id: str
    title: str
    done: bool

# stockage en mémoire
todos: List[Todo] = [Todo(id="1", title="First task", done=False)]

@app.get("/todos", response_model=List[Todo])
def list_todos():
    return todos

@app.post("/todos", response_model=Todo, status_code=201)
def create_todo(payload: TodoCreate):
    new = Todo(id=str(len(todos) + 1), title=payload.title, done=bool(payload.done))
    todos.append(new)
    return new

@app.patch("/todos/{todo_id}", response_model=Todo)
def update_todo(todo_id: str, payload: TodoUpdate):
    for i, t in enumerate(todos):
        if t.id == todo_id:
            data = t.model_dump()
            if payload.title is not None:
                data["title"] = payload.title
            if payload.done is not None:
                data["done"] = payload.done
            updated = Todo(**data)
            todos[i] = updated
            return updated
    raise HTTPException(status_code=404, detail="Not found")

@app.delete("/todos/{todo_id}", response_model=Todo)
def delete_todo(todo_id: str):
    for i, t in enumerate(todos):
        if t.id == todo_id:
            deleted = todos.pop(i)
            return deleted
    raise HTTPException(status_code=404, detail="Not found")
```

## Lancer le serveur

```bash
uvicorn main:app --reload
```

## Tester

```bash
# GET
curl http://127.0.0.1:8000/todos

# POST
curl -X POST http://127.0.0.1:8000/todos \
  -H "Content-Type: application/json" \
  -d '{"title":"Write docs","done":false}'

# PATCH
curl -X PATCH http://127.0.0.1:8000/todos/1 \
  -H "Content-Type: application/json" \
  -d '{"done":true}'

# DELETE
curl -X DELETE http://127.0.0.1:8000/todos/1
```

---

# Différences visibles dans le code

* **Déclaration des routes**

  * Next.js : fichiers/dossiers `app/api/.../route.ts`.
  * FastAPI : décorateurs `@app.get`, `@app.post`, etc.

* **Validation**

  * Next.js : libre, tu valides toi-même (ou via Zod/Yup).
  * FastAPI : Pydantic valide automatiquement le schéma d’entrée et produit un schéma OpenAPI.

* **Documentation**

  * Next.js : pas de Swagger/OAS par défaut.
  * FastAPI : docs intégrées sur `/docs` (Swagger UI) et `/redoc`.

* **Usage type**

  * Next.js : idéal pour UI + endpoints simples.
  * FastAPI : idéal pour un backend API structuré, data/ML, microservices.

