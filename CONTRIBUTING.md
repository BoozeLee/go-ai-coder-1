# Contributing to Go AI Coder

Thank you for contributing to the Go AI Coder project! This guide will help you understand our development workflow and contribution standards.

## Quick Start

1. **Build and Test**: Use `go build -o github_ai_agent github_ai_agent.go` for development
2. **Validate Changes**: Always run `./github_ai_agent --help` to verify functionality
3. **Follow Instructions**: Check `.github/copilot-instructions.md` for detailed development guidance

## Development Workflow

### Working vs Broken Components

✅ **Use These (Working):**
- `github_ai_agent.go` - Primary executable (fully functional)
- `make deps` - Dependency installation
- `make clean` - Cleanup artifacts

❌ **Avoid These (Known Issues):**
- `cmd/main.go` - Missing NewCloudAIClient function
- `make build` - Dependency resolution issues
- Docker builds - Context and dependency problems

### Build Validation

Every contribution must pass these validation steps:

```bash
# Primary validation (must complete in <2 seconds)
go build -o test github_ai_agent.go

# Functionality validation
./test --help          # Must show usage information
./test                 # Must show interactive prompt
```

## Manual Action Required Issues

When automated systems (like GitHub Copilot agents) encounter blocking issues that cannot be resolved automatically, they will create issues using our "Manual Action Required" workflow.

### For Contributors

If you see issues labeled with `manual-action`, `needs-triage`, and `copilot-flagged`:

1. **Review the issue details** - Check the problem summary, logs, and attempted solutions
2. **Validate the problem** - Reproduce the issue if possible
3. **Implement the fix** - Address the root cause based on the recommendations
4. **Update the issue** - Document your solution and testing steps
5. **Close when resolved** - Verify the fix works and close the issue

### For Automated Systems

Use the "Manual Action Required" issue template when:

- Build failures persist after standard troubleshooting
- Missing dependencies or functions require code changes
- Docker or environment configuration issues need human decisions
- Repository setup problems require manual intervention
- Performance issues exceed validated thresholds by 3x

**Template Location**: `.github/ISSUE_TEMPLATE/manual-action-required.yml`

## Priority Guidelines

**Critical** (24 hours): Blocks core functionality
- Primary executable won't build or run
- Core file operations broken
- Security vulnerabilities

**High** (48 hours): Blocks development workflow  
- Build system failures
- Test infrastructure issues
- CI/CD pipeline problems

**Medium** (1 week): Blocks optional features
- AI integration issues (when Ollama unavailable)
- GitHub API integration problems
- Documentation generation failures

**Low** (2 weeks): Enhancement requests
- Documentation improvements
- Code quality optimizations
- Performance enhancements

## Code Quality Standards

### Go Code Standards
- Follow standard Go formatting (`gofmt`)
- Include error handling for all operations
- Use descriptive variable and function names
- Add comments for complex logic

### Testing Requirements
- Validate builds complete in expected timeframes
- Test both with and without optional dependencies (Ollama, GitHub token)
- Verify error handling for common failure scenarios

### Documentation
- Update `.github/copilot-instructions.md` for any workflow changes
- Include command examples with expected execution times
- Document any new environment requirements or dependencies

## Submitting Changes

1. **Test thoroughly** using the validation steps above
2. **Update documentation** if changing workflows or adding features
3. **Include timing information** for any new build or test commands
4. **Follow commit message format**: 
   - `feat:` for new features
   - `fix:` for bug fixes  
   - `docs:` for documentation changes
   - `build:` for build system changes

## Getting Help

- **Development Questions**: Check `.github/copilot-instructions.md` first
- **Build Issues**: Review the troubleshooting section in the instructions
- **Manual Action Required**: Use the issue template for blocking problems
- **General Support**: Create a regular GitHub issue with appropriate labels

## Repository Structure

```
├── github_ai_agent.go           # Primary executable (USE THIS)
├── cmd/main.go                  # Alternative entry point (BROKEN)
├── .github/
│   ├── copilot-instructions.md  # Development workflow guide
│   ├── ISSUE_TEMPLATE/          # Issue templates
│   └── workflows/               # CI/CD automation
├── Makefile                     # Build automation (partial)
├── go.mod/go.sum               # Go module definition
└── internal/                   # Internal packages
```

Remember: This project is designed to work with GitHub Copilot agents, so clear documentation and automated workflows are essential for effective collaboration between human developers and AI assistants.