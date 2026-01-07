# Mojo GPU Puzzles - Deprecation Audit Repository

**Repository Purpose**: Systematic deprecation audit of the [Mojo GPU Puzzles](https://github.com/modular/mojo-gpu-puzzles) repository and development of an automated deprecation scanner tool.

## Overview

This repository contains the work performed to identify and fix deprecated Mojo API usage in the GPU Puzzles codebase, along with a reusable TOML-driven scanner tool for future deprecation audits.

## Contributions to Upstream

### Pull Requests Submitted

1. **PR #193** - Fix LayoutTensor Builder Syntax
   - Fixed 5 documentation files using deprecated `tb[dtype]().row_major[SIZE]().shared().alloc()` syntax
   - Updated to modern `LayoutTensor[...].stack_allocation()` pattern
   - Status: ✅ Merged
   - https://github.com/modular/mojo-gpu-puzzles/pull/193

2. **PR #194** - GPU Import Path Fixes
   - Fixed 18 instances of deprecated GPU imports across 13 files
   - `gpu.warp` → `gpu.primitives.warp` (16 instances)
   - `gpu.cluster` → `gpu` (2 instances)
   - Affects puzzles 24, 25, 26, 27, 34
   - Status: 🔄 Under Review
   - https://github.com/modular/mojo-gpu-puzzles/pull/194

3. **PR #195** - Deprecation Scanner Tool
   - TOML-driven scanner with 21+ deprecation patterns
   - Python script with uv support
   - Complete documentation and usage examples
   - Status: 🔄 Under Review (optional tool contribution)
   - https://github.com/modular/mojo-gpu-puzzles/pull/195

## Repository Structure

### Branches

- `fix/update-deprecated-layouttensor-syntax` - LayoutTensor fixes (PR #193)
- `fix/update-gpu-import-paths` - GPU import path fixes
- `feature/add-deprecation-scanner` - Scanner tool contribution

### Scanner Tool

Located in `tools/deprecation-scanner/`:

```
tools/deprecation-scanner/
├── README.md                    # Complete documentation
├── scan_deprecations.py         # Scanner implementation (executable)
├── deprecation_patterns.toml    # Pattern database (21+ patterns)
└── PR_DESCRIPTION.md           # Contribution rationale
```

**Usage**:
```bash
# Scan repository for deprecations
uv run tools/deprecation-scanner/scan_deprecations.py --output report.md

# From repository root
cd mojo-gpu-puzzles
uv run tools/deprecation-scanner/scan_deprecations.py
```

## Audit Results

### Deprecation Summary

| Pattern | Category | Instances Found | Severity | Fixed In |
|---------|----------|-----------------|----------|----------|
| LayoutTensor builder syntax | layout | 5 | High | PR #193 |
| gpu.warp imports | gpu_imports | 16 | Medium | GPU Import PR |
| gpu.cluster imports | gpu_imports | 2 | Medium | GPU Import PR |
| **Total** | - | **23** | - | - |

### Patterns Covered in Scanner

The deprecation scanner checks for 21+ patterns across:
- Pointer type removals (DTypePointer, LegacyPointer)
- Layout Tensor API changes
- GPU module reorganisation (8 import path patterns)
- GPU type consolidations
- Python interop changes
- Trait removals/renames
- Language keyword changes (@value, let)
- Type renames (AnyRegType)

All patterns are documented with:
- Regex pattern for detection
- Mojo version when deprecated/removed
- Recommended replacement code
- Severity level (high/medium/low)
- Changelog reference URL

## Methodology

### Phase 1: Manual Discovery
- Identified LayoutTensor builder syntax deprecation while reviewing Puzzle 13
- Searched repository for similar patterns
- Created PR #193 with fixes

### Phase 2: Systematic Scan
- Developed TOML-based configuration system
- Implemented Python scanner with regex matching
- Scanned entire codebase against Mojo 24.0+ changelog
- Discovered additional GPU import path deprecations

### Phase 3: Tool Development
- Refined scanner based on real-world findings
- Added comprehensive documentation
- Prepared for upstream contribution

## Tools & Technologies

- **Language**: Python 3.9+ (scanner), Mojo (target codebase)
- **Dependencies**: `tomli` (TOML parsing, Python < 3.11)
- **Package Manager**: uv (PEP 723 inline dependencies)
- **Format**: TOML (configuration), Markdown (reports)
- **Version Control**: Git with feature branch workflow

## Findings & Impact

### Discovered Issues
- **23 deprecation instances** across multiple Mojo releases
- All verified against official Mojo changelog
- Mix of code (.mojo) and documentation (.md) files

### Code Quality Improvements
- Eliminates deprecation warnings
- Future-proofs codebase for upcoming Mojo releases
- Provides accurate, current examples for learners

### Maintainability
- Scanner tool enables proactive deprecation monitoring
- TOML configuration makes pattern updates trivial
- Can be adapted for other Mojo projects

## References

- [Mojo GPU Puzzles](https://github.com/modular/mojo-gpu-puzzles)
- [Mojo Changelog](https://docs.modular.com/mojo/changelog/)
- [Mojo 25.7 Release Notes](https://docs.modular.com/mojo/changelog/#v257-2024-12-19)
- [Personal Mojo Section](https://www.databooth.com.au/posts/mojo)
- [mojo-dotenv Project](https://github.com/DataBooth/mojo-dotenv)

## Related Work

This audit work complements other Mojo projects by DataBooth:
- **mojo-dotenv**: Modern .env file parser for Mojo
- **mojo-fireplace**: Warm, practical Mojo examples

## License

This audit work and scanner tool are provided under the same license as the original Mojo GPU Puzzles repository (Apache 2.0 or compatible).

## Author

**Michael Booth** - [DataBooth](https://www.databooth.com.au)
- GitHub: [@Mjboothaus](https://github.com/mjboothaus)
- Organization: [@DataBooth](https://github.com/DataBooth)
- LinkedIn: [mjboothaus](https://www.linkedin.com/in/mjboothaus)

**Co-Authored-By**: Warp AI Assistant

## Acknowledgements

- Modular team for the excellent GPU Puzzles educational resource
- Mojo community for feedback and support
- Contributors to the original GPU Puzzles repository

---

*This repository serves as documentation of the audit process and a reference for the contributed scanner tool. The actual fixes and tool have been submitted as PRs to the upstream repository.*
