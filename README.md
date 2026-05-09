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
│   ├── organizers.md     # Front matter template for new organizers
│   ├── speakers.md       # Front matter template for new speakers
│   └── resources.md      # Front matter template for new resources
├── content/
│   ├── _index.md         # Homepage hero
│   ├── about/            # About the group
│   ├── events/           # Meetup listings (upcoming + past)
│   ├── organizers/       # Organizer profiles
│   ├── resources/        # Slides, recordings, code from sessions
│   └── speakers/         # Speaker profiles
├── layouts/
│   ├── organizers/
│   │   └── single.html   # Profile page layout (supports photo field)
│   └── speakers/
│       └── single.html   # Profile page layout (supports photo field)
├── static/
│   └── img/
│       ├── organizers/   # Profile photos for organizers
│       └── speakers/     # Profile photos for speakers
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
| `photo` | Path to profile photo (see [Adding a profile photo](#adding-a-profile-photo)) |
| `tags` | Topic areas |

### Organizer front matter fields

| Field | Description |
|---|---|
| `title` | Organizer's full name |
| `description` | One-line bio summary |
| `github` / `linkedin` | Handle (not full URL) |
| `company` | Current employer |
| `photo` | Path to profile photo (see [Adding a profile photo](#adding-a-profile-photo)) |
| `tags` | Topic areas |

### Adding a profile photo

Profile photos appear as a circular avatar next to the person's name on their individual page. Supported for both organizers and speakers.

**Step 1 — Prepare the image**

Use a square image for best results (the browser crops it to a circle). JPG or PNG, minimum 256×256 px recommended.

**Step 2 — Place the file in the correct folder**

| Page type | Folder | Example filename |
|---|---|---|
| Organizer | `static/img/organizers/` | `stephen-valdinger.jpg` |
| Speaker | `static/img/speakers/` | `jane-smith.jpg` |

Name the file to match the person's content file (replace spaces with hyphens, all lowercase).

**Step 3 — Set the `photo` field in the front matter**

Open the person's content file and set `photo` to the path of the image:

```yaml
# For an organizer — content/organizers/stephen-valdinger.md
photo: "/img/organizers/stephen-valdinger.jpg"

# For a speaker — content/speakers/jane-smith.md
photo: "/img/speakers/jane-smith.jpg"
```

**Step 4 — Verify locally**

Run `hugo server -D` and open the person's page. The photo should appear as a circle to the left of their name.

> If the photo does not appear, double-check that the filename in `static/` matches exactly (including case) what is set in the `photo` field.

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
