Backend Setup (FastAPI)
📦 Prerequisites
Python 3.10+

PostgreSQL installed and running

VS Code or any IDE

# Install Dependencies
cd backend
python -m venv venv
venv\Scripts\activate     # (Windows)
pip install -r requirements.txt

# If requirements.txt not available, install manually:
pip install fastapi uvicorn sqlalchemy psycopg2-binary python-jose passlib[bcrypt] python-multipart

# Configure Database
In app/database.py:
DATABASE_URL = "postgresql://postgres:your_password@localhost:5432/issues_db"
Replace with your actual DB credentials
password : postgres

# Create Tables
# run manually
python
from app.database import Base, engine
Base.metadata.create_all(bind=engine)
exit()

# Run Server
uvicorn app.main:app --reload

Server runs at: http://localhost:8000



Frontend Setup (SvelteKit)
📦 Prerequisites
Node.js 18+

pnpm or npm

#  Install
cd frontend
npm install

# Run Dev Server
npm run dev

Frontend runs at: http://localhost:5173


Auth Flow
/signup: Register (email, password, role)

/login: Get JWT token via form (stored in localStorage)

# Authenticated requests include:

headers: { Authorization: `Bearer ${token}` }

# Roles
ADMIN: Full access (can delete any issue)

MAINTAINER: Can edit all

REPORTER: Can create/view only their own issues

# Realtime WebSocket

# FastAPI:
@app.websocket("/ws/issues")
async def websocket_endpoint(websocket: WebSocket):
    await websocket.accept()
    while True:
        await websocket.send_json({"event": "updated"})
        await asyncio.sleep(5)  # simulate updates

# SvelteKit:
const socket = new WebSocket('ws://localhost:8000/ws/issues');
socket.onmessage = () => loadIssues(); // refresh on update

# Features
✅ Signup / Login with Role

✅ Create / View / Update / Delete Issues

✅ Role-Based Access (RBAC)

✅ Real-time issue updates (WebSocket)

✅ Responsive UI with Tailwind

✅ JWT-based Authentication

# Todo / Improvements
 Upload & preview attachments

 Pagination on issue list

 Production deployment (e.g. Vercel + Railway)

 Unit tests and CI pipeline

 