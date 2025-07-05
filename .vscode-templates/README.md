# VS Code Configuration Templates

This directory contains ready-to-use Visual Studio Code configuration templates that can significantly improve your development experience.

## Available Templates

- [settings.json](./settings.json) - Optimized VS Code settings for multiple languages and frameworks
- [extensions.json](./extensions.json) - Recommended extensions for various development tasks
- [launch.json](./launch.json) - Debug configurations for multiple languages and frameworks

## How to Use

### Option 1: Copy to your project

1. Create a `.vscode` directory in your project's root:
   ```
   mkdir -p /path/to/your/project/.vscode
   ```

2. Copy the configuration files you need:
   ```
   cp /path/to/this/repo/.vscode-templates/settings.json /path/to/your/project/.vscode/
   cp /path/to/this/repo/.vscode-templates/extensions.json /path/to/your/project/.vscode/
   cp /path/to/this/repo/.vscode-templates/launch.json /path/to/your/project/.vscode/
   ```

3. Customize the files as needed for your specific project

### Option 2: Use as User Settings (Global)

For settings that you want to apply to all your projects:

1. Open VS Code
2. Go to File > Preferences > Settings (or press `Ctrl+,` / `Cmd+,`)
3. Click on the "Open Settings (JSON)" icon in the top-right corner
4. Copy relevant settings from our `settings.json` template

## What's Included

### settings.json

- Editor optimizations (formatting, linting, etc.)
- Language-specific settings (JavaScript, TypeScript, Python, Java, etc.)
- Git integration settings
- Performance optimizations
- File handling settings
- Terminal configurations

### extensions.json

Recommendations for VS Code extensions categorized by:

- Essential extensions (ESLint, Prettier, etc.)
- Language-specific extensions
- Debugging tools
- Productivity boosters
- Themes and UI enhancements

### launch.json

Debug configurations for:

- Node.js/JavaScript
- React and Next.js
- Python (including Django and Flask)
- Java and Spring Boot
- .NET/C#
- Go
- PHP
- Docker

## Customization

These templates are designed to be a starting point. You should modify them to fit your specific workflow and preferences:

1. Remove settings for languages/frameworks you don't use
2. Adjust formatting rules to match your team's coding style
3. Update file paths and project-specific configurations
4. Add or remove extensions based on your needs

## Recommended Extensions Manager

After setting up `extensions.json`, you can see all recommended extensions in VS Code:

1. Open the Extensions view (`Ctrl+Shift+X` / `Cmd+Shift+X`)
2. Click on "Filter Extensions..." and select "Recommended"

This will show all extensions defined in your workspace's `extensions.json` file that you haven't installed yet.

## Best Practices

- Keep your settings simple and focused on what you actually need
- Document any unusual settings with comments
- Update your configurations regularly as VS Code and extensions evolve
- Consider using a team-wide configuration for consistent development experiences
