# 🤝 Contributing to Naar & Noor

Thank you for your interest in contributing to **Naar & Noor**! We welcome contributions from the community and appreciate your help in making this project better.

---

## 📜 Code of Conduct

We are committed to providing a welcoming and inclusive environment for everyone. Please:

- ✅ Be respectful and professional in all interactions
- ✅ Welcome newcomers and help them get started
- ✅ Accept constructive criticism gracefully
- ✅ Focus on what is best for the community
- ❌ Do not engage in harassment or discriminatory behavior

---

## 🚀 Quick Start

### 1. Fork the Repository

Click the **Fork** button at the top right of the repository page.

### 2. Clone Your Fork

```bash
git clone https://github.com/YOUR_USERNAME/Naar-Noor.git
cd Naar-Noor
```

### 3. Add Upstream Remote

```bash
git remote add upstream https://github.com/Mostafa-SAID7/Naar-Noor.git
```

### 4. Create a Feature Branch

```bash
git checkout -b feature/your-feature-name
```

### 5. Make Your Changes

Follow the coding standards and best practices outlined below.

### 6. Commit Your Changes

```bash
git add .
git commit -m "feat: Add your feature description"
```

### 7. Push to Your Fork

```bash
git push origin feature/your-feature-name
```

### 8. Create a Pull Request

Go to the original repository and click **New Pull Request**.

---

## 🌿 Branching Strategy

### Branch Naming Convention

| Prefix | Purpose | Example |
|--------|---------|---------|
| `feature/` | New features | `feature/add-payment-integration` |
| `bugfix/` | Bug fixes | `bugfix/fix-reservation-validation` |
| `hotfix/` | Production hotfixes | `hotfix/critical-security-patch` |
| `docs/` | Documentation updates | `docs/update-api-documentation` |
| `refactor/` | Code refactoring | `refactor/optimize-database-queries` |
| `test/` | Test additions | `test/add-reservation-tests` |

### Example Workflow

```bash
# Create feature branch
git checkout -b feature/add-user-authentication

# Make changes and commit
git add .
git commit -m "feat: Implement JWT authentication"

# Keep your branch updated
git fetch upstream
git rebase upstream/main

# Push to your fork
git push origin feature/add-user-authentication
```

---

## 💬 Commit Message Guidelines

### Format

```
<type>: <subject>

<body>

<footer>
```

### Types

| Type | Description | Example |
|------|-------------|---------|
| `feat` | New feature | `feat: Add email notifications` |
| `fix` | Bug fix | `fix: Resolve menu price display issue` |
| `docs` | Documentation | `docs: Update API documentation` |
| `style` | Code style changes | `style: Format code with Prettier` |
| `refactor` | Code refactoring | `refactor: Simplify reservation logic` |
| `test` | Test additions/changes | `test: Add unit tests for MenuService` |
| `chore` | Build/dependency changes | `chore: Update Angular to v18` |
| `perf` | Performance improvements | `perf: Optimize database queries` |

### Examples

**Good Commit Messages:**

```
feat: Add reservation confirmation email

Implement email notification system using SendGrid.
Sends confirmation email when reservation status changes to Confirmed.

Fixes #456
```

```
fix: Resolve decimal formatting for menu prices

Menu item prices were displaying with incorrect decimal places.
Updated to use currency pipe with 2 decimal places.

Closes #789
```

**Bad Commit Messages:**

```
❌ Fixed stuff
❌ Update
❌ WIP
❌ asdfasdf
```

---

## 🎨 Code Style Guidelines

### Backend (C# / .NET)

