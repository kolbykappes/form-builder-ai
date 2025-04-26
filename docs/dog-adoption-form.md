# Form Definition Template

## Basic Information

### Form Name
- Name: Pet Adoption Application
- Description: Form to collect information from potential adopters seeking to adopt a dog
- Business Context: To streamline the dog adoption process and ensure good matches between adopters and dogs

## Data Schema

### Schema Version
- Version: 1.0
- Last Updated: April 26, 2025

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
- Sections: 5
- Fields per section: Variable (5-8)
- Submit button: "Submit Adoption Application"

### Submit Actions
- Store submission: Yes
- Send email: Yes
- Redirect: "/thank-you"

### Messages
- Success: "Thank you for your adoption application! We will review your information and contact you within 2 business days."
- Error: "There was an error submitting your application. Please try again or contact us directly."

## Integration Points

### Storage
- Type: database
- Location: "mongodb://adoption-db:27017/adoptions"
- Encryption: Yes

### Email
- Service: SendGrid
- Template: "adoption-application-receipt"
- Recipients: ["adoptions@rescueshelter.org", "{applicant.email}"]

### API
- Endpoints: ["/api/applications", "/api/applications/{id}"]
- Authentication: JWT

## Security Requirements

### Authentication
- Required: No (for submission), Yes (for access)
- Type: Form submission is public, admin access requires OAuth2
- Scope: Public write, Admin read/update

### Data Protection
- Encryption: Yes
- Retention: 1 year
- Access: Adoption coordinators, Shelter management

## Example Implementation

