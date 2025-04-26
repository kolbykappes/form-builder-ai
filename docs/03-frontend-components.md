# Frontend Components Documentation

## Component Architecture

### DynamicForm
The main form container component that manages form state and submission.

```tsx
// components/DynamicForm.tsx
"use client";

const DynamicForm = ({ config, onSubmit }) => {
  return (
    <div className="min-h-screen bg-[#f8f9ff] py-16">
      <div className="mx-auto max-w-3xl px-4 sm:px-6 lg:px-8">
        {/* Form content */}
      </div>
    </div>
  );
};
```

**Styling Requirements:**
- Full-height background with custom color
- Centered content with maximum width
- Responsive padding system
- Proper form provider context

### FormSection
Renders a logical group of form fields with a title and description.

```tsx
// components/FormSection.tsx
"use client";

const FormSection = ({ title, description, children }) => {
  return (
    <div className="overflow-hidden rounded-xl bg-white shadow ring-1 ring-gray-900/5">
      <div className="px-8 py-6">
        <h2 className="text-base font-semibold text-gray-900">{title}</h2>
        {description && (
          <p className="mt-1 text-sm text-gray-500">{description}</p>
        )}
        <div className="mt-6 grid grid-cols-1 gap-x-4 gap-y-6 sm:grid-cols-2">
          {children}
        </div>
      </div>
    </div>
  );
};
```

**Styling Requirements:**
- White background with subtle shadow
- Rounded corners with border ring
- Consistent internal padding
- Two-column responsive grid for fields
- Proper typography hierarchy
- Semantic spacing between elements

### FormField
Renders individual form fields with labels, inputs, and error states.

```tsx
// components/FormField.tsx
"use client";

const FormField = ({ label, error, children }) => {
  return (
    <div className="relative">
      <label className="block text-sm font-medium text-gray-900">
        {label}
      </label>
      <div className="mt-2">
        {children}
      </div>
      {error && (
        <p className="mt-2 text-sm text-red-600">{error}</p>
      )}
    </div>
  );
};
```

**Styling Requirements:**
- Consistent label styling
- Proper input spacing
- Error message styling
- Accessible focus states
- Responsive width handling

## Input Types

### Text Input
```tsx
<input
  type="text"
  className="block w-full rounded-lg border-gray-300 py-2 px-3 shadow-sm focus:border-indigo-500 focus:ring-indigo-500 sm:text-sm"
/>
```

### Select Input
```tsx
<select
  className="block w-full rounded-lg border-gray-300 py-2 pl-3 pr-10 shadow-sm focus:border-indigo-500 focus:ring-indigo-500 sm:text-sm"
>
  {options.map((option) => (
    <option key={option.value} value={option.value}>
      {option.label}
    </option>
  ))}
</select>
```

### Checkbox Input
```tsx
<div className="relative flex items-start">
  <div className="flex h-5 items-center">
    <input
      type="checkbox"
      className="h-4 w-4 rounded border-gray-300 text-indigo-600 focus:ring-indigo-500"
    />
  </div>
  <div className="ml-3 text-sm">
    <label className="font-medium text-gray-700">{label}</label>
    {description && (
      <p className="text-gray-500">{description}</p>
    )}
  </div>
</div>
```

### Radio Input
```tsx
<div className="space-y-4">
  {options.map((option) => (
    <div key={option.value} className="flex items-center">
      <input
        type="radio"
        className="h-4 w-4 border-gray-300 text-indigo-600 focus:ring-indigo-500"
      />
      <label className="ml-3 block text-sm font-medium text-gray-700">
        {option.label}
      </label>
    </div>
  ))}
</div>
```

## Button Styles

### Primary Button
```tsx
<button
  type="submit"
  className="inline-flex justify-center rounded-lg bg-indigo-600 px-4 py-2.5 text-sm font-semibold text-white shadow-sm hover:bg-indigo-500 focus-visible:outline focus-visible:outline-2 focus-visible:outline-offset-2 focus-visible:outline-indigo-600"
>
  {children}
</button>
```

