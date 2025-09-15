# Go AI Coder - GitHub Copilot Instructions

**ALWAYS follow these instructions first and only fallback to additional search and context gathering if the information here is incomplete or found to be in error.**

## Working Effectively

### Environment Setup and Dependencies
- Install Go 1.21+ (current version works with Go 1.24.7)
- Run `go mod download` - takes ~1.5 seconds 
- Run `go mod tidy` to verify dependencies
- Optional: Install Ollama for AI functionality (app works without it for file operations)
- Optional: Set GITHUB_TOKEN environment variable for GitHub API features

### Building the Application
**NEVER CANCEL builds or long-running commands. Always wait for completion.**

#### Primary Build Method
- `go build -o github_ai_agent github_ai_agent.go` - **Build time: ~1 second**
- This creates a working executable for the main AI agent functionality

#### Alternative Build Methods  
- `make deps` - Install dependencies (~0.6 seconds)
- `make build` - **CURRENTLY FAILS** due to missing NewCloudAIClient function in cmd/main.go scope
- `make clean` - Clean artifacts (~0.15 seconds)

#### Docker Build
- `make docker` - **FAILS** - Docker build expects complete go.sum and has dependency issues
- Docker builds are not currently functional due to build context issues

### Build Time Expectations and Timeouts
- **Go dependency download: 1.5 seconds** - Set timeout to 60+ seconds
- **Single file Go build: 1 second** - Set timeout to 30+ seconds
- **Full project build: CURRENTLY BROKEN** - Would need 60+ minute timeout when working
- **Docker build: 4+ seconds (fails)** - Set timeout to 60+ minutes when working

### Running the Application

#### Working Executable
- `./github_ai_agent --help` - Show command line options
- `./github_ai_agent --model "llama3.2:3b"` - Start with specific model
- Application starts successfully and shows interactive prompt
- **VALIDATION SCENARIO**: Run `./github_ai_agent --help` and verify help output displays correctly

#### Command Line Options
```bash
-autosave       Auto-save conversations (default true)
-cache          Enable response caching (default true)  
-learning string Directory for AI learning data (default "ai_learning")
-model string   Ollama model to use (default "llama3.2:3b")
-ollama string  Ollama API URL (default "http://localhost:11434/v1")
-temp float     AI temperature (0.0-1.0) (default 0.7)
-tokens int     Maximum tokens for AI responses (default 2000)
-verbose        Enable verbose output
```

### Core Functionality Testing

#### File Operations (Works without AI)
- `read <file>` - Read and analyze file content
- `list <directory>` - List directory contents with analysis
- These commands work even without Ollama running

#### GitHub Integration (Requires GITHUB_TOKEN)
- `github repos` - List your repositories 
- `github search <query>` - Search GitHub repositories
- `github issues <repo>` - List repository issues
- `github prs <repo>` - List pull requests
- **Note**: Without GITHUB_TOKEN, GitHub features are limited

#### AI Learning Features (Requires Ollama)
- `ai learn` - Comprehensive Go ecosystem research
- `ai research <topic>` - Research specific topics  
- `ai scrape <url>` - Learn from web content
- `go resources` - Show curated Go learning resources

### Project Structure
```
├── cmd/main.go              # Main entry point (BROKEN - missing functions)
├── github_ai_agent.go       # Working AI agent (USE THIS)
├── Makefile                 # Build system (partially broken)
├── go.mod                   # Go module definition
├── go.sum                   # Dependency checksums
├── .github/workflows/ci.yml # CI/CD pipeline
├── Dockerfile               # Docker configuration (broken)
├── scripts/install.sh       # Installation script
└── internal/               # Internal packages
```

### Validation Requirements

#### Essential Validation Steps
1. **Build Validation**: `go build -o test github_ai_agent.go` - must complete in under 2 seconds
2. **Help Validation**: `./github_ai_agent --help` - must show usage information
3. **Start Validation**: Run app and verify it shows the command prompt
4. **File Read Test**: Within app, run `read README.md` (works without Ollama)

#### DO NOT attempt these (Known Issues)
- Building cmd/main.go - missing NewCloudAIClient function
- Running full test suite - build failures prevent testing  
- Docker builds - context and dependency issues
- Full make build - dependency resolution problems

### Troubleshooting

#### Standard Resolution Steps
1. **First Attempt**: Follow standard troubleshooting for the specific error
2. **Second Attempt**: If first attempt fails, retry once with additional diagnostic info
3. **Manual Action Required**: If both attempts fail, create manual action issue

#### Common Issues and Resolutions

**Build Error "undefined: NewCloudAIClient"**
- **Resolution**: Use github_ai_agent.go instead of cmd/main.go
- **If Persists**: Create manual action issue for missing function implementation

**"No .env file found"**
- **Resolution**: Normal warning, app works with environment variables
- **Action**: No intervention needed