### Dog Adoption Form
```typescript
{
  name: "Dog Adoption Application",
  description: "Apply to adopt a dog from our shelter",
  businessContext: "Match potential adopters with available dogs",
  
  schema: {
    fields: [
      {
        name: "applicantInfo",
        type: "section",
        label: "Your Information",
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
            label: "Email Address",
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
          },
          {
            name: "address",
            type: "text",
            label: "Street Address",
            required: true
          },
          {
            name: "city",
            type: "text",
            label: "City",
            required: true
          },
          {
            name: "state",
            type: "select",
            label: "State",
            required: true,
            options: ["AL", "AK", "AZ", "AR", "CA", /* all US states */]
          },
          {
            name: "zipCode",
            type: "text",
            label: "ZIP Code",
            required: true,
            validation: {
              type: "custom",
              pattern: "^[0-9]{5}(-[0-9]{4})?$",
              message: "Please enter a valid ZIP code"
            }
          }
        ]
      },
      {
        name: "homeInfo",
        type: "section",
        label: "Home Environment",
        fields: [
          {
            name: "homeType",
            type: "select",
            label: "Type of Home",
            required: true,
            options: ["House", "Apartment", "Condo", "Townhouse", "Mobile Home", "Other"]
          },
          {
            name: "ownRent",
            type: "radio",
            label: "Do you own or rent your home?",
            required: true,
            options: ["Own", "Rent"]
          },
          {
            name: "landlordPermission",
            type: "radio",
            label: "If renting, do you have permission to have a dog?",
            required: false,
            options: ["Yes", "No", "Not Applicable"]
          },
          {
            name: "landlordContact",
            type: "text",
            label: "Landlord's name and phone number (if applicable)",
            required: false
          },
          {
            name: "yard",
            type: "radio",
            label: "Do you have a fenced yard?",
            required: true,
            options: ["Yes", "No"]
          },
          {
            name: "yardDetails",
            type: "text",
            label: "If yes, fence height and type",
            required: false
          },
          {
            name: "householdMembers",
            type: "number",
            label: "Number of adults in household",
            required: true
          },
          {
            name: "children",
            type: "number",
            label: "Number of children in household",
            required: true
          },
          {
            name: "childrenAges",
            type: "text",
            label: "Ages of children (if applicable)",
            required: false
          }
        ]
      },
      {
        name: "petExperience",
        type: "section",
        label: "Pet Experience",
        fields: [
          {
            name: "currentPets",
            type: "checkbox",
            label: "Current pets in household (select all that apply)",
            required: false,
            options: ["Dogs", "Cats", "Birds", "Small Animals", "Reptiles", "Fish", "Other", "None"]
          },
          {
            name: "currentPetsDetails",
            type: "text",
            label: "Details about current pets (breeds, ages, temperaments)",
            required: false
          },
          {
            name: "pastPets",
            type: "text",
            label: "Tell us about your past experience with pets",
            required: true
          },
          {
            name: "vetInfo",
            type: "text",
            label: "Current/past veterinarian name and contact information",
            required: false
          },
          {
            name: "trainingExperience",
            type: "select",
            label: "Experience with dog training",
            required: true,
            options: ["None", "Basic", "Intermediate", "Advanced", "Professional"]
          }
        ]
      },
      {
        name: "adoptionPreferences",
        type: "section",
        label: "Adoption Preferences",
        fields: [
          {
            name: "specificDog",
            type: "text",
            label: "Are you applying for a specific dog? If yes, which one?",
            required: false
          },
          {
            name: "dogSize",
            type: "checkbox",
            label: "Preferred dog size",
            required: true,
            options: ["Small (under 25lbs)", "Medium (25-50lbs)", "Large (50-80lbs)", "X-Large (80+ lbs)"]
          },
          {
            name: "dogAge",
            type: "checkbox",
            label: "Preferred dog age",
            required: true,
            options: ["Puppy (under 1 year)", "Young (1-3 years)", "Adult (3-7 years)", "Senior (7+ years)"]
          },
          {
            name: "energyLevel",
            type: "select",
            label: "Preferred energy level",
            required: true,
            options: ["Low", "Medium", "High", "Any"]
          },
          {
            name: "specialNeeds",
            type: "radio",
            label: "Would you consider a dog with special needs?",
            required: true,
            options: ["Yes", "No", "Maybe - depends on the needs"]
          },
          {
            name: "dogPurpose",
            type: "checkbox",
            label: "What role will this dog play in your life? (select all that apply)",
            required: true,
            options: ["Companion", "Guard dog", "Service/therapy dog", "Working dog", "For children", "For elderly", "Other"]
          }
        ]
      },
      {
        name: "careDetails",
        type: "section",
        label: "Care Information",
        fields: [
          {
            name: "hoursAlone",
            type: "select",
            label: "How many hours will the dog typically be alone each day?",
            required: true,
            options: ["0-2 hours", "2-4 hours", "4-8 hours", "8+ hours"]
          },
          {
            name: "sleepLocation",
            type: "text",
            label: "Where will the dog sleep?",
            required: true
          },
          {
            name: "exercisePlan",
            type: "text",
            label: "Describe your plan for exercising the dog",
            required: true
          },
          {
            name: "trainingPlan",
            type: "text",
            label: "Describe your plan for training the dog",
            required: true
          },
          {
            name: "accommodations",
            type: "text",
            label: "What accommodations are you willing to make for house training, medical needs, or behavior issues?",
            required: true
          },
          {
            name: "rehomeReason",
            type: "text",
            label: "Under what circumstances would you need to rehome this dog?",
            required: true
          },
          {
            name: "references",
            type: "text",
            label: "Please provide two personal references (name, relationship, phone)",
            required: true
          },
          {
            name: "additionalInfo",
            type: "text",
            label: "Any additional information you'd like to share?",
            required: false
          },
          {
            name: "photoUpload",
            type: "file",
            label: "Upload a photo of where the dog will stay (optional)",
            required: false
          }
        ]
      }
    ]
  },
  
  configuration: {
    layout: {
      sections: 5,
      fieldsPerSection: "Variable"
    },
    submitActions: {
      store: true,
      email: true,
      redirect: "/thank-you"
    }
  }
}
```

## Usage Instructions

1. **Copy Template**
   - Copy this template
   - Customize fields as needed for your shelter's requirements
   - Save as `dog-adoption.template.ts`

2. **Define Form**
   - Review and adjust all required fields
   - Add custom validation rules if needed
   - Configure email notifications for staff

3. **Use with AI**
   - Import template into your form building system
   - Request implementation of any custom logic
   - Generate admin views for application review

4. **Implement Form**
   - Deploy form on shelter website
   - Test submission process thoroughly
   - Train staff on reviewing applications