Follow [Microsoft C# Coding Conventions](https://docs.microsoft.com/dotnet/csharp/fundamentals/coding-style/coding-conventions)

**Naming Conventions:**

| Element | Convention | Example |
|---------|------------|---------|
| Classes | PascalCase | `ReservationService` |
| Methods | PascalCase | `GetMenuItems()` |
| Variables | camelCase | `menuItems` |
| Constants | PascalCase | `MaxGuestCount` |
| Private fields | _camelCase | `_context` |

**Code Example:**

```csharp
namespace NaarNoor.Application.Reservations.Commands;

/// <summary>
/// Handles the creation of new reservations.
/// </summary>
public class CreateReservationCommandHandler 
    : IRequestHandler<CreateReservationCommand, int>
{
    private readonly IApplicationDbContext _context;
    private readonly ILogger<CreateReservationCommandHandler> _logger;

    public CreateReservationCommandHandler(
        IApplicationDbContext context,
        ILogger<CreateReservationCommandHandler> logger)
    {
        _context = context;
        _logger = logger;
    }

    /// <summary>
    /// Creates a new reservation in the database.
    /// </summary>
    /// <param name="request">The reservation details</param>
    /// <param name="cancellationToken">Cancellation token</param>
    /// <returns>The ID of the created reservation</returns>
    public async Task<int> Handle(
        CreateReservationCommand request, 
        CancellationToken cancellationToken)
    {
        var reservation = new Reservation
        {
            GuestName = request.GuestName,
            Email = request.Email,
            PhoneNumber = request.PhoneNumber,
            ReservationDate = request.ReservationDate,
            NumberOfGuests = request.NumberOfGuests,
            Status = ReservationStatus.Pending
        };

        _context.Reservations.Add(reservation);
        await _context.SaveChangesAsync(cancellationToken);

        _logger.LogInformation(
            "Reservation created for {GuestName} on {Date}", 
            reservation.GuestName, 
            reservation.ReservationDate);

        return reservation.Id;
    }
}
```

**Best Practices:**

- ✅ Use meaningful variable and method names
- ✅ Keep methods small and focused (single responsibility)
- ✅ Add XML documentation comments for public APIs
- ✅ Use async/await for I/O operations
- ✅ Handle exceptions appropriately
- ✅ Use dependency injection
- ✅ Write unit tests for business logic

---

### Frontend (TypeScript / Angular)

Follow [Angular Style Guide](https://angular.io/guide/styleguide)

**Naming Conventions:**

| Element | Convention | Example |
|---------|------------|---------|
| Components | PascalCase | `MenuComponent` |
| Services | PascalCase | `ApiService` |
| Variables | camelCase | `menuItems` |
| Constants | UPPER_SNAKE_CASE | `MAX_ITEMS` |
| Interfaces | PascalCase with I prefix | `IMenuItem` |

**Code Example:**

```typescript
import { Injectable } from '@angular/core';
import { HttpClient } from '@angular/common/http';
import { Observable } from 'rxjs';
import { environment } from '../environments/environment';

/**
 * Service for handling API communication
 */
@Injectable({
  providedIn: 'root'
})
export class ApiService {
  private readonly apiUrl = environment.apiUrl;

  constructor(private http: HttpClient) {}

  /**
   * Fetches all menu items from the API
   * @returns Observable of menu items array
   */
  getMenuItems(): Observable<MenuItem[]> {
    return this.http.get<MenuItem[]>(`${this.apiUrl}/menu`);
  }

  /**
   * Creates a new reservation
   * @param reservation - The reservation data
   * @returns Observable of created reservation
   */
  createReservation(reservation: CreateReservationDto): Observable<Reservation> {
    return this.http.post<Reservation>(
      `${this.apiUrl}/reservations`, 
      reservation
    );
  }
}
```

**Best Practices:**

- ✅ Use standalone components
- ✅ Implement OnPush change detection when possible
- ✅ Unsubscribe from observables to prevent memory leaks
- ✅ Use TypeScript strict mode
- ✅ Add JSDoc comments for public methods
- ✅ Keep components focused and small
- ✅ Use reactive forms for complex forms
- ✅ Write unit tests for components and services

---

## 🧪 Testing Requirements

### Backend Testing

**Run Tests:**

```bash
cd api-server
dotnet test
```

**Unit Test Example:**

```csharp
using Xunit;
using Moq;
using NaarNoor.Application.Reservations.Commands.CreateReservation;

public class CreateReservationCommandHandlerTests
{
    private readonly Mock<IApplicationDbContext> _mockContext;
    private readonly CreateReservationCommandHandler _handler;

    public CreateReservationCommandHandlerTests()
    {
        _mockContext = new Mock<IApplicationDbContext>();
        _handler = new CreateReservationCommandHandler(_mockContext.Object);
    }

    [Fact]
    public async Task Handle_WithValidData_ReturnsReservationId()
    {
        // Arrange
        var command = new CreateReservationCommand
        {
            GuestName = "John Doe",
            Email = "john@example.com",
            PhoneNumber = "+1-555-0123",
            ReservationDate = DateTime.Now.AddDays(7),
            NumberOfGuests = 4
        };

        // Act
        var result = await _handler.Handle(command, CancellationToken.None);

        // Assert
        Assert.True(result > 0);
        _mockContext.Verify(x => x.SaveChangesAsync(It.IsAny<CancellationToken>()), Times.Once);
    }

    [Fact]
    public async Task Handle_WithInvalidEmail_ThrowsValidationException()
    {
        // Arrange
        var command = new CreateReservationCommand
        {
            GuestName = "John Doe",
            Email = "invalid-email",
            PhoneNumber = "+1-555-0123",
            ReservationDate = DateTime.Now.AddDays(7),
            NumberOfGuests = 4
        };

        // Act & Assert
        await Assert.ThrowsAsync<ValidationException>(
            () => _handler.Handle(command, CancellationToken.None)
        );
    }
}
```

---

### Frontend Testing

**Run Tests:**

```bash
cd naar-noor
npm test
```

**Component Test Example:**

```typescript
import { ComponentFixture, TestBed } from '@angular/core/testing';
import { HttpClientTestingModule } from '@angular/common/http/testing';
import { MenuComponent } from './menu.component';
import { ApiService } from '../../services/api.service';
import { of } from 'rxjs';

describe('MenuComponent', () => {
  let component: MenuComponent;
  let fixture: ComponentFixture<MenuComponent>;
  let apiService: jasmine.SpyObj<ApiService>;

  beforeEach(async () => {
    const apiServiceSpy = jasmine.createSpyObj('ApiService', ['getMenuItems']);

    await TestBed.configureTestingModule({
      imports: [MenuComponent, HttpClientTestingModule],
      providers: [
        { provide: ApiService, useValue: apiServiceSpy }
      ]
    }).compileComponents();

    fixture = TestBed.createComponent(MenuComponent);
    component = fixture.componentInstance;
    apiService = TestBed.inject(ApiService) as jasmine.SpyObj<ApiService>;
  });

  it('should create', () => {
    expect(component).toBeTruthy();
  });

  it('should load menu items on init', () => {
    const mockItems = [
      { id: 1, name: 'Tandoori Chicken', price: 14.99, category: 'Mains' }
    ];
    apiService.getMenuItems.and.returnValue(of(mockItems));

    component.ngOnInit();

    expect(component.items).toEqual(mockItems);
    expect(apiService.getMenuItems).toHaveBeenCalled();
  });

  it('should display menu items in template', () => {
    component.items = [
      { id: 1, name: 'Tandoori Chicken', price: 14.99, category: 'Mains' }
    ];
    fixture.detectChanges();

    const compiled = fixture.nativeElement;
    expect(compiled.querySelector('h2').textContent).toContain('Tandoori Chicken');
  });
});
```

---

## 📝 Pull Request Process

### Before Submitting

- [ ] Code follows style guidelines
- [ ] All tests pass
- [ ] New tests added for new features
- [ ] Documentation updated
- [ ] No merge conflicts
- [ ] Commit messages follow conventions

### PR Template

```markdown
## 📋 Description
Brief description of what this PR does

## 🔧 Type of Change
- [ ] 🐛 Bug fix (non-breaking change which fixes an issue)
- [ ] ✨ New feature (non-breaking change which adds functionality)
- [ ] 💥 Breaking change (fix or feature that would cause existing functionality to not work as expected)
- [ ] 📝 Documentation update

## 🔗 Related Issues
Fixes #123
Closes #456

## 🧪 Testing
Describe the testing you performed:
- [ ] Unit tests added/updated
- [ ] Integration tests added/updated
- [ ] Manual testing performed

## 📸 Screenshots (if applicable)
Add screenshots to help explain your changes

## ✅ Checklist
- [ ] My code follows the style guidelines of this project
- [ ] I have performed a self-review of my own code
- [ ] I have commented my code, particularly in hard-to-understand areas
- [ ] I have made corresponding changes to the documentation
- [ ] My changes generate no new warnings
- [ ] I have added tests that prove my fix is effective or that my feature works
- [ ] New and existing unit tests pass locally with my changes
```

### Review Process

1. **Submit PR** - Create pull request with clear description
2. **Automated Checks** - CI/CD pipeline runs tests
3. **Code Review** - Maintainers review your code
4. **Address Feedback** - Make requested changes
5. **Approval** - PR approved by maintainers
6. **Merge** - PR merged into main branch

---

## 📚 Documentation

### When to Update Documentation

- ✅ Adding new features
- ✅ Changing API endpoints
- ✅ Modifying configuration
- ✅ Updating dependencies
- ✅ Fixing bugs that affect usage

### Documentation Files

| File | Purpose |
|------|---------|
| `README.md` | Project overview and quick start |
| `docs/GETTING_STARTED.md` | Setup instructions |
| `docs/API.md` | API endpoint documentation |
| `docs/FRONTEND.md` | Frontend development guide |
| `docs/BACKEND.md` | Backend development guide |
| `docs/DATABASE.md` | Database schema |
| `docs/DEPLOYMENT.md` | Deployment instructions |
| `docs/TROUBLESHOOTING.md` | Common issues and solutions |

---

## ⚡ Performance Guidelines

### Backend Performance

- ✅ Use `AsNoTracking()` for read-only queries
- ✅ Implement pagination for large datasets
- ✅ Add database indexes for frequently queried columns
- ✅ Use caching for frequently accessed data
- ✅ Avoid N+1 query problems
- ✅ Use async/await for I/O operations

**Example:**

```csharp
// Good - Uses AsNoTracking and pagination
var menuItems = await _context.MenuItems
    .AsNoTracking()
    .Where(m => m.IsAvailable)
    .OrderBy(m => m.Name)
    .Skip((page - 1) * pageSize)
    .Take(pageSize)
    .ToListAsync();

// Bad - Loads all data and tracks changes
var menuItems = _context.MenuItems.ToList();
```

### Frontend Performance

- ✅ Use lazy loading for routes
- ✅ Implement OnPush change detection
- ✅ Optimize images (WebP, lazy loading)
- ✅ Minimize bundle size
- ✅ Use trackBy in *ngFor loops
- ✅ Unsubscribe from observables

**Example:**

```typescript
// Good - Lazy loading
{
  path: 'menu',
  loadComponent: () => import('./pages/menu/menu.component')
    .then(m => m.MenuComponent)
}

// Good - OnPush change detection
@Component({
  selector: 'app-menu',
  changeDetection: ChangeDetectionStrategy.OnPush
})
export class MenuComponent {}

// Good - TrackBy function
trackByItem(index: number, item: MenuItem): number {
  return item.id;
}
```

---

## 🔒 Security Guidelines

### General Security

- ✅ Validate all user inputs
- ✅ Use parameterized queries (prevent SQL injection)
- ✅ Never commit secrets or API keys
- ✅ Use HTTPS in production
- ✅ Implement proper authentication/authorization
- ✅ Follow OWASP security guidelines
- ✅ Keep dependencies up to date

### Backend Security

```csharp
// Good - Parameterized query (EF Core does this automatically)
var user = await _context.Users
    .FirstOrDefaultAsync(u => u.Email == email);

// Bad - String concatenation (vulnerable to SQL injection)
var query = $"SELECT * FROM Users WHERE Email = '{email}'";
```

### Frontend Security

```typescript
// Good - Sanitize user input
import { DomSanitizer } from '@angular/platform-browser';

constructor(private sanitizer: DomSanitizer) {}

getSafeHtml(html: string) {
  return this.sanitizer.sanitize(SecurityContext.HTML, html);
}

// Good - Use environment variables for API keys
const apiKey = environment.apiKey;
```

---

## 🐛 Reporting Issues

### Before Creating an Issue

1. Search existing issues to avoid duplicates
2. Check the [Troubleshooting Guide](./TROUBLESHOOTING.md)
3. Verify you're using the latest version

### Issue Template

```markdown
## 🐛 Bug Description
Clear and concise description of the bug

## 📋 Steps to Reproduce
1. Go to '...'
2. Click on '...'
3. Scroll down to '...'
4. See error

## ✅ Expected Behavior
What you expected to happen

## ❌ Actual Behavior
What actually happened

## 🖼️ Screenshots
If applicable, add screenshots

## 🌍 Environment
- OS: [e.g., Windows 11]
- Browser: [e.g., Chrome 120]
- Node Version: [e.g., 18.17.0]
- .NET Version: [e.g., 8.0.0]

## 📝 Additional Context
Any other relevant information
```

---

## 🎉 Recognition

Contributors will be recognized in:

- ✨ **CONTRIBUTORS.md** - List of all contributors
- 📝 **Release Notes** - Mentioned in release notes
- 🏆 **README.md** - Featured contributors section

---

## 📞 Getting Help

### Resources

- 📖 [Documentation](./README.md)
- 🐛 [Issue Tracker](https://github.com/Mostafa-SAID7/Naar-Noor/issues)
- 💬 [Discussions](https://github.com/Mostafa-SAID7/Naar-Noor/discussions)

### Contact

- Open an issue for bugs or feature requests
- Start a discussion for questions or ideas
- Review existing documentation before asking

---

## 📄 License

By contributing to Naar & Noor, you agree that your contributions will be licensed under the same license as the project.

---

## 🙏 Thank You!

Thank you for taking the time to contribute to **Naar & Noor**! Your contributions help make this project better for everyone.

**Happy Coding! 🚀**


## 🧪 Testing Requirements

### Before Submitting PR

All contributions must include appropriate tests:

#### Backend Tests

For any backend changes (C# / .NET):

1. **Unit Tests** - Test individual methods
   ```bash
   cd api-server
   dotnet test
   ```

2. **Property Tests** - Use FsCheck for complex logic (100 iterations)
   ```csharp
   [Property(MaxTest = 100)]
   public async Task YourPropertyTest(string input)
   {
       // Test implementation
   }
   ```

3. **Coverage Requirements**
   - Domain: 85%+ coverage
   - Application: 82%+ coverage
   - Infrastructure: 78%+ coverage
   - API: 80%+ coverage

   Verify coverage locally:
   ```bash
   python scripts/validate-coverage.py --backend-dir api-server/tests
   ```

#### Frontend Tests

For any frontend changes (Angular / TypeScript):

1. **Unit Tests** - Test services and components
   ```bash
   cd naar-noor
   npm run test:ci
   ```

2. **Coverage Requirements**
   - Services: 80%+ coverage
   - Components: 75%+ coverage

3. **E2E Tests** - For new user workflows
   ```bash
   npx cypress run --spec "cypress/e2e/your-feature.cy.ts"
   ```

### Running Tests Locally

```bash
# Backend - all tests
cd api-server && dotnet test

# Backend - specific layer
dotnet test --filter "Category=Domain"

# Frontend - unit tests
cd naar-noor && npm run test:ci

# Frontend - E2E tests
npx cypress run

# Check coverage
python scripts/validate-coverage.py --backend-dir api-server/tests
```

### Test-Driven Development (TDD)

We recommend TDD approach:

1. **Write test first** - Define expected behavior
2. **See test fail** - Verify test is valid
3. **Write minimum code** - Make test pass
4. **Refactor** - Clean up code
5. **Repeat** - For next feature

## 📋 Pull Request Process

1. **Create Feature Branch**
   ```bash
   git checkout -b feature/your-feature-name
   ```

2. **Make Changes**
   - Follow code style guide (see below)
   - Write tests for all changes
   - Update relevant documentation
   - Add comments to complex logic

3. **Run Pre-Commit Checks**
   ```bash
   # Backend
   cd api-server && dotnet test && dotnet build

   # Frontend
   cd naar-noor && npm run test:ci

   # Coverage validation
   python scripts/validate-coverage.py --backend-dir api-server/tests
   ```

4. **Commit with Clear Messages**
   ```bash
   git commit -m "feat: add user authentication"
   git commit -m "fix: resolve reservation date validation"
   git commit -m "docs: update API documentation"
   ```

5. **Push to Your Fork**
   ```bash
   git push origin feature/your-feature-name
   ```

6. **Create Pull Request**
   - Fill out PR template completely
   - Link related issues
   - Describe what you changed and why
   - Include screenshots/videos if UI changes

## 📏 Code Style Guide

### C# (.NET)

```csharp
// PascalCase for classes, methods, properties
public class ChefService
{
    // Private fields with underscore prefix
    private readonly IRepository<Chef> _repository;

    // Public methods
    public async Task<Chef> GetChefAsync(int id)
    {
        return await _repository.GetByIdAsync(id);
    }

    // Private methods
    private bool ValidateChef(Chef chef)
    {
        return !string.IsNullOrWhiteSpace(chef.Name);
    }
}
```

### TypeScript/Angular

```typescript
// PascalCase for classes
export class ChefService {
  // camelCase for private properties
  private _chefs: Chef[] = [];

  // camelCase for methods
  getChefs(): Observable<Chef[]> {
    return this.http.get<Chef[]>('/api/chefs');
  }

  // UPPER_CASE for constants
  private readonly MAX_CHEFS = 100;
}
```

## 🏗️ Project Structure

### Backend

```
api-server/
├── src/
│   ├── NaarNoor.Domain/          # Business rules
│   ├── NaarNoor.Application/     # Use cases
│   ├── NaarNoor.Infrastructure/  # Data access
│   └── NaarNoor.API/             # HTTP endpoints
└── tests/
    ├── NaarNoor.Domain.Tests/
    ├── NaarNoor.Application.Tests/
    ├── NaarNoor.Infrastructure.Tests/
    └── NaarNoor.API.Tests/
```

### Frontend

```
naar-noor/
├── src/
│   ├── app/
│   │   ├── components/           # Angular components
│   │   ├── services/             # HTTP & business logic
│   │   └── models/               # TypeScript interfaces
│   └── test.ts
├── cypress/                       # E2E tests
└── karma.conf.js                 # Test configuration
```

## 🔍 Code Review Checklist

When reviewing code, check:

- [ ] Tests included for all changes
- [ ] Coverage thresholds met
- [ ] Code style consistent
- [ ] No hardcoded values
- [ ] Error handling implemented
- [ ] Documentation updated
- [ ] Comments explain "why" not "what"
- [ ] No console.log() left in code
- [ ] Security best practices followed
- [ ] Performance considered

## 📝 Documentation

### Code Comments

```csharp
// ✅ Good - explains why
// Cache the chef list for 1 hour to reduce database queries
var chefs = await GetChefsFromCacheAsync();

// ❌ Bad - obvious from code
// Get chefs from cache
```

### Commit Messages

Follow [Conventional Commits](https://www.conventionalcommits.org/):

```
feat: add authentication to API
fix: resolve chef reservation date validation
docs: update API documentation
style: format code per guidelines
refactor: extract Chef validation logic
test: add property tests for pagination
```

## 🐛 Reporting Bugs

### Bug Report Template

- **Description** - What is broken?
- **Steps to Reproduce** - How to trigger the bug?
- **Expected Behavior** - What should happen?
- **Actual Behavior** - What happens instead?
- **Screenshots** - Visual proof (if applicable)
- **Environment** - OS, browser, versions
- **Additional Context** - Any other info?

## 💡 Feature Requests

### Feature Request Template

- **Problem** - What problem does this solve?
- **Proposed Solution** - How should it work?
- **Alternatives** - Other approaches considered?
- **Additional Context** - Why is this important?

## 📞 Getting Help

- **Questions** - Use GitHub Discussions
- **Issues** - Create GitHub Issues
- **Chat** - Community Discord (link TBD)

## ✨ Thank You!

Thank you for contributing to Naar & Noor! Your efforts help make this project better for everyone. We appreciate your time and effort!