**"GITHUB_TOKEN not set"**
- **Resolution**: Optional, only needed for GitHub API features
- **Action**: Set environment variable if GitHub integration needed

**"AIMLAPI_API_KEY required"**
- **Resolution**: Only needed for certain AI features, not core functionality
- **Action**: Set if cloud AI features required

**Docker Build Failures**
- **Resolution**: Currently known issue with dependency context
- **Action**: **CREATE MANUAL ACTION ISSUE** - do not attempt workarounds

**Performance Degradation (>3x expected time)**
- **Resolution**: Log warning but allow completion
- **If Exceeds**: Create manual action issue with performance metrics

#### Working Around Build Issues
- Focus on github_ai_agent.go which builds successfully
- Use individual Go file builds rather than module builds
- Skip Docker and complex build processes until dependencies are fixed
- **When all workarounds fail**: Create manual action issue with full diagnostic info

### Development Guidelines
- **Primary executable**: Always use github_ai_agent.go for development and testing
- **Build validation**: Always run `go build -o test github_ai_agent.go` to verify changes
- **Timeout values**: Use 60+ seconds for any build operations, 30+ seconds for simple builds
- **Manual testing**: Always test the help command and basic functionality after changes

### File System Operations
- **Read files**: Use the `read` command within the application
- **List directories**: Use the `list` command within the application  
- **Configuration**: App creates `ai_learning/` directory for data storage
- **Logs**: Application outputs to stdout/stderr

Remember: This is an AI-powered coding assistant that combines file operations, GitHub integration, and AI learning capabilities. The core file and directory operations work independently of AI services.

## Manual Action Required Workflow

When encountering issues that cannot be resolved automatically, Copilot agents **MUST** create a GitHub issue to request manual intervention.

### When to Create Manual Action Issues

**ALWAYS create a manual action issue when:**
- Build failures persist after standard troubleshooting attempts
- Missing dependencies or functions that require code changes  
- Docker context or environment configuration issues
- Repository setup problems requiring human decision-making
- Security or permission issues that need manual review
- Performance degradation that exceeds validated timeout thresholds by 3x or more

### Manual Action Issue Creation Process

**Step 1: Stop Automated Attempts**
- Do not retry failed operations more than 2 times
- Do not attempt workarounds for explicitly flagged broken paths
- Document all attempted solutions before escalating

**Step 2: Create GitHub Issue Using Template**
- Use the "Manual Action Required" issue template
- Include specific error messages and log outputs
- List all automated attempts made
- Provide clear reproduction steps

**Step 3: Issue Format Requirements**
```
Title: [MANUAL ACTION REQUIRED] <concise problem description>
Labels: manual-action, needs-triage, copilot-flagged
```

**Required Issue Content:**
- **Problem Summary**: Clear description of the blocking issue
- **Context & Logs**: Complete error messages and relevant output
- **Automated Attempts**: List of all retry attempts and troubleshooting steps taken
- **Recommended Action**: Specific suggestions for human resolution (if known)

### Example Manual Action Scenarios

**Example 1: Missing Function Error**
```
Title: [MANUAL ACTION REQUIRED] Fix missing NewCloudAIClient function in cmd/main.go
Problem: Build fails due to undefined function
Context: Error during 'go build ./cmd/main.go'
Attempts: Verified imports, checked function definitions, searched codebase
Recommendation: Implement missing NewCloudAIClient function or remove reference
```

**Example 2: Docker Build Failure**
```
Title: [MANUAL ACTION REQUIRED] Resolve Docker dependency context issues
Problem: Docker build fails with dependency resolution errors
Context: 'make docker' command fails consistently
Attempts: Verified Dockerfile syntax, checked base image, tried dependency cache clear
Recommendation: Review Docker build context and dependency management strategy
```

### Automation Guidelines for Manual Actions

**Before Creating Issue:**
1. Verify the problem is not in the "DO NOT attempt" list
2. Check if issue already exists with same problem signature
3. Attempt standard troubleshooting (max 2 retries)
4. Gather complete diagnostic information

**After Creating Issue:**
1. Stop all related automated attempts on that component
2. Log the issue number in any related error messages
3. Continue with other unrelated tasks if possible
4. Monitor issue for resolution before reattempting

### Human Response Expectations

**Priority Levels:**
- **Critical**: Blocks core functionality (within 24 hours)
- **High**: Blocks development workflow (within 48 hours) 
- **Medium**: Blocks optional features (within 1 week)
- **Low**: Documentation or enhancement requests (within 2 weeks)

**Resolution Process:**
1. Human reviews issue and validates problem
2. Human implements fix or provides specific guidance
3. Human updates issue with resolution steps
4. Copilot agent validates fix and closes issue

This workflow ensures efficient escalation of problems that require human expertise while maintaining clear communication and avoiding endless automated retry loops.