# Testing Guide - Naar & Noor

Complete testing framework for full-stack development with backend (.NET) and frontend (Angular).

## Quick Start

### Backend Tests

```bash
cd api-server
dotnet test --collect:"XPlat Code Coverage" --settings:../coverlet.runsettings
```

### Frontend Tests

```bash
cd naar-noor
npm run test:ci
```

### All Tests with Coverage

```bash
# Backend
cd api-server && dotnet test --collect:"XPlat Code Coverage"

# Frontend
cd ../naar-noor && npm run test:ci

# Validate coverage thresholds
python ../scripts/validate-coverage.py --backend-dir tests
```

## Test Structure

### Backend (NaarNoor) - .NET 8 / xUnit / FsCheck

**Layer Organization:**

1. **Domain Tests** (`NaarNoor.Domain.Tests`)
   - Entity validation and immutability
   - Business rule enforcement
   - Domain invariant preservation
   - Value object equality
   - Property 1-4 tests

2. **Application Tests** (`NaarNoor.Application.Tests`)
   - Command handler processing
   - Query handler filtering
   - Validator enforcement
   - Query pagination (Property 8)
   - Property 5-6 tests

3. **Infrastructure Tests** (`NaarNoor.Infrastructure.Tests`)
   - Repository CRUD operations (Property 7)
   - Database transaction atomicity (Property 9)
   - Referential integrity enforcement (Property 10)
   - Property 7, 9-10 tests

4. **API Tests** (`NaarNoor.API.Tests`)
   - HTTP endpoint integration
   - Authorization enforcement
   - Exception mapping
   - Input validation
   - Property 11, 16-17 tests

**Running Backend Tests:**

```bash
# All tests
dotnet test

# Specific layer
dotnet test --filter "Category=Domain"
dotnet test --filter "Category=Application"
dotnet test --filter "Category=Infrastructure"
dotnet test --filter "Category=API"

# With verbose output
dotnet test -v detailed

# Watch mode (continuous)
dotnet watch test
```

### Frontend (Angular) - Jasmine / Karma / Istanbul

**Test Organization:**

1. **Services** (`src/app/services/**/*.spec.ts`)
   - HTTP communication (Property 12)
   - Error handling (Property 13)
   - Caching behavior (Property 14)
   - 3-4 test methods per service
   - Coverage target: 80%

2. **Components** (`src/app/components/**/*.spec.ts`)
   - State management (Property 15)
   - User interactions
   - Form binding and validation
   - 3-4 test methods per component
   - Coverage target: 75%

3. **E2E Tests** (`cypress/e2e/**/*.cy.ts`)
   - Complete user workflows
   - Reservation creation
   - Order placement
   - Menu search & filtering
   - 8-10 scenarios per flow

**Running Frontend Tests:**

```bash
# Watch mode
npm test

# CI mode (single run)
npm run test:ci

# Coverage check
npm run coverage:check

# E2E tests
npx cypress run

# E2E open browser
npx cypress open
```

## Coverage Requirements

| Layer | Threshold | Current | Status |
|-------|-----------|---------|--------|
| **Backend** | | | |
| Domain | 85% | 88.5% | ✅ Pass |
| Application | 82% | 78.57% | ⚠️ Needs 3.43% |
| Infrastructure | 78% | 78.33% | ✅ Pass |
| API | 80% | 82.15% | ✅ Pass |
| **Frontend** | | | |
| Services | 80% | ⏳ | In Progress |
| Components | 75% | ⏳ | In Progress |

## Coverage Validation

### Run Locally

```bash
# Validate backend coverage
python scripts/validate-coverage.py --backend-dir api-server/tests

# Generate reports
python scripts/generate-coverage-report.py --backend-report backend.json

# Generate badges
python scripts/generate-coverage-badge.py --backend-report backend.json --output coverage/
```

### View Reports

```bash
# Backend HTML report
open coverage/backend-reports/index.html

# Frontend HTML report
open naar-noor/coverage/index.html
```

## Property-Based Testing

Uses **FsCheck** for backend, runs 100 iterations with random inputs per test.

### Properties Implemented

