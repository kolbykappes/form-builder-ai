# Getting Started

## Overview

This guide provides a step-by-step process for building form applications using our pattern. It's designed to be used alongside our detailed documentation and AI assistance.

## Prerequisites

> **Version Information Note**: The versions and setup instructions in this guide are current as of April 2025. Future readers (including AI assistants) should verify these against official sources. If your knowledge cutoff date is after April 2025, please ask for updated documentation.

- Node.js (v18 or later)
  - Download from: https://nodejs.org/
  - Verify installation: `node --version` and `npm --version`
- Git for Windows
  - Download from: https://git-scm.com/download/win
  - Verify installation: `git --version`
- VS Code (recommended)
  - Download from: https://code.visualstudio.com/
  - Install recommended extensions:
    - ESLint
    - Prettier
    - Tailwind CSS IntelliSense (for class name autocompletion and documentation)

## Initial Setup

> **Setup Note**: The following setup commands and configurations are current as of April 2025. Future readers should verify the latest recommended setup from:
> - Next.js: https://nextjs.org/docs/getting-started
> - Tailwind CSS: https://tailwindcss.com/docs/installation
> - React Hook Form: https://react-hook-form.com/get-started

1. **Create Next.js Project**
   ```powershell
   # Create Next.js project with TypeScript and Tailwind CSS
   npx create-next-app@latest . --typescript --tailwind --eslint --app --src-dir --import-alias "@/*"
   
   # When prompted:
   # - Would you like to use TypeScript? Yes
   # - Would you like to use ESLint? Yes
   # - Would you like to use Tailwind CSS? Yes
   # - Would you like to use `src/` directory? Yes
   # - Would you like to use App Router? Yes
   # - Would you like to customize the default import alias (@/*)? Yes
   ```

2. **Install Additional Dependencies**
   ```powershell
   # Install form and database dependencies
   npm install @hookform/resolvers zod react-hook-form mongodb @sendgrid/mail @tailwindcss/forms
   ```

3. **Configure Tailwind CSS**
   ```powershell
   # Update tailwind.config.js
   npx tailwindcss init -p
   ```
   
   Add the forms plugin to your `tailwind.config.js`:
   ```js
   module.exports = {
     content: [
       "./app/**/*.{js,ts,jsx,tsx}",
       "./pages/**/*.{js,ts,jsx,tsx}",
       "./components/**/*.{js,ts,jsx,tsx}",
     ],
     plugins: [
       require('@tailwindcss/forms'),
     ],
   }
   ```

4. **Set Up Environment Variables**
   ```powershell
   # Create .env.local file
   New-Item -Path ".env.local" -ItemType "file"
   ```
   
   Add the following to `.env.local`:
   ```
   MONGODB_URI=mongodb://adoption-db:27017/adoptions
   SENDGRID_API_KEY=your_sendgrid_api_key
   SENDGRID_FROM_EMAIL=noreply@rescueshelter.org
   ```

5. **Create Project Structure**
   ```powershell
   # Create necessary directories
   mkdir src\components
   mkdir src\lib
   mkdir src\types
   mkdir src\utils
   ```

6. **Verify Setup**
   ```powershell
   # Start development server
   npm run dev
   ```
   
   Open http://localhost:3000 in your browser to verify the setup.

## Project Structure

After setup, your project should have this structure:
```
<repository-name>/
├── src/
│   ├── app/
│   ├── components/
│   ├── lib/
│   ├── types/
│   └── utils/
├── public/
├── docs/
│   ├── 00-getting-started.md
│   ├── 01-form-definition.md
│   ├── 02-architecture-overview.md
│   └── 03-implementation-guide.md
├── .env.local
├── .gitignore
├── next.config.js
├── package.json
├── tailwind.config.js
└── tsconfig.json
```

## Next Steps

1. **Review Documentation**
   - Read `docs/01-form-definition.md` for form structure
   - Review `docs/02-architecture-overview.md` for system design
   - Follow `docs/03-implementation-guide.md` for development

2. **Define Your Form**
   - Use the form definition template
   - Specify your form requirements
   - Define data schema

3. **Start Implementation**
   - Follow the implementation guide
   - Use AI assistance for specific components
   - Test each feature as you build

## AI Interaction Guidelines

When working with the AI assistant:

1. **Be Specific**
   - Provide clear requirements
   - Share relevant code snippets
   - Ask for explanations when needed

2. **Follow Best Practices**
   - Review AI suggestions carefully
   - Test all code changes
   - Document modifications

3. **Debugging**
   - Share error messages
   - Provide context
   - Ask for step-by-step solutions

## Common Issues and Solutions

1. **Next.js Installation Issues**
   - If `npx create-next-app` fails, try:
     ```