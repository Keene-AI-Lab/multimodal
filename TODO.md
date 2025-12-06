## Active Tasks

**CONTEXT:** The Multimodal Alzheimer's Detection portfolio entry is already complete and published. This TODO tracks the migration of static diagram images to version-controlled mermaid charts with automated generation, plus optional enhancements using mermaid for data visualizations.

**WORKFLOW:** Tasks are ordered chronologically: verify current state (01), set up mermaid infrastructure (02), convert existing diagrams (03-04), create new visualizations if feasible (05), integrate into portfolio (06), clean up assets (07), and document (08).


## Phase 1: Verification and Setup


### Task 02 — Set Up Mermaid Infrastructure

**Objective:**
Create directory structure and GitHub Actions workflow for automated mermaid diagram generation, following the pattern from ECE-471-Generative-Machine-Learning/Final_Project. This enables version-controlled diagram sources that automatically regenerate images on changes.

**Checklist:**
- [x] Create `assets/diagrams/` directory to store mermaid source files separate from generated images
    - [x] chore: create directory structure for mermaid diagram sources
- [x] Create GitHub Actions workflow `.github/workflows/mermaid-diagrams.yml` that triggers on changes to `assets/diagrams/*.mmd` files
    - [x] build: add GitHub Actions workflow for mermaid diagram generation
- [x] Configure workflow to use mermaid-cli (mmdc) with parameters: transparent background, 1200px width, 800px height, dark theme
    - [x] build: configure mermaid-cli image generation parameters
- [x] Configure workflow to auto-commit generated PNG images to `assets/` directory
    - [x] build: configure auto-commit for generated diagram images
- [ ] Add `.mmd` files to git tracking and update `.gitignore` if needed to ensure both sources and generated PNGs are committed
    - [ ] chore: configure git tracking for mermaid sources and generated images


## Phase 2: Diagram Migration

### Task 03 — Convert Pipeline Flowchart to Mermaid

**Objective:**
Convert `01-pipeline.png` (high-level pipeline flowchart) from static PNG to mermaid source format, enabling easy updates and version control of the diagram logic.

**Checklist:**
- [x] Create `assets/diagrams/01-pipeline.mmd` recreating the pipeline flowchart in mermaid syntax
    - [x] feat: convert pipeline flowchart to mermaid format
- [x] Generate `assets/01-pipeline.png` from mermaid source using mermaid-cli
    - [x] build: generate pipeline diagram from mermaid source
- [x] Verify generated PNG renders correctly in README and has proper styling (dark theme, labels, legend)
    - [x] test: verify generated pipeline diagram quality


### Task 04 — Convert Feature Extraction Diagram to Mermaid

**Objective:**
Convert `02-feature-extraction.png` (feature extraction pipeline detail) from static PNG to mermaid source format, following the same workflow as the pipeline flowchart.

**Checklist:**
- [x] Create `assets/diagrams/02-feature-extraction.mmd` recreating the feature extraction flow in mermaid syntax
    - [x] feat: convert feature extraction diagram to mermaid format
- [x] Generate `assets/02-feature-extraction.png` from mermaid source using mermaid-cli
    - [x] feat: generate feature extraction diagram from mermaid source
- [ ] Verify generated PNG renders correctly in README with proper styling
    - [ ] test: verify generated feature extraction diagram quality



## Phase 3: Integration and Cleanup

### Task 06 — Update README with Mermaid-Generated Figures

**Objective:**
Update README.md to reference mermaid-generated figures and document the mermaid-based diagram workflow for future maintainers.

**Checklist:**
- [ ] Verify all mermaid-generated PNGs display correctly in README.md (no broken references)
    - [ ] test: verify mermaid-generated figures display in readme
- [ ] Add note to README documenting that flowchart diagrams are generated from mermaid sources in `assets/diagrams/`
    - [ ] docs: document mermaid diagram workflow in readme
- [ ] Update figure captions if needed to reflect any visual changes from mermaid conversion
    - [ ] docs: update figure captions if needed
- [ ] Test README rendering on GitHub to ensure all images load correctly
    - [ ] test: verify readme renders correctly on github


### Task 07 — Clean Up Assets and Standardize Organization

**Objective:**
Clean up asset files, archive original static PNGs if replaced by mermaid-generated versions, and ensure consistent naming conventions across the repository.

**Checklist:**
- [ ] Create `assets/archive/` directory for original static PNGs that have been replaced by mermaid-generated versions
    - [ ] chore: create archive directory for original static figures
- [ ] Move original 01-pipeline.png and 02-feature-extraction.png to archive if successfully replaced by mermaid versions
    - [ ] chore: archive original static diagrams replaced by mermaid
- [ ] Verify all figure files follow consistent naming convention (01-description, 02-description, etc.)
    - [ ] chore: standardize figure naming conventions
- [ ] Remove any unused or duplicate figure files from assets directory
    - [ ] chore: clean up unused figure files
- [ ] Update project directory structure documentation if significant changes were made
    - [ ] docs: update directory structure documentation


## Phase 4: Documentation

### Task 08 — Document Mermaid Workflow and Maintenance

**Objective:**
Create comprehensive documentation explaining the mermaid diagram workflow, making it easy for future maintainers to update diagrams and understand the automated generation process.

**Checklist:**
- [ ] Create `DIAGRAMS.md` documenting the mermaid workflow: where sources live, how to edit, how auto-generation works
    - [ ] docs: create mermaid workflow documentation
- [ ] Document mermaid syntax patterns used for each diagram type (flowcharts, bar charts if used)
    - [ ] docs: document mermaid syntax patterns for each diagram
- [ ] Document GitHub Actions workflow configuration and parameters (mmdc settings, triggers, auto-commit behavior)
    - [ ] docs: document github actions mermaid workflow
- [ ] Add troubleshooting section for common issues (workflow failures, PNG generation errors, theme/styling problems)
    - [ ] docs: add troubleshooting guide for mermaid workflow
- [ ] Link to mermaid diagram documentation from main README.md
    - [ ] docs: add link to diagram documentation from readme