| # | Name | Layer | Status |
|----|------|-------|--------|
| 1 | Entity State Validation | Domain | ✅ |
| 2 | Value Object Immutability | Domain | ✅ |
| 3 | Entity State Transitions | Domain | ✅ |
| 4 | Domain Invariants | Domain | ✅ |
| 5 | Command Handler Processing | Application | ✅ |
| 6 | Query Result Filtering | Application | ✅ |
| 7 | CRUD Round-Trip | Infrastructure | ✅ |
| 8 | Query Pagination | Application | ✅ |
| 9 | Transaction Atomicity | Infrastructure | ✅ |
| 10 | Referential Integrity | Infrastructure | ✅ |
| 11 | HTTP Exception Mapping | API | ✅ |
| 12 | Service HTTP Communication | Frontend | ⏳ |
| 13 | Service Error Handling | Frontend | ⏳ |
| 14 | Service Caching | Frontend | ⏳ |
| 15 | Component State Management | Frontend | ⏳ |
| 16 | Input Validation | API | ✅ |
| 17 | Authorization Enforcement | API | ✅ |

### Example Property Test

```csharp
[Property(MaxTest = 100)]
public async Task Property_CreateOperation_PersistsValidChef(string name, string specialization)
{
    if (string.IsNullOrWhiteSpace(name) || string.IsNullOrWhiteSpace(specialization))
        return;

    var chef = new Chef(name, specialization, yearsOfExperience: 5, rating: 4.5m);
    var repository = new Repository<Chef>(_context);

    await repository.AddAsync(chef);
    await _context.SaveChangesAsync();

    var retrieved = await repository.GetByIdAsync(chef.Id);
    Assert.NotNull(retrieved);
    Assert.Equal(name, retrieved.Name);
}
```

## Pre-Commit Hooks (Husky)

Tests run automatically on commit:

```bash
git commit -m "message"  # Runs npm test (frontend)
```

**If tests fail:** Fix and retry
```bash
npm test  # Check locally first
git add .
git commit -m "message"  # Retry
```

**Emergency bypass (not recommended):**
```bash
git commit --no-verify
```

## Continuous Integration (GitHub Actions)

Workflow: `.github/workflows/tests.yml`

**Runs automatically on:**
- Push to main/develop
- Pull request creation
- Manual trigger

**Jobs:**

1. **backend-tests** (15 min timeout)
   - Setup .NET 8
   - Restore dependencies
   - Build projects
   - Run tests with coverage
   - Upload TRX files

2. **frontend-tests** (15 min timeout)
   - Setup Node.js 18
   - Install dependencies
   - Run Karma/Jasmine
   - Collect Istanbul coverage
   - Upload coverage XML

3. **coverage-gate** (Depends on 1 & 2)
   - Validate coverage thresholds
   - Generate comparison report
   - Generate SVG badges
   - Post PR comment with results
   - Upload artifacts (30 days)

## Troubleshooting

### Backend Tests Not Running

```bash
# Check .NET version
dotnet --version  # Should be 8.0+

# Restore dependencies
dotnet restore api-server

# Clean build
dotnet clean api-server && dotnet build api-server
```

### Coverage Not Generated

```bash
# Verify coverlet.runsettings exists
ls api-server/coverlet.runsettings

# Check project has Coverlet dependency
grep Coverlet.collector api-server/tests/*/NaarNoor.*.Tests.csproj

# Run with explicit settings
dotnet test api-server --collect:"XPlat Code Coverage" --settings:api-server/coverlet.runsettings
```

### Frontend Tests Fail

```bash
# Install dependencies
cd naar-noor && npm ci

# Check Karma configured
ls karma.conf.js

# Ensure Chrome is installed
which google-chrome  # Linux/Mac
where chrome.exe     # Windows

# Run in debug mode
npm test -- --browsers=Chrome --no-single-run
```

### Pre-Commit Hook Issues

```bash
# Check Husky installed
ls naar-noor/.husky/pre-commit

# Reinstall hooks
cd naar-noor && npx husky install

# Make executable (Linux/Mac)
chmod +x naar-noor/.husky/pre-commit
```

## Best Practices

1. **Run tests before commit** - Use pre-commit hooks
2. **Keep coverage high** - Aim for thresholds, not just pass
3. **Test business logic** - Property tests are more valuable than simple unit tests
4. **Use meaningful assertions** - Clear messages on failure
5. **Organize tests logically** - Mirror source structure
6. **Document complex tests** - Explain the "why"
7. **Run E2E tests regularly** - Catch integration issues early

## Next Steps

- Complete frontend service tests (Property 12-14)
- Implement component tests (Property 15)
- Add E2E test coverage
- Increase Application layer coverage to 82%+
- Monitor coverage trends in CI/CD