### Secondary Button
```tsx
<button
  type="button"
  className="inline-flex justify-center rounded-lg bg-white px-4 py-2.5 text-sm font-semibold text-gray-900 shadow-sm ring-1 ring-inset ring-gray-300 hover:bg-gray-50"
>
  {children}
</button>
```

## Loading States

### Loading Spinner
```tsx
<div className="inline-flex items-center">
  <svg
    className="animate-spin -ml-1 mr-3 h-5 w-5 text-white"
    xmlns="http://www.w3.org/2000/svg"
    fill="none"
    viewBox="0 0 24 24"
  >
    <circle
      className="opacity-25"
      cx="12"
      cy="12"
      r="10"
      stroke="currentColor"
      strokeWidth="4"
    />
    <path
      className="opacity-75"
      fill="currentColor"
      d="M4 12a8 8 0 018-8V0C5.373 0 0 5.373 0 12h4zm2 5.291A7.962 7.962 0 014 12H0c0 3.042 1.135 5.824 3 7.938l3-2.647z"
    />
  </svg>
  Loading...
</div>
```

## Error States

### Field Error
```tsx
<div className="relative">
  <input
    type="text"
    className="block w-full rounded-lg border-red-300 pr-10 text-red-900 placeholder-red-300 focus:border-red-500 focus:ring-red-500 sm:text-sm"
  />
  <div className="pointer-events-none absolute inset-y-0 right-0 flex items-center pr-3">
    <ExclamationCircleIcon
      className="h-5 w-5 text-red-500"
      aria-hidden="true"
    />
  </div>
  <p className="mt-2 text-sm text-red-600">{error}</p>
</div>
```

## Component Props

### DynamicForm Props
```typescript
interface DynamicFormProps {
  config: FormConfig;                // Form configuration object
  onSubmit: (data: any) => Promise<void>; // Form submission handler
  className?: string;               // Optional class name for styling
  initialValues?: Record<string, any>; // Optional initial form values
}
```

### FormSection Props
```typescript
interface FormSectionProps {
  title: string;                    // Section title
  description?: string;             // Optional section description
  children: React.ReactNode;        // Section content
  className?: string;               // Optional class name for styling
  style?: SectionStyle;             // Optional style overrides
}
```

### FormField Props
```typescript
interface FormFieldProps {
  name: string;                     // Field name
  label: string;                    // Field label
  type: FieldType;                  // Field type
  required?: boolean;               // Whether field is required
  error?: string;                   // Error message
  description?: string;             // Helper text
  className?: string;               // Optional class name
  style?: FieldStyle;               // Optional style overrides
}
```

## Accessibility Guidelines

1. **Proper ARIA Labels**
   - Use `aria-label` for non-visible labels
   - Use `aria-describedby` for error messages
   - Use `aria-invalid` for invalid fields

2. **Focus Management**
   - Visible focus indicators
   - Proper tab order
   - Skip links for navigation

3. **Error Announcements**
   - Use `role="alert"` for error messages
   - Provide clear error descriptions
   - Use proper ARIA live regions

## Responsive Design

1. **Breakpoints**
   - sm: 640px
   - md: 768px
   - lg: 1024px
   - xl: 1280px
   - 2xl: 1536px

2. **Grid System**
   - Single column on mobile
   - Two columns on tablet and above
   - Proper spacing adjustments
   - Flexible width handling

3. **Typography**
   - Responsive font sizes
   - Line height adjustments
   - Proper text wrapping

## Best Practices

1. **Component Organization**
   - Keep components focused and single-responsibility
   - Use composition over inheritance
   - Maintain consistent prop interfaces
   - Document component APIs

2. **State Management**
   - Use form context for form state
   - Handle loading states properly
   - Manage error states effectively
   - Implement proper validation

3. **Performance**
   - Optimize re-renders
   - Lazy load components when needed
   - Use proper memoization
   - Implement proper error boundaries

4. **Type Safety**
   - Use TypeScript for all components
   - Define proper interfaces
   - Validate props with PropTypes
   - Use strict type checking 