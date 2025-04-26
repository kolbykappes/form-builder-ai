# Implementation Guide

## Overview

This guide provides step-by-step instructions for implementing a form application from the form template. It serves as the bridge between the template definition and the actual implementation.

## Setup Steps

> **Dependency Version Note**: The dependencies and their versions listed below are current as of April 2025. Future readers (including AI assistants) should verify the latest versions from:
> - npm: https://www.npmjs.com/
> - React Hook Form: https://www.npmjs.com/package/react-hook-form
> - Zod: https://www.npmjs.com/package/zod
> - Tailwind CSS: https://www.npmjs.com/package/tailwindcss

### 1. Install Required Dependencies

```bash
# Core dependencies
npm install @hookform/resolvers zod react-hook-form

# Styling dependencies
npm install -D tailwindcss@latest postcss autoprefixer @tailwindcss/forms
```

> **Configuration Note**: The following configuration examples are current as of April 2025. Future readers should verify the latest configuration options from the official documentation of each tool.

### 2. Configure Tailwind CSS

```javascript
// tailwind.config.js
module.exports = {
  content: [
    './src/pages/**/*.{js,ts,jsx,tsx,mdx}',
    './src/components/**/*.{js,ts,jsx,tsx,mdx}',
    './src/app/**/*.{js,ts,jsx,tsx,mdx}',
  ],
  theme: {
    extend: {
      colors: {
        primary: {
          50: '#f0f9ff',
          100: '#e0f2fe',
          200: '#bae6fd',
          300: '#7dd3fc',
          400: '#38bdf8',
          500: '#0ea5e9',
          600: '#0284c7',
          700: '#0369a1',
          800: '#075985',
          900: '#0c4a6e',
        },
        success: {
          50: '#f0fdf4',
          500: '#22c55e',
          700: '#15803d',
        },
        error: {
          50: '#fef2f2',
          500: '#ef4444',
          700: '#b91c1c',
        },
        neutral: {
          50: '#fafafa',
          100: '#f5f5f5',
          200: '#e5e5e5',
          300: '#d4d4d4',
          400: '#a3a3a3',
          500: '#737373',
          600: '#525252',
          700: '#404040',
          800: '#262626',
          900: '#171717',
        }
      },
      spacing: {
        'form-spacing': '1.5rem',
        'section-spacing': '2.5rem',
      },
      borderRadius: {
        'form': '0.5rem',
        'input': '0.375rem',
      },
      animation: {
        'fade-in': 'fadeIn 0.3s ease-in-out',
        'slide-up': 'slideUp 0.4s ease-out',
        'shake': 'shake 0.5s ease-in-out',
      },
      keyframes: {
        fadeIn: {
          '0%': { opacity: '0' },
          '100%': { opacity: '1' },
        },
        slideUp: {
          '0%': { transform: 'translateY(10px)', opacity: '0' },
          '100%': { transform: 'translateY(0)', opacity: '1' },
        },
        shake: {
          '0%, 100%': { transform: 'translateX(0)' },
          '25%': { transform: 'translateX(-5px)' },
          '75%': { transform: 'translateX(5px)' },
        },
      },
      boxShadow: {
        'form': '0 1px 3px 0 rgb(0 0 0 / 0.1), 0 1px 2px -1px rgb(0 0 0 / 0.1)',
        'input': '0 1px 2px 0 rgb(0 0 0 / 0.05)',
      },
    },
  },
  plugins: [require('@tailwindcss/forms')],
}
```

### 3. Set Up Global Styles

```css
/* globals.css */
@tailwind base;
@tailwind components;
@tailwind utilities;

@layer base {
  body {
    @apply text-neutral-800 bg-neutral-50 min-h-screen;
  }

  h1 {
    @apply text-3xl font-bold text-neutral-900 mb-6;
  }

  h2 {
    @apply text-2xl font-semibold text-neutral-800 mb-4;
  }

  h3 {
    @apply text-xl font-medium text-neutral-700 mb-3;
  }
}

@layer components {
  .form-container {
    @apply max-w-3xl mx-auto p-6 bg-white rounded-form shadow-form animate-fade-in;
  }

  .form-section {
    @apply mb-section-spacing p-6 bg-neutral-50 rounded-form border border-neutral-200;
  }

  .form-field {
    @apply mb-form-spacing;
  }

  .form-label {
    @apply block text-sm font-medium text-neutral-700 mb-1;
  }

  .form-input {
    @apply w-full rounded-input border border-gray-300 shadow-input
           focus:border-primary-500 focus:ring-2 focus:ring-primary-200 focus:ring-opacity-50
           disabled:bg-neutral-100 disabled:cursor-not-allowed;
  }

  .form-select {
    @apply form-input;
  }

  .form-checkbox {
    @apply rounded text-primary-600 border-gray-300 shadow-sm
           focus:border-primary-500 focus:ring-2 focus:ring-primary-200 focus:ring-opacity-50;
  }

  .form-radio {
    @apply text-primary-600 border-gray-300 shadow-sm
           focus:border-primary-500 focus:ring-2 focus:ring-primary-200 focus:ring-opacity-50;
  }

  .form-error {
    @apply mt-1 text-sm text-error-500 animate-shake;
  }

  .form-helper {
    @apply mt-1 text-sm text-neutral-500;
  }

  .form-button {
    @apply px-4 py-2 rounded-input font-medium text-white bg-primary-600
           hover:bg-primary-700 focus:outline-none focus:ring-2 focus:ring-primary-500 focus:ring-offset-2
           disabled:opacity-50 disabled:cursor-not-allowed
           transition-colors duration-200;
  }

  .form-button-secondary {
    @apply form-button bg-neutral-600 hover:bg-neutral-700 focus:ring-neutral-500;
  }
}
```

