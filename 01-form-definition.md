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
interface FieldDefinition {
  name: string;
  type: 'text' | 'email' | 'number' | 'date' | 'select' | 'checkbox' | 'radio' | 'file';
  label: string;
  required: boolean;
  validation?: {
    type: 'email' | 'phone' | 'url' | 'custom';
    pattern?: string;
    message?: string;
  };
  options?: string[]; // For select, checkbox, radio fields
}
```

## Form Configuration

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
          }
        ]
      }
    ]
  },
  
  configuration: {
    layout: {
      sections: 3,
      fieldsPerSection: 5
    },
    submitActions: {
      store: true,
      email: true,
      redirect: "/confirmation"
    }
  }
}
```

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