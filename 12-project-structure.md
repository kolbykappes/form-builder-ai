# Project Structure

## Overview

This document outlines the project structure for the form system, including directory organization, file naming conventions, and common utilities.

## Directory Structure

### 1. Source Directory
```
src/
├── app/                 # Next.js app directory
│   ├── api/            # API routes
│   ├── forms/          # Form pages
│   └── layout.tsx      # Root layout
├── components/         # React components
│   ├── forms/         # Form components
│   ├── fields/        # Field components
│   └── layout/        # Layout components
├── lib/               # Library code
│   ├── storage/       # Storage utilities
│   ├── email/         # Email utilities
│   └── api/           # API utilities
├── utils/             # Utility functions
│   ├── validation/    # Validation utilities
│   ├── form/          # Form utilities
│   └── common/        # Common utilities
├── types/             # TypeScript types
│   ├── form.ts        # Form types
│   ├── field.ts       # Field types
│   └── api.ts         # API types
├── config/            # Configuration files
│   ├── form.ts        # Form configuration
│   ├── storage.ts     # Storage configuration
│   └── email.ts       # Email configuration
└── data/              # Data files
    ├── forms/         # Form definitions
    └── templates/     # Email templates
```

### 2. Test Directory
```
tests/
├── unit/              # Unit tests
│   ├── components/    # Component tests
│   ├── utils/         # Utility tests
│   └── lib/           # Library tests
├── integration/       # Integration tests
│   ├── api/           # API tests
│   ├── forms/         # Form tests
│   └── storage/       # Storage tests
└── e2e/               # End-to-end tests
    ├── forms/         # Form tests
    └── workflows/     # Workflow tests
```

## File Naming Conventions

### 1. Component Files
```typescript
// PascalCase for components
FormField.tsx
FormSection.tsx
DynamicForm.tsx

// kebab-case for component styles
form-field.css
form-section.css
dynamic-form.css
```

### 2. Utility Files
```typescript
// camelCase for utilities
formUtils.ts
validationUtils.ts
storageUtils.ts

// kebab-case for utility styles
form-utils.css
validation-utils.css
storage-utils.css
```

### 3. Type Files
```typescript
// camelCase for types
formTypes.ts
fieldTypes.ts
apiTypes.ts

// Interface naming
interface FormConfig {}
interface FieldConfig {}
interface ApiConfig {}
```

## Common Utilities

### 1. Form Utilities
```typescript
// src/utils/form/formUtils.ts
export function createFormState(initialState: FormState): FormState {
  return {
    ...initialState,
    errors: {},
    touched: {},
    isValid: false
  };
}

export function updateFormState(
  state: FormState,
  updates: Partial<FormState>
): FormState {
  return {
    ...state,
    ...updates,
    isValid: validateFormState({ ...state, ...updates })
  };
}
```

### 2. API Utilities
```typescript
// src/utils/api/apiUtils.ts
export async function fetchApi(
  endpoint: string,
  options: RequestInit
): Promise<ApiResponse> {
  try {
    const response = await fetch(endpoint, {
      ...options,
      headers: {
        'Content-Type': 'application/json',
        ...options.headers
      }
    });

    if (!response.ok) {
      throw new ApiError(response.status, await response.text());
    }

    return response.json();
  } catch (error) {
    throw new ApiError(500, error.message);
  }
}
```

### 3. Storage Utilities
```typescript
// src/utils/storage/storageUtils.ts
export async function storeData(
  data: any,
  config: StorageConfig
): Promise<void> {
  try {
    const preparedData = prepareDataForStorage(data);
    await storageAdapter.store(preparedData, config);
  } catch (error) {
    throw new StorageError(error.message);
  }
}

export async function retrieveData(
  key: string,
  config: StorageConfig
): Promise<any> {
  try {
    const data = await storageAdapter.retrieve(key, config);
    return prepareDataForUse(data);
  } catch (error) {
    throw new StorageError(error.message);
  }
}
```

## Development Workflow

### 1. Local Development
```bash
# Install dependencies
npm install

# Start development server
npm run dev

# Run tests
npm run test

# Build for production
npm run build
```

### 2. Code Organization
```typescript
// 1. Import order
import React from 'react';
import { useForm } from 'react-hook-form';
import { zodResolver } from '@hookform/resolvers/zod';

// 2. Component structure
export function Component() {
  // Hooks
  const [state, setState] = useState();

  // Handlers
  const handleSubmit = useCallback(() => {
    // Implementation
  }, []);

  // Render
  return (
    // JSX
  );
}
```

### 3. Debugging
```typescript
// 1. Logging
console.log('Debug:', { state, props });

// 2. Error boundaries
class ErrorBoundary extends React.Component {
  componentDidCatch(error: Error, errorInfo: React.ErrorInfo) {
    console.error('Error:', error, errorInfo);
  }
}

// 3. Development tools
if (process.env.NODE_ENV === 'development') {
  // Development-only code
}
```

## Best Practices

1. **Code Organization**
   - Follow directory structure
   - Use consistent naming
   - Group related code

2. **Development Process**
   - Use version control
   - Follow coding standards
   - Document changes

3. **Maintenance**
   - Keep code clean
   - Update dependencies
   - Review regularly

## Implementation Guidelines

1. **Setup Project**
   - Create directory structure
   - Set up development environment
   - Configure tools

2. **Develop Features**
   - Follow structure
   - Use utilities
   - Test thoroughly

3. **Maintain Project**
   - Update documentation
   - Review code
   - Optimize performance 