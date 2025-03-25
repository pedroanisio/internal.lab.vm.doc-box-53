# 📝 Documentation Guidelines

## Naming Conventions

The documentation system follows a hierarchical structure with reversed hierarchy naming:
- Format: `internal.lab.vm.{vm-name}`
- Example: `internal.lab.vm.doc-box-53`

This naming convention ensures consistency and traceability across all documentation sets.

## File Structure

Each VM documentation should follow the template structure provided in the Templates section:

```
/srv/docs/repos/internal.lab.vm.{vm-name}/
├── docs/
│   ├── index.md              # Home page / Overview
│   ├── reference/            # Technical specifications
│   ├── services/             # Documentation for services running on the VM
│   └── maintenance/          # Maintenance procedures and schedules
└── mkdocs.yml                # MkDocs configuration
```

## Content Guidelines

### 1. Be Concise
- Use clear, direct language
- Avoid unnecessary technical jargon
- Break up content with headings and lists

### 2. Use Consistent Formatting
- Follow Markdown formatting guidelines
- Use emoji icons consistently (📝 for docs, 🖥️ for hardware, etc.)
- Use code blocks for commands and configuration snippets

### 3. Include Essential Information
Every VM documentation should include:
- Purpose and business justification
- Server specifications
- Network configuration
- Services running
- Maintenance procedures
- Dependency relationships

### 4. Keep Documentation Updated
- Update documentation whenever changes are made to the VM
- Include a "Last Updated" date in each major section
- Document changes in the "Recent Changes" section of index.md

## MkDocs Structure

Each VM documentation container should:
- Use the Material theme with consistent styling
- Include proper navigation structure in mkdocs.yml
- Deploy on its own port (8001-8010)
- Be accessible via the central Traefik reverse proxy

## Central Documentation Hub

The central entry point connects all documentation instances through:
- A landing page listing all available documentation sets
- Consistent navigation between documentation sets
- Unified search capabilities across all documentation

## Contribution Process

1. Clone the repository for the VM documentation
2. Make changes following these guidelines
3. Test locally using Docker
4. Commit and push changes
5. Watchtower will automatically deploy the updated documentation