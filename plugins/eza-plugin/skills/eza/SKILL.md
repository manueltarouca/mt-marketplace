---
name: eza
description: Modern replacement for ls with colors, icons, git status, and tree views. Use when listing files, exploring directories, viewing file metadata, or when the user mentions eza, ls alternatives, directory listings, or file exploration.
allowed-tools: Bash
---

# eza - Modern ls Alternative

A modern replacement for `ls` with colors, icons, Git integration, and tree views.

## When to Use This Skill

- Listing files and directories with enhanced visualization
- Exploring directory structures with tree views
- Viewing file metadata (permissions, sizes, timestamps)
- Checking Git status alongside file listings
- Any time the user mentions eza, ls alternatives, or wants better directory visualization

## Quick Reference

### Basic Display Modes

```bash
# Grid view (default)
eza

# One file per line
eza -1

# Long format with metadata
eza -l

# Tree view
eza -T

# Grid + long details
eza -lG
```

## IMPORTANT: Output Capture Issue with Bash Tool

**Known Issue**: When running eza through Claude Code's Bash tool, output may not be captured correctly for some command combinations, resulting in empty output even though the command executes successfully (exit code 0).

**Root Cause**: Eza detects when stdout is not a real terminal (TTY) and adjusts its behavior. When output is piped or redirected through automation tools, certain flag combinations may produce no output.

**Working Solution**: Use `--git-ignore` flag or specify an explicit path with `.` to ensure output is generated:

```bash
# These commands work reliably in automated contexts:
eza -T --git-ignore .
eza -T --git-ignore
eza --git-ignore -l
eza . -l

# These may produce no output when run through automation:
eza
eza -1
eza -lh
eza -T
```

**Best Practice**: When executing eza through the Bash tool, ALWAYS include either:
1. The `--git-ignore` flag, OR
2. An explicit path argument (even if just `.`)

This ensures consistent output capture regardless of the environment.

### Common Use Cases

```bash
# Long format with header, icons, and git status
eza -lh --icons --git

# Tree view with 3 levels depth
eza -T -L 3

# Show all files including hidden (twice for . and ..)
eza -aa

# Sort by size (largest first) with reverse
eza -lrs size

# Sort by modification time (newest first)
eza -lrs modified

# Show only directories
eza -D

# Show only files
eza -f

# Recursive listing
eza -R

# Long format with all metadata
eza -l --header --inode --links --modified --created --accessed
```

## Display Options

### Basic Views

- `-1`, `--oneline`: One entry per line
- `-G`, `--grid`: Grid view (default)
- `-l`, `--long`: Extended details table
- `-T`, `--tree`: Recursive tree view
- `-R`, `--recurse`: Recurse into directories
- `-x`, `--across`: Sort grid across instead of down

### Visual Enhancements

- `--icons[=WHEN]`: Display icons (always/auto/never)
- `--color[=WHEN]`: Use colors (always/auto/never)
- `--color-scale[=FIELD]`: Highlight levels (all, age, size)
- `--color-scale-mode[=MODE]`: Gradient or fixed colors
- `-F`, `--classify[=WHEN]`: Show file type indicators
- `--hyperlink`: Display as hyperlinks

### Path Display

- `--absolute[=MODE]`: Show absolute paths (on/follow/off)
  - `on`: Show absolute paths
  - `follow`: Resolve symlinks to targets
  - `off`: Show relative paths (default)

## Filtering and Sorting

### Show/Hide Files

- `-a`, `--all`: Show hidden files (use twice for . and ..)
- `-A`, `--almost-all`: Equivalent to --all
- `-d`, `--treat-dirs-as-files`: List dir metadata, not contents
- `-D`, `--only-dirs`: List only directories
- `-f`, `--only-files`: List only files
- `--show-symlinks`: Show symlinks with --only-files/--only-dirs
- `--no-symlinks`: Hide symbolic links

### Filtering

- `-L`, `--level=DEPTH`: Limit recursion depth
- `-I`, `--ignore-glob=GLOBS`: Ignore patterns (pipe-separated)
- `--git-ignore`: Respect .gitignore

### Sorting

- `-s`, `--sort=FIELD`: Sort by field
  - Fields: `name`, `Name`, `size`, `modified`, `accessed`, `created`, `extension`, `Extension`, `inode`, `type`, `none`
  - Aliases: `modified` = `date`/`time`/`newest`, reverse = `age`/`oldest`
  - Capital letter fields sort uppercase before lowercase
- `-r`, `--reverse`: Reverse sort order
- `--group-directories-first`: Dirs before files
- `--group-directories-last`: Dirs after files

## Long View Options

Available with `-l`/`--long`:

### Size and Space

- `-b`, `--binary`: Binary prefixes for sizes
- `-B`, `--bytes`: Sizes in bytes only
- `-S`, `--blocksize`: Show allocated filesystem blocks
- `--total-size`: Show recursive directory size (Unix only)
- `--no-filesize`: Hide file size field

### Timestamps

