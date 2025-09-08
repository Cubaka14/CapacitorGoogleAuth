# Contributing to CapacitorGoogleAuth

Thank you for your interest in contributing to CapacitorGoogleAuth! This guide will help you get started with contributing to this project.

## 🚀 Getting Started

### Prerequisites

- Node.js (v16 or higher)
- npm or yarn
- Git
- Basic knowledge of TypeScript and Capacitor

### Development Setup

1. **Fork and Clone**
   ```bash
   git clone https://github.com/your-username/CapacitorGoogleAuth.git
   cd CapacitorGoogleAuth
   ```

2. **Install Dependencies**
   ```bash
   npm install
   ```

3. **Build the Project**
   ```bash
   npm run build
   ```

4. **Run the Demo (Optional)**
   ```bash
   cd demo
   npm install
   npm start
   ```

## 🛠️ Development Workflow

### Making Changes

1. **Create a Feature Branch**
   ```bash
   git checkout -b feature/your-feature-name
   ```

2. **Make Your Changes**
   - Follow the existing code style
   - Add TypeScript types for any new functionality
   - Update documentation as needed

3. **Build and Test**
   ```bash
   npm run build
   ```

4. **Test on Multiple Platforms**
   - Test web implementation in the demo
   - Test iOS implementation (if applicable)
   - Test Android implementation (if applicable)

### Code Style

- **TypeScript**: Use TypeScript for all new code
- **Formatting**: Follow existing code formatting patterns
- **Naming**: Use descriptive variable and function names
- **Comments**: Add JSDoc comments for public APIs

### Commit Guidelines

Use clear and descriptive commit messages:

```bash
# Good examples
git commit -m "feat: add support for custom scopes in initialization"
git commit -m "fix: resolve token refresh issue on Android"
git commit -m "docs: update README with new configuration options"

# Conventional commit types
feat:     # New feature
fix:      # Bug fix
docs:     # Documentation only changes
style:    # Code style changes (formatting, etc.)
refactor: # Code refactoring
test:     # Adding or updating tests
chore:    # Maintenance tasks
```

## 📋 Contribution Areas

### High Priority

- **Bug Fixes**: Address reported issues and bugs
- **Platform Compatibility**: Ensure compatibility with latest Capacitor versions
- **Documentation**: Improve guides, examples, and API documentation
- **Performance**: Optimize authentication flows and token management

### Welcome Contributions

- **Demo Applications**: New examples for different frameworks
- **Error Handling**: Improved error messages and recovery mechanisms
- **Testing**: Unit tests and integration tests
- **Accessibility**: Improve accessibility features
- **TypeScript**: Better type definitions and type safety

### Feature Requests

Before implementing new features:
1. Check existing issues and discussions
2. Open an issue to discuss the feature
3. Wait for maintainer feedback before starting work

## 🧪 Testing

### Manual Testing

1. **Web Testing**
   ```bash
   cd demo
   npm start
   # Test in different browsers
   ```

2. **Mobile Testing**
   ```bash
   cd demo
   npx cap add ios
   npx cap add android
   npx cap run ios
   npx cap run android
   ```

### Testing Checklist

- [ ] Sign-in flow works correctly
- [ ] Sign-out flow works correctly
- [ ] Token refresh functionality
- [ ] Error handling for network issues
- [ ] Error handling for user cancellation
- [ ] Configuration validation
- [ ] Cross-platform compatibility

## 📚 Documentation

### Updating Documentation

When making changes that affect the public API:

1. **Update README.md**: Main documentation and examples
2. **Update EXAMPLES.md**: Add new usage examples
3. **Update PROJECT_OVERVIEW.md**: Architectural changes
4. **Update DEVELOPER_GUIDE.md**: Development-related changes

### Documentation Standards

- Use clear, concise language
- Provide complete code examples
- Include error handling in examples
- Test all code examples before submitting

## 🐛 Bug Reports

### Before Reporting

1. Check existing issues
2. Test with the latest version
3. Verify the issue on multiple platforms if possible

### Bug Report Template

```markdown
**Description**
A clear description of the bug.

**Platform**
- [ ] Web
- [ ] iOS
- [ ] Android

**Environment**
- Capacitor version: 
- Plugin version:
- OS version:
- Framework (Angular/Vue/React):

**Steps to Reproduce**
1. 
2. 
3. 

**Expected Behavior**
What should happen.

**Actual Behavior**
What actually happens.

**Code Example**
```typescript
// Minimal code example
```

**Error Messages**
Any error messages or logs.
```

## 🚀 Pull Requests

### Before Submitting

- [ ] Code builds without errors
- [ ] Changes are tested on relevant platforms
- [ ] Documentation is updated
- [ ] Commit messages follow conventions
- [ ] Branch is up to date with main

### PR Template

```markdown
**Description**
Brief description of changes.

**Type of Change**
- [ ] Bug fix
- [ ] New feature
- [ ] Documentation update
- [ ] Code refactoring
- [ ] Performance improvement

**Testing**
- [ ] Tested on Web
- [ ] Tested on iOS
- [ ] Tested on Android

**Checklist**
- [ ] Code builds successfully
- [ ] Documentation updated
- [ ] Follows existing code style
- [ ] Added appropriate comments
```

## 🤝 Community Guidelines

### Be Respectful

- Be welcoming to newcomers
- Use inclusive language
- Provide constructive feedback
- Help others learn and grow

### Communication

- Use clear, concise language
- Provide context in discussions
- Reference relevant issues or PRs
- Be patient with response times

## 📞 Getting Help

- **Issues**: Open GitHub issues for bugs and feature requests
- **Discussions**: Use GitHub Discussions for questions and ideas
- **Documentation**: Check existing documentation first

## 🎯 Roadmap and Priorities

### Current Focus Areas

1. **Stability**: Fixing bugs and improving reliability
2. **Compatibility**: Supporting latest Capacitor and platform versions
3. **Documentation**: Improving guides and examples
4. **Performance**: Optimizing authentication flows

### Future Goals

- Enhanced error handling and recovery
- Better TypeScript support
- Improved testing coverage
- Additional authentication options

Thank you for contributing to CapacitorGoogleAuth! Your contributions help make Google authentication easier for the entire Capacitor community. 🎉