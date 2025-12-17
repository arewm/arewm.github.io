# Source and Build Provenance Setup

This repository implements supply chain security through both **source provenance** (gittuf) and **build provenance** (SLSA).

## Overview

- **Source Provenance (gittuf)**: Tracks the authenticity and integrity of source code changes
- **Build Provenance (SLSA)**: Generates verifiable attestations for the Jekyll build artifacts

## Source Provenance with gittuf

### What is gittuf?

gittuf provides Git-native source provenance using:
- **in-toto attestations**: Cryptographically signed metadata about code changes
- **TUF (The Update Framework)**: Protects against repository compromise
- **Git integration**: Works with existing Git workflows

### Features

- Tracks all commits with cryptographic signatures
- Enforces policies on branches and files
- Detects unauthorized changes
- Provides audit trail of all repository modifications
- Compatible with existing Git workflows

### Setup Instructions

#### Option 1: Automated Setup (Recommended)

1. Go to Actions → Initialize gittuf → Run workflow
2. Review and merge the generated PR
3. gittuf will be active on your repository

#### Option 2: Manual Setup

```bash
# Install gittuf
mkdir -p $HOME/.local/bin
wget https://github.com/gittuf/gittuf/releases/latest/download/gittuf_linux_amd64 -O $HOME/.local/bin/gittuf
chmod +x $HOME/.local/bin/gittuf
export PATH="$HOME/.local/bin:$PATH"

# Initialize gittuf
gittuf trust init

# Create policy
gittuf policy init
gittuf policy add-rule --rule-name "protect-main" --rule-pattern "refs/heads/main"

# Verify
gittuf verify-ref refs/heads/main
```

### Verification

After setup, verify source provenance:

```bash
# Verify a specific commit
gittuf verify-commit <commit-sha>

# Verify current branch
gittuf verify-ref refs/heads/main

# View attestations
gittuf attest list
```

## Build Provenance with SLSA

### What is SLSA?

SLSA (Supply-chain Levels for Software Artifacts) is a framework for ensuring the integrity of software artifacts throughout the software supply chain.

### What Gets Attested?

For this Jekyll site, the following artifact receives SLSA Level 3 provenance:

- **site.tar.gz**: Compressed archive of the entire _site/ directory containing all generated HTML, CSS, and JavaScript files
- **Build metadata**: Ruby version, Jekyll version, dependencies (Gemfile.lock)
- **Build environment**: GitHub Actions runner details, timestamps, workflow information
- **Source information**: Exact commit SHA, branch name, repository URL

The provenance file cryptographically links the artifact to:
- The exact source code commit that produced it
- The build commands that were executed
- The isolated environment where the build occurred

### Provenance File

After each build, a provenance attestation file is generated:
- **Location**: Attached to GitHub release (for tagged builds) or workflow artifacts
- **Format**: SLSA Provenance v1.0 (JSON)
- **Signature**: Signed with GitHub's Sigstore identity

### Verification

Verify the build provenance using slsa-verifier:

```bash
# Install slsa-verifier
go install github.com/slsa-framework/slsa-verifier/v2/cli/slsa-verifier@latest

# Download the artifacts from GitHub Actions:
# 1. site-artifact.zip (contains site.tar.gz)
# 2. site.intoto.jsonl (the provenance file)

# Extract the artifact
unzip site-artifact.zip

# Verify the provenance
slsa-verifier verify-artifact site.tar.gz \
  --provenance-path site.intoto.jsonl \
  --source-uri github.com/arewm/arewm.github.io
```

Example successful output:
```
Verified signature against tlog entry index 12345678 at URL: https://rekor.sigstore.dev/api/v1/log/entries/...
Verified build using builder https://github.com/slsa-framework/slsa-github-generator/.github/workflows/generator_generic_slsa3.yml@refs/tags/v2.0.0 at commit abc123...
PASSED: Verified SLSA provenance
```

### What SLSA Level 3 Provides

✅ **Build Integrity**: Ensures the build ran in an isolated environment
✅ **Source Tracking**: Links artifacts back to specific source commits
✅ **Non-falsifiable**: Provenance cannot be forged
✅ **Tamper Evidence**: Detects any modifications to artifacts

## Workflow Integration

### deploy-with-provenance.yml

This workflow:
1. Builds the Jekyll site
2. Generates SLSA provenance attestation
3. Deploys to GitHub Pages
4. Uploads provenance as workflow artifact

### How It Works Together

```
Source Code (main branch)
    ↓
gittuf verification (source provenance)
    ↓
GitHub Actions build (isolated environment)
    ↓
Jekyll build produces _site/ artifacts
    ↓
SLSA provenance generated
    ↓
Deploy to GitHub Pages
    ↓
Provenance stored as artifact
```

## Benefits

### For You

- **Supply Chain Security**: Follow best practices you advocate for (SLSA maintainer!)
- **Transparency**: Anyone can verify your build process
- **Compliance**: Meet security requirements for sensitive projects
- **Education**: Demonstrate supply chain security concepts

### For Visitors

- **Trust**: Verify the site they're viewing matches the source code
- **Learning**: See real-world implementation of SLSA/gittuf
- **Confidence**: Know the build process is secure

## Monitoring

### Check Provenance Generation

1. Go to Actions → Build and Deploy with SLSA Provenance
2. Click on a workflow run
3. Look for "provenance" job completion
4. Download artifacts to see the provenance file

### Check gittuf Status

```bash
# Clone your repository
git clone https://github.com/arewm/arewm.github.io

# Verify
gittuf verify-ref refs/heads/main
```

## Troubleshooting

### SLSA Provenance Not Generating

- Ensure `id-token: write` permission is set in workflow
- Verify slsa-github-generator version is v2.0.0 or later
- Check that artifact was uploaded successfully

### gittuf Verification Fails

- Ensure you have the latest gittuf version
- Check that policies are properly configured
- Verify GPG keys if using signed commits

## Additional Resources

- **SLSA Framework**: https://slsa.dev/
- **gittuf Documentation**: https://github.com/gittuf/gittuf
- **GitHub SLSA Generator**: https://github.com/slsa-framework/slsa-github-generator
- **in-toto Attestations**: https://in-toto.io/

## Cost

- **gittuf**: Free, open source
- **SLSA provenance**: Free on GitHub Actions (public repos)
- **Storage**: Minimal (attestation files are small)

## Questions?

As a SLSA maintainer, feel free to reach out if you want to customize these implementations further!
