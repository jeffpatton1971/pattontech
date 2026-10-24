# Patton-Tech

Jekyll source for [patton-tech.com](https://patton-tech.com), using Minimal Mistakes 4.28.0 and its dark skin.

## Local development

Install Ruby with Bundler (RubyInstaller with Devkit on Windows), then:

```text
bundle install
bundle exec jekyll build --strict_front_matter
bundle exec ruby scripts/check-portfolio.rb
bundle exec jekyll serve --host 127.0.0.1
```

## Projects

Edit `_projects/<slug>.md` to update a project. Keep its filename stable: the collection generates `/projects/<slug>/`. Metadata controls the landing-page group (`featured` or `earlier`), order, description, technologies, and resources. `prominent: true` identifies a lead case study.

The landing page is `projects/index.md`; the shared layout is `_layouts/project.html`. The portfolio uses the existing theme plus a small scoped stylesheet. Blog and series content remain in their original locations.

See [portfolio decisions and evidence](docs/projects-portfolio.md) for repository links, URL preservation, confidentiality boundaries, and editorial follow-ups. Documentation and validation scripts are excluded from the generated site.

Build output and local tool caches are ignored. Local builds do not publish the site; retain the existing hosting workflow and obtain explicit authorization before pushing or deploying.
