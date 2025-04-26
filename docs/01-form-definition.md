# Form Definition Template

## Basic Information

### Form Name
- Name: [Form Name]
- Description: [Brief description]
- Business Context: [Why this form exists]

## Data Schema

### Schema Version
- Version: 1.0
- Last Updated: [Date]

### Field Definitions
```typescript
// Field types must be strictly typed
type FieldType = 'text' | 'email' | 'number' | 'date' | 'select' | 'checkbox' | 'radio' | 'file';

interface FieldDefinition {
  name: string;
  type: FieldType;
  label: string;
  required: boolean;
  validation?: {
    type: 'email' | 'phone' | 'url' | 'custom';
    pattern?: string;
    message?: string;
  };
  options?: string[]; // For select, checkbox, radio fields
}

// Form data must use strict typing
type FormData = {
  [section: string]: {
    [field: string]: string | number | string[] | boolean;
  };
};

// Field paths must be strictly typed
type FormFieldPath = 
  | `sectionName.${keyof SectionData}`
  | `otherSection.${keyof OtherSectionData}`;
```

## Form Configuration

### Component Structure
```tsx
// Required component hierarchy
<DynamicForm>
  <FormProvider {...methods}>
    <form>
      <FormSection>
        <FormField />
      </FormSection>
    </form>
  </FormProvider>
</DynamicForm>
```

### Validation Schema
```typescript
// Zod schema for form validation
const schema = z.object({
  sectionName: z.object({
    numberField: z.number().min(0),
    phoneField: z.string().regex(/^\d{3}-\d{3}-\d{4}$/),
    emailField: z.string().email(),
    requiredField: z.string().min(1, "This field is required")
  })
});

// Form setup with validation
const methods = useForm<FormData>({
  resolver: zodResolver(schema),
  defaultValues: {
    sectionName: {
      numberField: 0,
      phoneField: '',
      emailField: '',
      requiredField: ''
    }
  }
});
```

### Layout
- Sections: [Number of sections]
- Fields per section: [Number]
- Submit button: [Text]

### Submit Actions
- Store submission: [Yes/No]
- Send email: [Yes/No]
- Redirect: [URL]

### Messages
- Success: [Message]
- Error: [Message]

## Integration Points

### Storage
- Type: [file/database]
- Location: [path/connection string]
- Encryption: [Yes/No]

### Email
- Service: [provider]
- Template: [template name]
- Recipients: [email addresses]

### API
- Endpoints: [list of endpoints]
- Authentication: [type]

## Security Requirements

### Authentication
- Required: [Yes/No]
- Type: [method]
- Scope: [access level]

### Data Protection
- Encryption: [Yes/No]
- Retention: [period]
- Access: [roles]

## Example Template

### Employee Onboarding Form
```typescript
{
  name: "Employee Onboarding",
  description: "Collect new employee information",
  businessContext: "Streamline employee onboarding process",
  
  schema: {
    fields: [
      {
        name: "personalInfo",
        type: "section",
        fields: [
          {
            name: "firstName",
            type: "text",
            label: "First Name",
            required: true
          },
          {
            name: "lastName",
            type: "text",
            label: "Last Name",
            required: true
          },
          {
            name: "email",
            type: "email",
            label: "Email",
            required: true,
            validation: {
              type: "email",
              message: "Please enter a valid email address"
            }
          },
          {
            name: "phone",
            type: "text",
            label: "Phone Number",
            required: true,
            validation: {
              type: "phone",
              pattern: "^[0-9]{3}-[0-9]{3}-[0-9]{4}$",
              message: "Please enter a valid phone number (format: 123-456-7890)"
            }
          }
        ]
      }
    ]
  }
}
```

## Implementation Requirements

### 1. Form Context
- ✅ Always wrap form components with FormProvider
- ✅ Use type-safe form context with proper generics
- ❌ Never use form hooks without proper context

### 2. Type Safety
- ✅ Use strictly typed field paths
- ✅ Define proper types for form data
- ✅ Use proper typing for validation schema
- ❌ Avoid generic string types

### 3. Field Handling
- ✅ Convert and validate all input values
- ✅ Handle empty states appropriately
- ✅ Implement proper formatting (e.g., phone numbers)
- ❌ Don't trust input types without validation

### 4. Error Handling
- ✅ Use type-safe error handling
- ✅ Display clear error messages
- ✅ Handle all validation states
- ❌ Don't ignore edge cases

## Usage Instructions

1. **Copy Template**
   - Copy this template
   - Fill in your form details
   - Save as [form-name].template.ts

2. **Define Form**
   - Add all required fields
   - Specify validation rules
   - Configure integrations

3. **Use with AI**
   - Share template with AI
   - Request implementation
   - Review generated code

4. **Implement Form**
   - Follow generated code
   - Test thoroughly
   - Deploy when ready 