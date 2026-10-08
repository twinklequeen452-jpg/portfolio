# Sonu Kumari — Portfolio

A personal portfolio website for a front-end developer, served with Flask. Single-page layout with a sticky side rail, and sections for profile, skills, projects, and contact.

## Features

- Sticky side navigation with smooth-scroll anchors
- Sections: Hero, Profile, Skills, Projects, Contact
- Downloadable resume (PDF embedded in the page)
- Responsive-friendly, editorial design using the Fraunces and Inter fonts
- Respects `prefers-reduced-motion`
- Visitor counter placeholder (needs a backend route, see [Known issues](#known-issues))

## Tech stack

| Layer     | Tools                              |
|-----------|------------------------------------|
| Frontend  | HTML, CSS, vanilla JavaScript      |
| Backend   | Python, Flask                      |
| Fonts     | Google Fonts (Fraunces, Inter)     |

## Project structure

Flask expects templates and static files in specific folders:

```
portfolio/
├── app.py
├── templates/
│   └── index.html      # the portfolio HTML file, renamed
└── static/
    └── style.css
```

Rename `portfolio-professional (3).html` to `index.html` and place it in `templates/`. Move `style.css` into `static/`.

## Getting started

**Requirements:** Python 3.8+

```bash
# 1. Create and activate a virtual environment (optional but recommended)
python -m venv venv
source venv/bin/activate        # Windows: venv\Scripts\activate

# 2. Install Flask
pip install flask

# 3. Run the app
python app.py
```

Open http://127.0.0.1:5000 in your browser.

## Customizing

- **Content:** edit the text in `templates/index.html` (hero, about, skills, projects, contact).
- **Colors:** change the CSS variables at the top of `static/style.css`:

  ```css
  :root{
    --bg:#F7F5F1;
    --ink:#1C1F26;
    --slate:#3E5C76;
    --gold:#A9814B;
  }
  ```

- **Projects:** duplicate a `<div class="project">` block and update the title, description, stack chips, status, and link.
- **Resume:** the PDF is embedded as base64 in the HTML. Replace it, or host the PDF in `static/` and point the two download links at `{{ url_for('static', filename='resume.pdf') }}`.

## Known issues

These are worth fixing before deploying:

1. **Stylesheet path.** `href="style.css"` won't resolve under Flask. Use:
   ```html
   <link rel="stylesheet" href="{{ url_for('static', filename='style.css') }}">
   ```
2. **Visitor counter has no backend.** The page calls `http://127.0.0.1:5000/view`, but `app.py` has no `/view` route. Add one that returns JSON, for example:
   ```python
   from flask import jsonify

   views = 0

   @app.route('/view')
   def view():
       global views
       views += 1
       return jsonify(views=views)
   ```
   Also change the fetch URL to the relative path `/view` so it works once deployed.
3. **Stray lines in `<head>` / `<body>`.** Remove the broken script lines (`docutement.getElementBYID...`, `<script scr="package.json">`, `<script scr="credentials.json">`), the duplicate `<title>`, and the unclosed `<iframe`. Referencing `credentials.json` in HTML is also a security risk, so never expose credential files to the browser.
4. **Project links** currently point to `#`. Replace them with real URLs.
5. **GitHub link** points to `https://github.com/`. Replace it with your profile URL.

## Deployment

`debug=True` is for development only. For production, turn it off and serve with a WSGI server such as Gunicorn:

```bash
pip install gunicorn
gunicorn app:app
```

## License

Personal portfolio. All rights reserved © 2026 Sonu Kumari.
