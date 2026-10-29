# Contributing to CityMove

Thank you for your interest in contributing to **CityMove**! CityMove is a 100% free and open-source civic technology platform helping city commuters navigate roads, report hazards, and track public transport without proprietary lock-in.

---

## Code of Conduct

By participating in this project, you agree to abide by our [Code of Conduct](CODE_OF_CONDUCT.md). Please treat all contributors with kindness, empathy, and respect.

---

## Local Development Workflow

### Prerequisites
- Node.js 20+ (Node 22 / 24 supported)
- npm 10+
- (Optional) Docker and Docker Compose

### 1. Clone & Install
```bash
git clone https://github.com/citymove-org/citymove.git
cd citymove

# Install backend dependencies
cd backend && npm install

# Install frontend dependencies
cd ../frontend && npm install
```

### 2. Database Setup & Seeding
```bash
cd backend
npm run seed
```

### 3. Running Development Servers
In two separate terminals:

**Backend (Port 3001):**
```bash
cd backend
npm run dev
```

**Frontend (Port 5173):**
```bash
cd frontend
npm run dev
```

Visit `http://localhost:5173` in your browser.

---

## Testing Guidelines

CityMove maintains high test quality across both stacks:
- **Backend**: Jest + Supertest (`npm test` inside `/backend`)
- **Frontend**: Vitest + React Testing Library (`npm test` inside `/frontend`)

All pull requests must pass existing tests and include tests for new functionality.

---

## Pull Request Guidelines

1. Fork the repository and create a descriptive branch: `git checkout -b feature/pothole-photo-compression`
2. Commit your changes with clear messages.
3. Verify all tests pass:
   ```bash
   npm test # in backend
   npm test # in frontend
   ```
4. Push to your fork and submit a Pull Request against `main`.

