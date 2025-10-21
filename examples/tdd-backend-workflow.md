# TDD Backend Workflow Example

Complete example of using the TDD Backend Orchestrator to build a FastAPI endpoint with automated testing.

---

## Scenario

You need to build a user registration API endpoint for a FastAPI application with:
- Pydantic model for user input validation
- SQLAlchemy model for database
- POST endpoint with proper error handling
- Unit tests with pytest
- Integration tests with TestClient

---

## Prerequisites

- Roo Code installed with modes imported
- Existing FastAPI project (or willingness to start fresh)
- Python 3.8+ environment

---

## Step-by-Step Workflow

### Step 1: Launch TDD Backend Orchestrator

1. Open Roo Code
2. Select **⚙️ TDD Backend Orchestrator** from the mode selector
3. The orchestrator will coordinate the entire Red-Green-Refactor cycle

### Step 2: Describe Your Feature

Type in Roo Code:

```
Create a user registration endpoint for FastAPI:
- POST /api/v1/users/register
- Accepts: email, password, full_name
- Returns: user_id, email, created_at
- Validates email format
- Hashes password with bcrypt
- Returns 400 for duplicate email
- Includes pytest tests
```

### Step 3: Orchestrator Executes (Automated)

The TDD Backend Orchestrator will automatically:

#### Phase 1: RED (Write Failing Tests)
Launches **Python FastAPI** mode to create:

```python
# tests/test_user_registration.py
import pytest
from fastapi.testclient import TestClient
from app.main import app

client = TestClient(app)

def test_register_user_success():
    """Test successful user registration"""
    response = client.post("/api/v1/users/register", json={
        "email": "test@example.com",
        "password": "SecurePass123!",
        "full_name": "Test User"
    })

    assert response.status_code == 200
    data = response.json()
    assert "user_id" in data
    assert data["email"] == "test@example.com"
    assert "created_at" in data
    assert "password" not in data  # Password should not be returned

def test_register_duplicate_email():
    """Test registration with duplicate email returns 400"""
    # First registration
    client.post("/api/v1/users/register", json={
        "email": "duplicate@example.com",
        "password": "Pass123!",
        "full_name": "First User"
    })

    # Duplicate registration
    response = client.post("/api/v1/users/register", json={
        "email": "duplicate@example.com",
        "password": "Pass456!",
        "full_name": "Second User"
    })

    assert response.status_code == 400
    assert "already exists" in response.json()["detail"].lower()

def test_register_invalid_email():
    """Test registration with invalid email format"""
    response = client.post("/api/v1/users/register", json={
        "email": "invalid-email",
        "password": "Pass123!",
        "full_name": "Test User"
    })

    assert response.status_code == 422  # Validation error
```

**Result:** Tests fail (no implementation yet) ❌

#### Phase 2: GREEN (Implement Feature)
Launches **Python FastAPI** mode again to implement:

```python
# app/models/user.py
from sqlalchemy import Column, Integer, String, DateTime
from sqlalchemy.sql import func
from app.database import Base

class User(Base):
    __tablename__ = "users"

    id = Column(Integer, primary_key=True, index=True)
    email = Column(String, unique=True, index=True, nullable=False)
    hashed_password = Column(String, nullable=False)
    full_name = Column(String, nullable=False)
    created_at = Column(DateTime(timezone=True), server_default=func.now())


# app/schemas/user.py
from pydantic import BaseModel, EmailStr, Field
from datetime import datetime

class UserRegisterRequest(BaseModel):
    email: EmailStr
    password: str = Field(..., min_length=8)
    full_name: str = Field(..., min_length=1, max_length=255)

class UserRegisterResponse(BaseModel):
    user_id: int
    email: str
    created_at: datetime

    class Config:
        from_attributes = True


# app/routers/users.py
from fastapi import APIRouter, Depends, HTTPException
from sqlalchemy.orm import Session
from app.database import get_db
from app.models.user import User
from app.schemas.user import UserRegisterRequest, UserRegisterResponse
from app.utils.security import hash_password

router = APIRouter(prefix="/api/v1/users", tags=["users"])

@router.post("/register", response_model=UserRegisterResponse, status_code=200)
async def register_user(
    user_data: UserRegisterRequest,
    db: Session = Depends(get_db)
):
    """Register a new user"""

    # Check if email already exists
    existing_user = db.query(User).filter(User.email == user_data.email).first()
    if existing_user:
        raise HTTPException(
            status_code=400,
            detail="User with this email already exists"
        )

    # Hash password
    hashed_password = hash_password(user_data.password)

    # Create user
    db_user = User(
        email=user_data.email,
        hashed_password=hashed_password,
        full_name=user_data.full_name
    )
    db.add(db_user)
    db.commit()
    db.refresh(db_user)

    return UserRegisterResponse(
        user_id=db_user.id,
        email=db_user.email,
        created_at=db_user.created_at
    )


# app/utils/security.py
import bcrypt

def hash_password(password: str) -> str:
    """Hash password using bcrypt"""
    salt = bcrypt.gensalt()
    hashed = bcrypt.hashpw(password.encode('utf-8'), salt)
    return hashed.decode('utf-8')

def verify_password(plain_password: str, hashed_password: str) -> bool:
    """Verify password against hash"""
    return bcrypt.checkpw(
        plain_password.encode('utf-8'),
        hashed_password.encode('utf-8')
    )
```

