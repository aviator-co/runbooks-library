# Reduce Docker Layer Sizes
Optimize Docker images by reducing layer sizes through multi-stage builds, dependency consolidation, and build cache improvements.

## Summary of changes
- Current Docker images likely contain unnecessary build dependencies, cached package managers, and inefficient layer ordering
- Implement multi-stage builds to separate build and runtime environments
- Consolidate RUN commands to reduce layer count and improve caching
- Add .dockerignore files to exclude unnecessary files from build context
- Optimize base image selection and package installation strategies

## Execution Steps

### Step 1: Analysis and Documentation
Establish baseline metrics and create optimization strategy documentation.

#### 1.1: Docker Image Size Audit
Create comprehensive analysis of current Docker image sizes and layer composition.
- Run `docker images` and `docker history` commands on all existing images to capture current sizes
- Document findings in **docs/docker-optimization-audit.md** including image names, sizes, and layer breakdown
- Identify top 5 largest images and their primary size contributors
- Create baseline metrics table with image names, total size, number of layers, and largest layer sizes

#### 1.2: Build Context Analysis
Analyze Docker build contexts to identify unnecessary files being included.
- Review all **Dockerfile** locations in the codebase and list build context directories
- Run `docker build --dry-run` or manually inspect build contexts to identify large files
- Document build context sizes and identify files that should be excluded
- Create inventory of all **package.json**, **requirements.txt**, **go.mod**, or similar dependency files

#### 1.3: Optimization Strategy Documentation
Create detailed optimization plan based on audit findings.
- Document multi-stage build opportunities in **docs/docker-optimization-strategy.md**
- Identify which services can benefit from Alpine Linux base images
- Plan RUN command consolidation strategies for each Dockerfile
- Define target size reduction goals (percentage or absolute size) for each image

### Step 2: Build Context Optimization
Reduce unnecessary files included in Docker build contexts.

#### 2.1: Create .dockerignore Files
Add .dockerignore files to exclude unnecessary files from build contexts.
- Create **.dockerignore** file in each directory containing a Dockerfile
- Add common exclusions: `.git/`, `*.md`, `node_modules/`, `__pycache__/`, `.pytest_cache/`, `*.log`
- Add service-specific exclusions based on build context analysis from Step 1.2
- Test build context size reduction by running `docker build` and comparing context sizes

#### 2.2: Optimize File Copying Strategy
Restructure COPY commands to improve layer caching and reduce context size.
- Modify Dockerfiles to copy dependency files (package.json, requirements.txt) before copying source code
- Use specific COPY commands instead of `COPY . .` where possible
- Implement `.dockerignore` patterns to exclude test files, documentation, and development tools
- Verify builds still work correctly after COPY command changes

### Step 3: Base Image Optimization
Switch to smaller base images where appropriate.

#### 3.1: Alpine Linux Migration Planning
Identify services that can migrate from full OS images to Alpine variants.
- Create compatibility matrix in **docs/alpine-migration-plan.md** listing current base images and Alpine alternatives
- Test Alpine compatibility for each service by creating experimental Dockerfiles with `-alpine` variants
- Document any Alpine-specific package installation changes needed (apk vs apt/yum)
- Identify services that cannot use Alpine due to specific dependencies

#### 3.2: Implement Alpine Base Images
Migrate compatible services to Alpine Linux base images.
- Update Dockerfiles to use Alpine variants (e.g., `node:18-alpine`, `python:3.11-alpine`)
- Replace `apt-get` or `yum` commands with `apk add` equivalents
- Add `--no-cache` flag to `apk add` commands to avoid storing package cache
- Update any shell scripts that assume bash to use sh or explicitly install bash if needed
- Test all migrated images to ensure functionality is preserved

### Step 4: Multi-stage Build Implementation
Implement multi-stage builds to separate build and runtime environments.

#### 4.1: Identify Multi-stage Candidates
Analyze which services would benefit most from multi-stage builds.
- Review Dockerfiles that install build tools (gcc, make, npm, pip with build dependencies)
- Document current build vs runtime dependency separation opportunities
- Create multi-stage build templates for common patterns (Node.js, Python, Go, Java)
- Prioritize services with largest potential size reduction

#### 4.2: Implement Multi-stage Builds
Convert identified Dockerfiles to use multi-stage build patterns.
- Create builder stage with all build dependencies and tools
- Create runtime stage with minimal base image and only runtime dependencies
- Copy built artifacts from builder stage to runtime stage using `COPY --from=builder`
- Remove build tools and intermediate files from final stage
- Test that multi-stage builds produce working images with reduced sizes

#### 4.3: Optimize Dependency Installation
Consolidate and optimize package installation commands.
- Combine multiple RUN commands that install packages into single commands
- Add cleanup commands in the same RUN layer (e.g., `apt-get clean`, `rm -rf /var/lib/apt/lists/*`)
- Use `--no-install-recommends` flag for apt-get to avoid suggested packages
- Remove package manager caches in the same layer they're created

### Step 5: Layer Optimization and Caching
Optimize layer ordering and consolidation for better caching and smaller sizes.

#### 5.1: RUN Command Consolidation
Combine related RUN commands to reduce layer count.
- Merge sequential RUN commands that perform related operations (install, configure, cleanup)
- Use `&&` to chain commands and `\` for line continuation to maintain readability
- Ensure cleanup commands (rm, apt-get clean) happen in same layer as operations that create temporary files
- Test that consolidated commands don't break builds or functionality

#### 5.2: Layer Ordering Optimization
Reorder Dockerfile commands to maximize build cache effectiveness.
- Move frequently changing commands (COPY source code) toward the end of Dockerfile
- Place stable commands (dependency installation) early in Dockerfile
- Group related operations that should be cached together
- Add comments explaining layer ordering decisions for future maintainers

### Step 6: Verification and Documentation
Validate optimizations and document results.

#### 6.1: Size Reduction Verification
Measure and document achieved size reductions.
- Build all optimized images and record new sizes using `docker images`
- Compare new sizes against baseline metrics from Step 1.1
- Document size reduction percentages and absolute savings in **docs/docker-optimization-results.md**
- Identify any images that didn't achieve expected reductions and analyze reasons

#### 6.2: Update Build Documentation
Update development and deployment documentation to reflect Docker changes.
- Update **README.md** files with new build instructions if multi-stage builds change the process
- Document any new build arguments or environment variables introduced
- Update CI/CD pipeline documentation if build processes changed
- Create troubleshooting guide for common issues with optimized images

## Manual testing plan
- Build all modified Docker images locally and verify they start successfully
- Run application health checks or smoke tests for each optimized service
- Compare application functionality between original and optimized images
- Test multi-stage builds work correctly in CI/CD environments
- Verify .dockerignore files don't exclude necessary files by testing builds in clean environments
- Test Alpine-based images on different architectures if applicable (ARM64, x86_64)
- Validate that optimized images work correctly in production-like environments
- Check that layer caching improvements actually speed up subsequent builds