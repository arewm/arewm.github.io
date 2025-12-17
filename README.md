# arewm.github.io

Personal website for Andrew McNamara, hosted on GitHub Pages.

## About

This is a personal website built with Jekyll and hosted on GitHub Pages. It will be accessible at both:
- `https://arewm.github.io` (GitHub Pages default)
- `https://arewm.com` (custom domain with SSL)

## Features

- Clean, modern design with responsive layout
- About page with professional information
- Links to presentations and GitHub projects
- SSL/HTTPS enabled through GitHub Pages
- Built with Jekyll for easy content management
- **PR Preview Deployments**: Automatic preview URLs for pull requests (see [PR_PREVIEW_SETUP.md](PR_PREVIEW_SETUP.md))

## Local Development

To run this site locally:

```bash
# Install dependencies
bundle install

# Run the development server
bundle exec jekyll serve

# Visit http://localhost:4000
```

## Deployment

The site automatically deploys to GitHub Pages when changes are pushed to the main branch.

### PR Preview Deployments

Pull requests automatically get preview deployments with unique URLs. See [PR_PREVIEW_SETUP.md](PR_PREVIEW_SETUP.md) for setup instructions.

### Custom Domain Setup

See [SSL_SETUP.md](SSL_SETUP.md) for detailed instructions on configuring the custom domain and SSL certificate.

## Content

- **Home**: Main landing page with introduction
- **About**: Detailed information about professional work and interests
- **Presentations**: Links to technical presentations on GitHub

## Technology Stack

- **Jekyll**: Static site generator
- **GitHub Pages**: Hosting platform
- **Minimal Mistakes**: Professional Jekyll theme
- **Let's Encrypt**: SSL certificates (automatic through GitHub Pages)
- **GitHub Copilot**: AI assistance for site generation

## Acknowledgments

This website was generated with assistance from GitHub Copilot.

## Links

- [Presentations Repository](https://github.com/arewm/presentations)
- [GitHub Profile](https://github.com/arewm)

## License

© 2025 Andrew McNamara. All rights reserved.
