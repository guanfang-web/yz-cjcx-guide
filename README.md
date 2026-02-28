# guide-pages

Markdown homepage project for GitHub Pages user guide and FAQ.

## Local content

- `index.md`: guide homepage (text + images + claim-extra widget with order number input)
- `assets/`: screenshots used by docs
- `_config.yml`: Jekyll config for GitHub Pages

## Deploy to a new GitHub Pages repository

1. Create a new GitHub repository, for example: `yz-cjcx-guide`.
2. Upload the files under this `guide-pages` directory to the root of that repository.
3. In GitHub repository settings, enable Pages:
   - Build and deployment: `Deploy from a branch`
   - Branch: `main` and folder `/ (root)`
4. Wait for deployment and open the generated Pages URL.

## API base for code claim widget

Open the page with query parameter:

```txt
https://<your-pages-domain>/?apiBase=https://<your-api-domain-or-tunnel>
```

or fill API address directly in the input box on the page.
