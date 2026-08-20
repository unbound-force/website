## ADDED Requirements

### Requirement: RPM installation section on project page

The Dewey project page (`content/docs/projects/dewey.md`) MUST include an RPM installation section between the Homebrew and "install from source" sections. The section MUST document:
- Downloading the `.rpm` package from [GitHub Releases](https://github.com/unbound-force/dewey/releases)
- The `sudo dnf install` command for installing the package
- That both `amd64` and `arm64` architectures are available
- A `<version>` placeholder in filenames (not a hardcoded version)

#### Scenario: User reads Dewey project page installation section
- **GIVEN** a user visits the Dewey project page
- **WHEN** they read the Installation section
- **THEN** they see three installation methods in order: Homebrew, RPM (Linux), and install from source

#### Scenario: RPM section content on project page
- **GIVEN** a user reads the RPM installation section on the project page
- **WHEN** they follow the documented commands
- **THEN** they can download and install the `.rpm` package using `sudo dnf install`

### Requirement: RPM installation section on getting-started page

The Dewey getting-started page (`content/docs/getting-started/knowledge.md`) MUST include an RPM installation section immediately after the Homebrew code block, before the embedding model explanation paragraph. The section MUST document:
- Downloading the `.rpm` package from [GitHub Releases](https://github.com/unbound-force/dewey/releases)
- The `sudo dnf install` command for installing the package
- That both `amd64` and `arm64` architectures are available
- That the binary installs to `/usr/bin/dewey`
- A `<version>` placeholder in filenames (not a hardcoded version)

#### Scenario: User reads getting-started page installation section
- **GIVEN** a user visits the Knowledge Retrieval with Dewey getting-started page
- **WHEN** they read the Installation section
- **THEN** they see Homebrew, RPM (Linux), and source install methods in that order

#### Scenario: RPM section content on getting-started page
- **GIVEN** a user reads the RPM installation section on the getting-started page
- **WHEN** they follow the documented commands
- **THEN** they can download and install the `.rpm` package with architecture and install path information

### Requirement: Content accuracy sourced from upstream

All RPM installation instructions MUST be derived from the upstream Dewey README (dewey#59). The documented commands, URLs, and paths MUST match the upstream documentation.

#### Scenario: Content matches upstream README
- **GIVEN** the upstream Dewey README documents RPM installation
- **WHEN** the website RPM sections are compared to the upstream README
- **THEN** the commands, URL patterns, and install paths are consistent

## MODIFIED Requirements

### Requirement: Source install section heading on project page

The existing "On Linux, install from source:" text in `content/docs/projects/dewey.md` MUST be updated to "Or install from source:" since RPM is also a Linux installation method and the existing phrasing implies source install is the only Linux option.

#### Scenario: Source install heading is platform-neutral
- **GIVEN** a user reads the Dewey project page after the RPM section is added
- **WHEN** they reach the source install section
- **THEN** the heading reads "Or install from source:" (not "On Linux, install from source:")

## REMOVED Requirements

None.