- `-t`, `--time=FIELD`: Which timestamp (modified/changed/accessed/created)
- `-m`, `--modified`: Use modified timestamp (default)
- `-u`, `--accessed`: Use accessed timestamp
- `-U`, `--created`: Use created timestamp
- `--changed`: Use changed timestamp
- `--time-style=STYLE`: Format timestamps
  - Styles: `default`, `iso`, `long-iso`, `full-iso`, `relative`
  - Custom: `+<FORMAT>` (e.g., `+%Y-%m-%d %H:%M`)
- `--no-time`: Hide time field

### Ownership and Permissions

- `-g`, `--group`: Show file group
- `--smart-group`: Show group only if different from owner
- `-n`, `--numeric`: Show numeric user/group IDs
- `--no-permissions`: Hide permissions field
- `-o`, `--octal-permissions`: Show octal permissions
- `--no-user`: Hide user field

### Metadata

- `-h`, `--header`: Add column headers
- `-H`, `--links`: Show hard link count
- `-i`, `--inode`: Show inode number
- `-@`, `--extended`: Show extended attributes
- `-Z`, `--context`: Show security context
- `-M`, `--mounts`: Show mount details (Linux/Mac)
- `-O`, `--flags`: Show file flags (Mac/BSD) or attributes (Windows)

### Git Integration

- `--git`: Show Git status for files
  - `-` = not modified, `M` = modified, `N` = new, `D` = deleted
  - `R` = renamed, `T` = type-change, `I` = ignored, `U` = conflicted
- `--git-repos`: Show Git repo status for directories
  - `|` = clean, `+` = dirty, `~` = unknown
- `--git-repos-no-status`: Show if dir is Git repo without status
- `--no-git`: Disable Git status (overrides all git options)

### Other

- `-X`, `--dereference`: Dereference symlinks for info
- `--follow-symlinks`: Follow symlinks into directories
- `--stdin`: Read file names from stdin (use `EZA_STDIN_SEPARATOR` env var)

## Practical Examples

### Development Workflow

```bash
# Project overview with git status
eza -lh --git --icons --group-directories-first

# Find largest files
eza -lhS --reverse

# Recent modifications
eza -lh --sort=modified --reverse

# Deep tree exploration (3 levels)
eza -T -L 3 --git-repos --icons

# All metadata for debugging
eza -lh --header --inode --links --extended --git
```

### Specific Scenarios

```bash
# Explore Java project structure
eza -T -L 4 -D --icons

# Find all Java files with metadata
eza -lh --icons *.java

# Check service directories
eza -D -1 *-service/

# View migration files chronologically
eza -1 --sort=name src/main/resources/db/migration/

# Inspect configuration files
eza -lh --icons *.yml *.yaml *.properties

# Check for hidden config files
eza -a -f -1
```

### Performance and Size Analysis

```bash
# Show directory sizes
eza -lh --total-size -D

# Binary sizes with blocks
eza -lbS --blocksize

# Find oldest files
eza -lh --sort=age

# Color-coded by size
eza -lh --color-scale=size --icons
```

## Environment Variables

### `COLUMNS`
Override terminal width: `COLUMNS=120 eza`

### `EZA_COLORS`
Customize color scheme (see eza_colors man page)

### `EZA_ICONS_AUTO`
Always enable icons: `export EZA_ICONS_AUTO=1`

### `EZA_GRID_ROWS`
Minimum rows for grid-details view

### `EZA_ICON_SPACING`
Spaces between icons and filenames (adjust for your terminal)

### `EZA_MIN_LUMINANCE`
Minimum luminance for color-scale (-100 to 100)

### `EZA_CONFIG_DIR`
Config directory (defaults to `~/.config/eza`)

### `EZA_OVERRIDE_GIT`
Override --git/--git-repos arguments

### `EZA_STDIN_SEPARATOR`
Separator for stdin input (default: newline)

### `NO_COLOR`
Disable colors: `export NO_COLOR=1`

## Instructions

When the user asks to list files, explore directories, or check file details:

1. **Assess the context**: Determine what information is most relevant
2. **Choose appropriate flags**:
   - Basic listing: `eza --git-ignore` or `eza -1 .`
   - Detailed info: `eza -lh --icons --git-ignore`
   - Tree view: `eza -T -L <depth> --git-ignore`
   - With git: add `--git` or `--git-repos` AND `--git-ignore`
3. **Use sorting**: Apply `--sort` and `--reverse` as needed
4. **Filter appropriately**: Use `-D`/`-f`, `--only-dirs`/`--only-files`, or `-I` for filtering
5. **CRITICAL: Ensure output capture**: Always include either `--git-ignore` flag OR an explicit path (like `.`) to guarantee output is captured by the Bash tool
6. **Execute with Bash tool**: Run the eza command
7. **Present results**: Show output to user with context

## Exit Status Codes

- `0`: Success
- `1`: I/O error
- `3`: Invalid command-line arguments
- `13`: Permission denied

## Additional Resources

- Man pages: `man eza`, `man eza_colors`
- Source: https://github.com/eza-community/eza
- Color customization: See eza_colors(5) man page
