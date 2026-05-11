# Local Development Setup Guide

Last Updated: May 12, 2024

## 1. Prerequisites

- Node.js 18+
- Python 3.9+
- Docker Desktop
- Git
- Text editor (VS Code)
- 8GB+ RAM
- 20GB+ disk space

## 2. Setup Steps

### 2.1 Clone Repository

```bash
git clone https://github.com/lutervyn/project.git
cd project
```

### 2.2 Install Dependencies

```bash
npm install
pip install -r requirements.txt
```

### 2.3 Configuration

```bash
cp .env.example .env
# Edit .env with local settings
```

### 2.4 Start Services

```bash
docker-compose up -d
npm run dev
```

## 3. Database Setup

```bash
npm run db:migrate
npm run db:seed
```

## 4. Running Tests

```bash
npm test
npm run test:integration
npm run test:e2e
```

## 5. Debugging

- Chrome DevTools
- VS Code debugger
- Node inspector
- Console logs

## 6. Contact

- Setup Help: setup@lutervyn.com
- Dev Support: dev@lutervyn.com