## Component Implementation

### 1. DynamicForm Component

```tsx
export const DynamicForm: React.FC<DynamicFormProps> = ({ config, onSubmit }) => {
  const methods = useForm<FormData>({
    resolver: zodResolver(schema)
  });

  return (
    <FormProvider {...methods}>
      <form onSubmit={methods.handleSubmit(onSubmit)} className="form-container">
        <div className="mb-8">
          <h1>{config.name}</h1>
          <p className="form-helper">{config.description}</p>
        </div>

        {config.schema.fields.map((section) => (
          <FormSection
            key={section.name}
            section={section as FormSectionType}
          />
        ))}

        <div className="mt-8">
          <button
            type="submit"
            disabled={methods.formState.isSubmitting}
            className="form-button w-full"
          >
            {methods.formState.isSubmitting ? 'Submitting...' : 'Submit'}
          </button>
        </div>
      </form>
    </FormProvider>
  );
};
```

### 2. FormSection Component

```tsx
export const FormSection: React.FC<FormSectionProps> = ({ section }) => {
  return (
    <div className="form-section">
      <h2>{section.label}</h2>
      <div className="space-y-4">
        {section.fields.map((field) => (
          <FormField
            key={field.name}
            field={field}
            sectionName={section.name as keyof FormData}
          />
        ))}
      </div>
    </div>
  );
};
```

### 3. FormField Component

```tsx
export const FormField: React.FC<FormFieldProps> = ({ field, sectionName }) => {
  const { control, formState: { errors } } = useFormContext<FormData>();
  const fieldName = `${sectionName}.${field.name}` as FormFieldPath;
  const error = errors[sectionName]?.[field.name];

  return (
    <div className="form-field">
      <label className="form-label">
        {field.label}
        {field.required && <span className="text-error-500">*</span>}
      </label>
      <Controller
        name={fieldName}
        control={control}
        rules={{ required: field.required }}
        render={({ field: { onChange, value } }) => (
          <input
            type={field.type}
            onChange={onChange}
            value={value || ''}
            className="form-input"
          />
        )}
      />
      {error && (
        <p className="form-error">{error.message}</p>
      )}
    </div>
  );
};
```

## Best Practices

### Styling Guidelines

1. **Use Semantic Classes**
   - Prefer component-specific classes (e.g., `form-input`) over utility classes
   - Keep styling consistent across similar elements
   - Use the provided color palette for consistency

2. **Animation Usage**
   - Use `animate-fade-in` for component mounting
   - Use `animate-shake` for error feedback
   - Use `animate-slide-up` for elements entering viewport

3. **Spacing**
   - Use `form-spacing` for vertical spacing between fields
   - Use `section-spacing` for spacing between sections

### Form Validation

1. **Required Fields**
   - Mark required fields with red asterisk using `text-error-500`
   - Show validation errors with shake animation
   - Use consistent error message styling

### Accessibility

1. **Focus States**
   - All interactive elements have visible focus states
   - Use proper ARIA labels and roles
   - Ensure proper tab order

### Performance

1. **Component Optimization**
   - Use `Controller` from react-hook-form for controlled inputs
   - Implement proper form state management
   - Use proper typing for form data

### Error Handling

1. **Form Errors**
   - Display field-specific errors below inputs
   - Use shake animation for error feedback
   - Use semantic error colors

### Mobile Responsiveness

1. **Layout**
   - Form container adapts to screen size
   - Proper input sizing on mobile
   - Touch-friendly input elements

## Implementation Steps

### 1. Parse Form Template

```typescript
// src/utils/template-parser.ts
export function parseFormTemplate(template: FormTemplate): ParsedTemplate {
  return {
    config: parseFormConfig(template.form),
    schema: parseSchema(template.schema),
    layout: parseLayout(template.configuration.layout),
    integrations: parseIntegrations(template.integrations),
    security: parseSecurity(template.security)
  };
}
```

### 2. Generate Form Schema

