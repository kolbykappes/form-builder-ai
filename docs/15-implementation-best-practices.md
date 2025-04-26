# Implementation Best Practices

## Critical Setup Requirements

### 1. Form Provider Setup (REQUIRED)
```tsx
// ALWAYS wrap form components with FormProvider
import { FormProvider, useForm } from 'react-hook-form';

export const DynamicForm = () => {
  const methods = useForm<FormData>();
  
  return (
    <FormProvider {...methods}>
      {/* All form fields must be children of FormProvider */}
      <form onSubmit={methods.handleSubmit(onSubmit)}>
        {/* Form fields here */}
      </form>
    </FormProvider>
  );
};
```

### 2. Type Safety Requirements
```tsx
// Define strict field paths for type safety
type FormFieldPath = 
  | `applicantInfo.${keyof FormData['applicantInfo']}`
  | `homeInfo.${keyof FormData['homeInfo']}`
  | `petExperience.${keyof FormData['petExperience']}`;

// Ensure error typing
type FormErrors = {
  [K in keyof FormData]: Record<string, { message: string }>;
};
```

### 3. Field Type Handling
```tsx
// Number fields
case 'number':
  return (
    <Controller
      name={fieldName}
      render={({ field }) => (
        <input
          type="number"
          onChange={(e) => {
            const val = e.target.value;
            field.onChange(val === '' ? '' : Number(val));
          }}
          value={field.value ?? ''}
          min={0}
        />
      )}
    />
  );

// Phone fields
case 'phone':
  return (
    <Controller
      name={fieldName}
      render={({ field }) => (
        <input
          type="tel"
          inputMode="numeric"
          placeholder="123-456-7890"
          onChange={(e) => {
            const formatted = formatPhoneNumber(e.target.value);
            field.onChange(formatted);
          }}
          onPaste={(e) => {
            e.preventDefault();
            const pastedText = e.clipboardData.getData('text');
            const formatted = formatPhoneNumber(pastedText);
            field.onChange(formatted);
          }}
          value={field.value || ''}
          maxLength={12}
        />
      )}
    />
  );
```

### 4. Validation Schema Setup
```tsx
const schema = z.object({
  applicantInfo: z.object({
    phone: z.string().regex(/^\d{3}-\d{3}-\d{4}$/, "Invalid phone format"),
    // ... other fields
  }),
  homeInfo: z.object({
    householdMembers: z.number().min(1),
    children: z.number().min(0),
    // ... other fields
  })
});

const methods = useForm<FormData>({
  resolver: zodResolver(schema)
});
```

## Common Pitfalls to Avoid

1. **Context Usage**
   - ❌ Don't use `useFormContext` without `FormProvider`
   - ✅ Always wrap form components with `FormProvider`
   - ✅ Ensure `FormProvider` is at the root of your form component

2. **Type Safety**
   - ❌ Don't use generic string types for field paths
   - ✅ Use strictly typed field paths with template literals
   - ✅ Define proper types for form errors and validation

3. **Value Handling**
   - ❌ Don't assume input values are of correct type
   - ✅ Always convert and validate values appropriately
   - ✅ Handle empty values and edge cases

4. **Component Structure**
```tsx
// Recommended component hierarchy
<DynamicForm>
  <FormProvider>
    <FormSection>
      <FormField />
    </FormSection>
  </FormProvider>
</DynamicForm>
```

## Input Type Best Practices

1. **Number Fields**
   - Use `type="number"` with proper value conversion
   - Handle empty values correctly
   - Set appropriate min/max values
   - Convert string inputs to numbers

2. **Phone Fields**
   - Use `type="tel"` for better mobile support
   - Use `inputMode="numeric"` for appropriate keyboard
   - Implement automatic formatting
   - Handle paste events
   - Validate format (XXX-XXX-XXXX)

3. **Select Fields**
   - Provide default "Select an option" choice
   - Handle empty/unselected states
   - Validate required selections

4. **Radio/Checkbox Fields**
   - Handle array values properly
   - Maintain checked state correctly
   - Validate required selections

## Testing Requirements

1. Test form submission with various input types:
   - Numbers (valid, invalid, empty)
   - Phone numbers (valid, invalid formats, paste)
   - Required fields (filled, empty)
   - Validation messages

2. Test form context availability:
   - Component mounting
   - Error handling
   - Value updates

3. Test input formatting:
   - Phone number formatting
   - Number conversion
   - Empty value handling

4. Test validation:
   - Required fields
   - Format validation
   - Custom validation rules 