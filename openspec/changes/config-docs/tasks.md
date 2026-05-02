## 1. Create Reference section (if not already present) and configuration page

- [x] 1.1 Create `content/docs/reference/_index.md` section index with frontmatter (title: "Reference", weight: 10) if it does not already exist (may already exist from `cli-reference-and-positioning` PR #73)
- [x] 1.2 Add `[[docs]]` entry for "Reference" in `config/_default/menus/menus.en.toml` if not already present
- [x] 1.3 Read upstream source files (`unbound-force/unbound-force/internal/config/config.go`, `template.go`, `validate.go`) and run `uf config --help` to verify current command surface, sections, and precedence order before writing content
- [x] 1.4 Create `content/docs/reference/config.md` with required frontmatter (title: "Configuration", description, weight for sidebar ordering)
- [x] 1.5 Write introduction section: what `uf config` does, why unified configuration matters, default behavior when no config exists

## 2. Document command reference

- [x] 2.1 Write command reference section: `init` (creates/updates `.uf/config.yaml`), `show` (displays merged config with `--format text|json`), `validate` (schema validation with `--format text|json`) — with copy-pasteable usage examples and flags

## 3. Document layered loading

- [x] 3.1 Write layered loading section: 5-tier precedence (CLI flags > env vars > repo config > user config > compiled defaults) with clear "wins" indicator
- [x] 3.2 Include precedence examples showing how env vars override repo config for specific keys, and note that missing config files produce defaults with no error

## 4. Document configuration sections

- [x] 4.1 Write section overview table: all 7 sections (setup, scaffold, embedding, sandbox, gateway, doctor, workflow) with purpose and 1-2 most commonly customized settings per section
- [x] 4.2 Write common customization examples as YAML code blocks: package manager (`setup.package_manager: dnf`), embedding model, gateway port (`gateway.port: 9090`), skip list (`setup.skip: [gaze, mxf]`)

## 5. Update common-workflows.md and add cross-references

- [x] 5.1 Add references to `uf config init` and `uf config validate` commands in the workflow configuration subsection of `content/docs/getting-started/common-workflows.md`
- [x] 5.2 Add link to the full configuration docs page from common-workflows.md
- [x] 5.3 Add cross-references section at the end of the config page linking to Common Workflows, Developer Guide, Quick Start, and CLI Reference

## 6. Verification

- [x] 6.1 Run `npm run build` and confirm no build errors
- [ ] 6.2 Run `npm run dev` and verify config page renders correctly with all sections
- [x] 6.3 Verify page appears in Reference section navigation
- [ ] 6.4 Verify both light and dark mode rendering
- [x] 6.5 Verify all internal links resolve (no dead links)
<!-- spec-review: passed -->
<!-- code-review: passed -->
