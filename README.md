# Action Channel

A documentation repository featuring streaming architecture diagrams and visualizations, built with Mermaid and deployed automatically to GitHub Pages.

## 🌟 Overview

This repository contains comprehensive documentation and diagrams for streaming architecture patterns, domain models, and system flows. All diagrams are created using Mermaid syntax and are automatically rendered and deployed to GitHub Pages through GitHub Actions.

## 📖 Documentation

Visit our **GitHub Pages site**: [https://heruwala.github.io/action-channel](https://heruwala.github.io/action-channel)

## 📁 Repository Structure

```
action-channel/
├── .github/
│   └── workflows/
│       └── deploy-pages.yml          # GitHub Actions workflow for automated deployment
├── docs/
│   ├── index.md                      # Main documentation landing page
│   └── streamy-diagrams.md          # Streaming architecture diagrams and models
└── README.md                         # This file
```

### Folder Descriptions

- **`.github/workflows/`** - Contains GitHub Actions automation workflows
  - `deploy-pages.yml` - Automates the conversion of Mermaid diagrams to static content and deploys to GitHub Pages

- **`docs/`** - Documentation source files
  - `index.md` - Main entry point for the documentation site
  - `streamy-diagrams.md` - Collection of Mermaid diagrams including:
    - Domain models for streaming platforms
    - Sequence diagrams for user workflows
    - System architecture visualizations

## 🚀 Features

- **Mermaid Diagrams**: Interactive and version-controlled diagrams
- **Automated Deployment**: GitHub Actions automatically builds and deploys changes
- **GitHub Pages Integration**: Live documentation site with clean URLs
- **Streaming Architecture Focus**: Specialized content for streaming platform design

## 🔧 How It Works

1. **Content Creation**: Diagrams are written in Mermaid syntax within Markdown files
2. **Automatic Processing**: On push to main branch, GitHub Actions workflow triggers
3. **Deployment**: The workflow converts Mermaid diagrams and deploys to GitHub Pages
4. **Live Site**: Updated documentation is immediately available at the GitHub Pages URL

## 📋 Available Documentation

- **Domain Models**: Entity relationships for streaming platforms
- **Sequence Diagrams**: User interaction flows and system processes
- **Architecture Patterns**: Visual representations of streaming system designs

## 🤝 Contributing

1. Fork the repository
2. Create your feature branch (`git checkout -b feature/new-diagram`)
3. Add or modify Mermaid diagrams in the `docs/` folder
4. Commit your changes (`git commit -am 'Add new streaming diagram'`)
5. Push to the branch (`git push origin feature/new-diagram`)
6. Open a Pull Request

## 🔗 Links

- **Live Documentation**: [https://heruwala.github.io/action-channel](https://heruwala.github.io/action-channel)
- **Mermaid Documentation**: [https://mermaid.js.org/](https://mermaid.js.org/)
- **GitHub Pages**: [https://pages.github.com/](https://pages.github.com/)

---

*This repository demonstrates the power of combining Mermaid diagrams with GitHub Actions for automated documentation deployment.*