**Result:** Tests pass ✅

#### Phase 3: REFACTOR (Code Review)
Launches **Code Reviewer** mode to analyze:

**Code Reviewer Report:**
```markdown
## Security Analysis ✅
- ✅ Password is hashed with bcrypt (not stored plaintext)
- ✅ Password not returned in response
- ✅ Email validated with Pydantic EmailStr
- ✅ SQL injection prevented (SQLAlchemy ORM)

## Performance Analysis
- ✅ Email column indexed for fast duplicate checks
- ⚠️ Consider rate limiting on registration endpoint
- 💡 Suggestion: Add database transaction rollback on error

## Best Practices
- ✅ Type hints on all parameters
- ✅ Async/await used correctly
- ✅ Pydantic models for validation
- ✅ Proper HTTP status codes (200, 400, 422)
- ⚠️ Add logging for registration events

## Suggested Refactoring
1. Add rate limiting decorator
2. Add structured logging
3. Add email verification (optional enhancement)

## Overall: APPROVED ✅
Code meets quality standards. Minor suggestions for production hardening.
```

#### Phase 4: Final Report (Orchestrator Aggregates)

The orchestrator returns:

```markdown
# TDD Backend Workflow Complete ✅

## Summary
Created user registration endpoint with:
- FastAPI POST endpoint
- Pydantic validation
- Bcrypt password hashing
- SQLAlchemy models
- Pytest tests (3 test cases)

## Test Results
✅ All 3 tests passing
- test_register_user_success
- test_register_duplicate_email
- test_register_invalid_email

## Code Review
✅ Approved with minor suggestions:
- Consider rate limiting
- Add structured logging
- Transaction rollback on errors

## Next Steps
1. Run: `pytest tests/test_user_registration.py`
2. Optional: Implement suggested improvements
3. Deploy: Endpoint ready for production
```

---

## What the Orchestrator Did Automatically

1. ✅ Wrote failing tests first (RED)
2. ✅ Implemented minimal code to pass tests (GREEN)
3. ✅ Reviewed code for security/quality (REFACTOR)
4. ✅ Aggregated results into actionable report

**Time Saved:** 30-45 minutes of manual context switching between modes

---

## Manual Alternative (Without Orchestrator)

If you did this manually, you'd need to:

```bash
1. Switch to Python FastAPI mode → write tests
2. Run tests manually → see failures
3. Switch to Python FastAPI mode → implement code
4. Run tests manually → verify passing
5. Switch to Code Reviewer mode → request review
6. Review suggestions → implement changes
7. Run tests again → verify still passing
```

**Orchestrator handles all of this automatically in one workflow!**

---

## Running the Tests

```bash
# In your FastAPI project
pytest tests/test_user_registration.py -v

# Expected output:
# test_user_registration.py::test_register_user_success PASSED
# test_user_registration.py::test_register_duplicate_email PASSED
# test_user_registration.py::test_register_invalid_email PASSED
```

---

## Production Enhancements

After the orchestrator completes, you might add:

### Rate Limiting
```python
from slowapi import Limiter
from slowapi.util import get_remote_address

limiter = Limiter(key_func=get_remote_address)

@router.post("/register", response_model=UserRegisterResponse)
@limiter.limit("5/minute")  # Max 5 registrations per minute per IP
async def register_user(...):
    # ... existing code
```

### Structured Logging
```python
import structlog

logger = structlog.get_logger()

@router.post("/register", response_model=UserRegisterResponse)
async def register_user(...):
    logger.info("user_registration_attempt", email=user_data.email)

    # ... registration logic

    logger.info("user_registration_success", user_id=db_user.id, email=db_user.email)
    return response
```

### Email Verification
```python
# Send verification email after registration
from app.utils.email import send_verification_email

await send_verification_email(db_user.email, verification_token)
```

---

## Tips & Tricks

### Customize the Workflow

Instead of the full description, you can ask for specific variations:

```
"Create user registration with OAuth integration"
"Create user registration with two-factor authentication"
"Create user registration with email verification"
```

### Request Specific Testing

```
"Include integration tests with real database"
"Add load tests with Locust"
"Include security tests for SQL injection"
```

### Iterate on the Design

After the first pass:
```
"Refactor to use repository pattern"
"Add caching with Redis"
"Add API versioning"
```

---

## Related Workflows

- **[TDD Frontend Workflow](./tdd-frontend-workflow.md)** - React/Next.js with testing
- **[Design Review Workflow](./design-review-workflow.md)** - UI/UX quality automation
- **[Full-Stack Project Setup](./full-stack-project-setup.md)** - Complete app with TDD

---

**Happy TDD! 🧪**
