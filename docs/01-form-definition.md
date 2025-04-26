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

// Layout specifications for fields
type FieldLayout = {
  width: 'full' | 'half';  // Determines if field spans full width or half width
  order?: number;          // Optional order in the layout
};

// Style overrides for specific fields
type FieldStyle = {
  labelStyle?: string;     // Custom label styling
  inputStyle?: string;     // Custom input styling
  containerStyle?: string; // Custom container styling
};

interface FieldDefinition {
  name: string;
  type: FieldType;
  label: string;
  required: boolean;
  placeholder?: string;    // Placeholder text for the field
  layout?: FieldLayout;    // Layout specifications
  style?: FieldStyle;      // Style overrides
  validation?: {
    type: 'email' | 'phone' | 'url' | 'custom';
    pattern?: string;
    message?: string;
  };
  options?: string[];      // For select, checkbox, radio fields
  description?: string;    // Helper text below the field
}

// Section styling specifications
interface SectionStyle {
  title?: {
    color?: string;       // Title color override
    size?: string;        // Title size override
  };
  container?: {
    padding?: string;     // Section padding override
    background?: string;  // Section background override
    border?: string;      // Section border override
  };
}

// Section definition with styling
interface Section {
  name: string;
  label: string;
  description?: string;
  style?: SectionStyle;
  fields: FieldDefinition[];
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
// Required component hierarchy with styling
<DynamicForm className="min-h-screen bg-[#f8f9ff] py-16">
  <FormProvider {...methods}>
    <form className="mx-auto max-w-3xl px-4 sm:px-6 lg:px-8">
      <FormSection className="overflow-hidden rounded-xl bg-white shadow ring-1 ring-gray-900/5">
        <FormField className="block w-full rounded-lg py-2 px-3" />
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

### Layout Requirements

1. **Form Container**
   - Maximum width: 48rem (max-w-3xl)
   - Centered on page
   - White background
   - Rounded corners
   - Subtle shadow
   - Border ring

2. **Section Layout**
   - Consistent padding (px-8 py-6)
   - Dividers between sections
   - Two-column grid for fields
   - Proper spacing between fields

3. **Field Layout**
   - Full-width fields:
     - Textareas
     - Radio groups
     - Checkbox groups
     - File inputs
   - Half-width fields:
     - Text inputs
     - Email inputs
     - Number inputs
     - Select inputs
   - Consistent height for all inputs
   - Proper label alignment
   - Error message placement

### Visual Requirements

1. **Colors**
   - Primary: #6366f1 (Indigo)
   - Hover: #5558e6
   - Focus: #4f46e5
   - Error: #ef4444
   - Text: #111827 (Gray 900)
   - Border: #d1d5db (Gray 300)

2. **Typography**
   - Form title: 2.25rem, bold
   - Section titles: 1rem, semibold
   - Labels: 0.875rem, medium
   - Input text: 0.875rem, regular
   - Error text: 0.875rem, regular, red

3. **Spacing**
   - Between sections: 2rem
   - Between fields: 1.5rem
   - Label to input: 0.5rem
   - Input to error: 0.5rem

4. **Interactive States**
   - Hover: Subtle border darkening
   - Focus: 2px ring, offset
   - Error: Red border, icon, message
   - Loading: Spinner animation
   - Disabled: Reduced opacity

### Example Template

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
        label: "Personal Information",
        style: {
          title: {
            color: "text-[#4f46e5]",
            size: "text-base font-semibold"
          }
        },
        fields: [
          {
            name: "firstName",
            type: "text",
            label: "First Name",
            required: true,
            placeholder: "Enter first name",
            layout: {
              width: "half"
            }
          },
          {
            name: "lastName",
            type: "text",
            label: "Last Name",
            required: true,
            placeholder: "Enter last name",
            layout: {
              width: "half"
            }
          },
          {
            name: "email",
            type: "email",
            label: "Email",
            required: true,
            placeholder: "Enter email address",
            layout: {
              width: "half"
            },
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
            placeholder: "123-456-7890",
            layout: {
              width: "half"
            },
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
- ✅ Follow layout specifications
- ✅ Implement all required states
- ✅ Include proper accessibility attributes

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