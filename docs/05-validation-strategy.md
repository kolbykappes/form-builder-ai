# Validation Strategy

## Overview

This document outlines the validation strategy for form fields, including built-in validation rules, custom validation patterns, and error handling.

## Critical Requirements

### 1. Form Context Setup
```tsx
// REQUIRED: Wrap all form components with FormProvider
import { FormProvider, useForm } from 'react-hook-form';
import { zodResolver } from '@hookform/resolvers/zod';

export const DynamicForm = () => {
  const methods = useForm<FormData>({
    resolver: zodResolver(schema)
  });
  
  return (
    <FormProvider {...methods}>
      <form onSubmit={methods.handleSubmit(onSubmit)}>
        {/* Form fields here */}
      </form>
    </FormProvider>
  );
};
```

### 2. Type-Safe Field Paths
```tsx
// Define strict field paths for type safety
type FormFieldPath = 
  | `applicantInfo.${keyof FormData['applicantInfo']}`
  | `homeInfo.${keyof FormData['homeInfo']}`
  | `petExperience.${keyof FormData['petExperience']}`;

// Use in form fields
const fieldName = `${sectionName}.${field.name}` as FormFieldPath;
```

## Validation Types

### 1. Built-in Validation
```typescript
interface BuiltInValidation {
  required?: boolean;
  minLength?: number;
  maxLength?: number;
  pattern?: string;
  min?: number;
  max?: number;
  email?: boolean;
  url?: boolean;
}
```

### 2. Custom Validation
```typescript
interface CustomValidation {
  validate: (value: any) => boolean | Promise<boolean>;
  message: string;
}
```

## Field-Specific Validation

### 1. Number Fields
```typescript
// Schema validation
const schema = z.object({
  homeInfo: z.object({
    householdMembers: z.number().min(1, "At least one adult required"),
    children: z.number().min(0, "Cannot be negative")
  })
});

// Component validation
case 'number':
  return (
    <Controller
      name={fieldName}
      control={control}
      rules={{ required: field.required }}
      render={({ field: { onChange, value } }) => (
        <input
          type="number"
          onChange={(e) => {
            const val = e.target.value;
            onChange(val === '' ? '' : Number(val));
          }}
          value={value ?? ''}
          min={0}
        />
      )}
    />
  );
```

### 2. Phone Fields
```typescript
// Schema validation
const schema = z.object({
  applicantInfo: z.object({
    phone: z.string().regex(/^\d{3}-\d{3}-\d{4}$/, "Invalid phone format")
  })
});

// Component validation
const formatPhoneNumber = (value: string) => {
  const numbers = value.replace(/\D/g, '');
  if (numbers.length <= 3) return numbers;
  if (numbers.length <= 6) return `${numbers.slice(0, 3)}-${numbers.slice(3)}`;
  return `${numbers.slice(0, 3)}-${numbers.slice(3, 6)}-${numbers.slice(6, 10)}`;
};

case 'text':
  if (field.validation?.type === 'phone') {
    return (
      <Controller
        name={fieldName}
        control={control}
        rules={{ required: field.required }}
        render={({ field: { onChange, value } }) => (
          <input
            type="tel"
            inputMode="numeric"
            placeholder="123-456-7890"
            onChange={(e) => {
              const formatted = formatPhoneNumber(e.target.value);
              onChange(formatted);
            }}
            onPaste={(e) => {
              e.preventDefault();
              const pastedText = e.clipboardData.getData('text');
              const formatted = formatPhoneNumber(pastedText);
              onChange(formatted);
            }}
            value={value || ''}
            maxLength={12}
          />
        )}
      />
    );
  }
```

## Error Handling

### 1. Type-Safe Error Handling
```typescript
interface ValidationError {
  field: string;
  message: string;
  code?: string;
}

// In form components
const { control, formState: { errors } } = useFormContext<FormData>();
const sectionErrors = errors[sectionName] as Record<string, { message: string }> | undefined;
const error = sectionErrors?.[field.name];
```

### 2. Error Display
```typescript
export function ErrorDisplay({ error }: { error: ValidationError }) {
  return (
    <div className="error-message">
      <span>{error.message}</span>
    </div>
  );
}
```

## Best Practices

1. **Form Context**
   - ❌ Never use form hooks without proper context
   - ✅ Always wrap form components with FormProvider
   - ✅ Use type-safe form context with proper generics

2. **Type Safety**
   - ❌ Avoid generic string types for field paths
   - ✅ Use template literal types for field paths
   - ✅ Properly type error objects and validation rules

3. **Value Handling**
   - ❌ Don't trust input values to be correct type
   - ✅ Always convert and validate values
   - ✅ Handle empty states appropriately

4. **Validation Rules**
   - Validate on both client and server
   - Provide clear error messages
   - Use appropriate validation types for each field

5. **Error Handling**
   - Handle all validation errors
   - Display errors clearly
   - Allow error recovery
   - Use type-safe error handling

6. **Performance**
   - Validate efficiently
   - Debounce validation where appropriate
   - Cache validation results when possible

## Implementation Guidelines

1. **Setup Validation**
   - Define validation rules in schema
   - Create type-safe validation functions
   - Set up proper form context

2. **Implement Validation**
   - Add field-specific validation
   - Implement form-level validation
   - Add error display components

3. **Test Validation**
   - Test all validation rules
   - Test error handling
   - Test edge cases
   - Test type conversion
   - Test form context availability 