```typescript
// src/utils/schema-generator.ts
export function generateFormSchema(parsedTemplate: ParsedTemplate): FormSchema {
  const { schema, config } = parsedTemplate;
  
  return {
    id: config.id,
    title: config.title,
    fields: schema.fields.map(field => ({
      name: field.name,
      type: field.type,
      validation: generateValidationRules(field.validation),
      options: field.options
    }))
  };
}
```

### 3. Create Form Components

```typescript
// src/components/forms/DynamicForm.tsx
export function DynamicForm({ schema }: { schema: FormSchema }) {
  const form = useForm({
    resolver: zodResolver(generateZodSchema(schema))
  });

  return (
    <FormProvider {...form}>
      <form onSubmit={form.handleSubmit(handleSubmit)}>
        {schema.fields.map(field => (
          <FormField
            key={field.name}
            field={field}
          />
        ))}
      </form>
    </FormProvider>
  );
}
```

### 4. Set Up Backend Processing

```typescript
// src/app/api/forms/submit/route.ts
export async function POST(request: Request) {
  const { formId, data } = await request.json();
  
  // 1. Validate submission
  const validationResult = validateSubmission(data, formId);
  if (!validationResult.success) {
    return handleValidationError(validationResult.errors);
  }

  // 2. Process submission
  const processingResult = await processSubmission(data, formId);
  if (!processingResult.success) {
    return handleProcessingError(processingResult.error);
  }

  // 3. Handle integrations
  await handleIntegrations(processingResult.data, formId);

  return new Response(JSON.stringify({ success: true }));
}
```

### 5. Implement Storage

```typescript
// src/lib/storage/index.ts
export async function storeSubmission(
  data: FormData,
  config: StorageConfig
): Promise<void> {
  // 1. Validate storage config
  validateStorageConfig(config);

  // 2. Prepare data
  const preparedData = prepareDataForStorage(data);

  // 3. Store data
  await storageAdapter.store(preparedData, config);
}
```

### 6. Set Up Email Service

```typescript
// src/lib/email/index.ts
export async function sendFormEmail(
  data: FormData,
  config: EmailConfig
): Promise<void> {
  // 1. Generate email content
  const content = generateEmailContent(data, config.template);

  // 2. Send email
  await emailService.send({
    to: config.recipients,
    from: config.from,
    subject: content.subject,
    html: content.body
  });
}
```

## Implementation Flow

1. **Template to Schema**
   - Parse form template
   - Generate form schema
   - Create validation rules

2. **Schema to Components**
   - Generate form components
   - Set up form state
   - Implement validation

3. **Components to Backend**
   - Create API routes
   - Set up request handling
   - Implement error handling

4. **Backend to Storage**
   - Configure storage
   - Implement data processing
   - Set up backups

5. **Storage to Integration**
   - Configure email service
   - Set up API integrations
   - Implement security measures

## Example Implementation

```typescript
// Example implementation using the template
async function implementForm(template: FormTemplate) {
  // 1. Parse template
  const parsedTemplate = parseFormTemplate(template);

  // 2. Generate schema
  const schema = generateFormSchema(parsedTemplate);

  // 3. Create form page
  const formPage = `
    // src/app/forms/[formId]/page.tsx
    export default function FormPage({ params }: { params: { formId: string } }) {
      return <DynamicForm schema={schema} />;
    }
  `;

  // 4. Set up API route
  const apiRoute = `
    // src/app/api/forms/submit/route.ts
    export async function POST(request: Request) {
      // Implementation
    }
  `;

  // 5. Configure storage
  const storageConfig = configureStorage(parsedTemplate.integrations.storage);

  // 6. Set up email service
  const emailConfig = configureEmail(parsedTemplate.integrations.email);

  return {
    formPage,
    apiRoute,
    storageConfig,
    emailConfig
  };
}
```

## Implementation Checklist

1. **Template Processing**
   - [ ] Parse form template
   - [ ] Validate template structure
   - [ ] Generate form schema

2. **Frontend Implementation**
   - [ ] Create form components
   - [ ] Implement form state
   - [ ] Add validation
   - [ ] Style components

3. **Backend Implementation**
   - [ ] Create API routes
   - [ ] Implement request handling
   - [ ] Add error handling
   - [ ] Set up logging

4. **Storage Implementation**
   - [ ] Configure storage
   - [ ] Implement data processing
   - [ ] Set up backups
   - [ ] Add encryption

5. **Integration Setup**
   - [ ] Configure email service
   - [ ] Set up API integrations
   - [ ] Implement security
   - [ ] Add monitoring

6. **Testing**
   - [ ] Add unit tests
   - [ ] Implement integration tests
   - [ ] Set up E2E tests
   - [ ] Test error handling

7. **Deployment**
   - [ ] Configure environment
   - [ ] Set up CI/CD
   - [ ] Configure monitoring
   - [ ] Add documentation 