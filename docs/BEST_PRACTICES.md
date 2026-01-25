
```markdown
# Best Practices Guide - FULLSTACK_NEST_ENCORE_NEXT_RSBUILD

## 📚 Table of Contents
- [Git Workflow](#git-workflow)
- [Commit Messages](#commit-messages)
- [Code Style](#code-style)
- [Testing](#testing)
- [Security](#security)
- [Documentation](#documentation)
- [Performance](#performance)

---

## Git Workflow

### Branch Naming Convention
```
feature/descriptive-name    # New features
bugfix/issue-description    # Bug fixes
hotfix/critical-fix         # Production hotfixes
release/v1.0.0              # Release preparation
docs/update-readme          # Documentation updates
```

### Commit Workflow
```bash
# 1. Create feature branch
git checkout develop
git pull origin develop
git checkout -b feature/my-feature

# 2. Make changes and commit
git add .
git commit -m "feat(scope): description"

# 3. Keep updated with develop
git fetch origin
git rebase origin/develop

# 4. Push and create PR
git push origin feature/my-feature
```

### Git Hooks
- ✅ `commit-msg`: Validates conventional commit format  
- ✅ `pre-commit`: Runs linting, formatting, security checks  
- ✅ `pre-push`: Prevents direct push to main/develop  

---

## Commit Messages

### Conventional Commits Format
```
type(scope): subject

body (optional)

footer (optional)
```

### Types
| Type      | Description       | Example                          |
|-----------|-------------------|----------------------------------|
| `feat`    | New feature       | `feat(auth): add Better Auth`    |
| `fix`     | Bug fix           | `fix(api): resolve CORS issue`   |
| `docs`    | Documentation     | `docs: update README`            |
| `style`   | Code style        | `style(frontend): format code`   |
| `refactor`| Code refactoring  | `refactor(db): optimize queries` |
| `perf`    | Performance       | `perf(api): add Redis caching`   |
| `test`    | Tests             | `test(auth): add login tests`    |
| `build`   | Build system      | `build: update dependencies`     |
| `ci`      | CI configuration  | `ci: add GitHub Actions`         |
| `chore`   | Maintenance       | `chore: cleanup old files`       |

---

## Code Style

### TypeScript
- **Formatting**: Prettier (auto-formatted on commit)
- **Naming Conventions**:
  - PascalCase → clases, interfaces
  - camelCase → variables, funciones
  - UPPER_SNAKE_CASE → constantes

### Imports Order
```typescript
// 1. External libraries
import { Module } from '@nestjs/common';
import { useQuery } from '@tanstack/react-query';

// 2. Internal modules
import { AuthService } from '@/backend/auth';
import { api } from '@/shared/api';

// 3. Types
import type { User } from '@/types';

// 4. Styles/assets
import './styles.css';
```

---

## Testing

### Frontend (Next.js + Rsbuild + Bun)
```bash
cd services/frontend
bun test                 # Run all tests
bun test --watch         # Watch mode
bun test --coverage      # With coverage
```

### Backend (NestJS + Encore.ts)
```bash
cd services/backend
bun test                 # Run backend unit tests
bun test --coverage      # Coverage reports
```

---

## Security

- **Never commit secrets** → usar `.env.example` y `.secrets/`  
- **Input validation** → DTOs y Zod/TypeScript types  
- **SQL Injection prevention** → usar Drizzle ORM  
- **Dependencies** → revisar con `bun audit`  

---

## Documentation

- Actualizar `README.md` al añadir features  
- Mantener `docs/` con guías técnicas y API examples  
- Incluir Postman/Insomnia collections en `/docs/api/`  

---

## Performance

### Frontend
- Lazy loading de rutas y componentes pesados  
- Memoization con `useMemo` y `React.memo`  

### Backend
- Indexes en DB con Drizzle migrations  
- Redis para cachear queries y sesiones  
- Vertical slicing para mantener independencia de features  

---

## Checklist for Pull Requests
- [ ] Code sigue guías de estilo  
- [ ] Tests pasan correctamente  
- [ ] Documentación actualizada  
- [ ] Commits siguen formato convencional  
- [ ] No hay `console.log` ni `TODO` en producción  
- [ ] Variables de entorno documentadas  
- [ ] Breaking changes documentados  

---

## Tools Configuration

### VS Code Extensions
- `dbaeumer.vscode-eslint`  
- `esbenp.prettier-vscode`  
- `ms-azuretools.vscode-docker`  
- `eamodio.gitlens`  
- `gruntfuggly.todo-tree`  

### VS Code Settings
```json
{
  "editor.formatOnSave": true,
  "editor.codeActionsOnSave": {
    "source.fixAll.eslint": true
  },
  "[typescript]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  }
}
```

---

## Getting Help
- **Docs**: revisa `docs/`  
- **Issues**: crea issue con template  
- **Code Reviews**: solicita revisión en PR  