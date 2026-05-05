# psoh.io

Source for the **[PowerShell Ohio User Group](https://psoh.io)** website, built with [Hugo](https://gohugo.io/) and the [Blowfish](https://blowfish.page/) theme.

---

## Prerequisites

| Tool | Version | Install |
|---|---|---|
| [Hugo Extended](https://gohugo.io/installation/) | ≥ 0.157.0 | `choco install hugo-extended` |
| [Go](https://go.dev/dl/) | ≥ 1.22 | Required for Hugo module support |
| [Git](https://git-scm.com/) | any | – |

## Local development

```powershell
git clone https://github.com/PSOhio/psoh.io.git
cd psoh.io
hugo server -D
```

The site will be available at `http://localhost:1313`. The `-D` flag includes draft content.

## Project structure

```
psoh.io/
├── assets/
│   └── img/              # Logo and image assets
├── config/_default/
│   ├── hugo.toml         # Base Hugo configuration
│   ├── languages.en.toml # Site title, description, author, logo
│   ├── markup.toml       # Goldmark / syntax highlight settings
│   ├── menus.en.toml     # Top nav and footer navigation
│   ├── module.toml       # Blowfish theme module import
│   └── params.toml       # Blowfish theme parameters
├── archetypes/
│   ├── events.md         # Front matter template for new events
│   ├── speakers.md       # Front matter template for new speakers
│   └── resources.md      # Front matter template for new resources
├── content/
│   ├── _index.md         # Homepage hero
│   ├── about/            # About the group
│   ├── events/           # Meetup listings (upcoming + past)
│   ├── resources/        # Slides, recordings, code from sessions
│   └── speakers/         # Speaker profiles
└── .github/workflows/
    └── hugo.yml          # GitHub Pages deployment (on PR merge to main)
```

## Adding content

Use the Hugo archetypes to scaffold new content with the correct front matter:

```powershell
# New event
hugo new content events/2026-06-june-meetup.md

# New speaker profile
hugo new content speakers/firstname-lastname.md

# New session resource
hugo new content resources/2026-06-session-title.md
```

Remove `draft = true` from the front matter when the content is ready to publish.

### Event front matter fields

| Field | Description |
|---|---|
| `title` | Event name |
| `date` | Event date/time (used for sorting) |
| `location` | Venue name and city |
| `rsvp_url` | Link to RSVP (e.g. Meetup.com event) |
| `is_upcoming` | `true` for upcoming events, `false` for past |
| `tags` | Topic tags |

### Speaker front matter fields

| Field | Description |
|---|---|
| `title` | Speaker's full name |
| `description` | One-line bio summary |
| `github` / `linkedin` | Handle (not full URL) |
| `company` | Current employer |
| `tags` | Topic areas |

### Resource front matter fields

| Field | Description |
|---|---|
| `title` | Session title |
| `speaker` | Speaker name |
| `event_date` | Human-readable date (e.g. `"June 19, 2026"`) |
| `slides_url` | Link to slide deck |
| `video_url` | Link to recording |
| `tags` | Topic tags |

## Navigation

Header and footer links are defined in `config/_default/menus.en.toml`.

Each entry follows this structure:

```toml
[[main]]          # 'main' = header nav; 'footer' = footer nav
  name = "Label"  # link text displayed in the nav
  pageRef = "/path" # links to a content page at content/path/
  # url = "https://..." # use 'url' instead of 'pageRef' for external links
  weight = 10     # controls order (lower = further left/up)
```

To **add a link**: append a new `[[main]]` or `[[footer]]` block and set `weight` to position it.

To **remove a link**: delete the corresponding block.

To **reorder links**: adjust the `weight` values (links are sorted ascending).

After editing, run `hugo server -D` and check the header/footer to confirm the change.

## Theme

The site uses [Blowfish v2](https://blowfish.page/) installed as a Hugo module. To update:

```powershell
hugo mod get -u
hugo mod tidy
```

## Deployment

The site deploys automatically to **GitHub Pages** when a pull request is merged into `main`. The workflow can also be triggered manually from the **Actions** tab.

Branch protection is enforced on `main` — all changes must go through a pull request.

## Contributing

1. Fork or create a branch
2. Make your changes and test locally with `hugo server -D`
3. Open a pull request against `main`

For questions or to propose a talk, open an issue or reach out via [GitHub Discussions](https://github.com/PSOhio/psoh.io/discussions).
